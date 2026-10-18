# Parte 1: identificação dos cortes

**Objetivo:** de um episódio, chegar a uma lista de cortes aprovados (início, fim, título, nota).
**Entrada:** arquivo de vídeo do episódio.
**Saída:** `08_cortes.json`.

Status: 🟡 regras detalhadas. As marcadas com 🟡 serão revisadas quando você passar as
referências do que é interessante (Q5 e D30).
Os nomes em `MAIÚSCULAS` são parâmetros; os valores ficam em [02-parametros.md](02-parametros.md).
Legenda: ✅ Decidido · 🟡 Proposto · 🧪 A testar · ❓ Aberto

## Fluxo

```
vídeo do episódio
 ├─ 1.1 extrair áudio        → 01_audio.wav
 ├─ 1.2 transcrever          → 02_palavras.json
 ├─ 1.3 diarizar             → 03_falantes.json
 ├─ 1.4 montar frases        → 04_frases.json
 ├─ 1.5 dividir em blocos       (em memória)
 ├─ 1.6 gerar candidatos     → 05_candidatos.json      (Qwen local)
 ├─ 1.7 filtrar              → 06_filtrados.json       (Python, sem IA)
 │      ── pausa: o pipeline local para aqui ──
 ├─ 1.8 julgar               → 07_notas.json           (Claude, sessão do Claude Code)
 ├─ 1.9 ranquear             → 08_cortes.json          (Claude agrupa assuntos + Python ordena)
 └─ 1.10 aprovação humana    → 08_cortes.json (campo "aprovado")
```

**Modelo híbrido (D33):** a parte local (1.1 a 1.7) roda no seu PC. Depois, numa sessão do
Claude Code, o Claude lê o `06_filtrados.json`, julga, agrupa os assuntos e grava o
`07_notas.json`. O Python termina o ranking. Só o texto dos candidatos sai do PC, nunca o
vídeo nem o áudio.

Tudo fica em `episodios/<id-do-video>/`. Cada etapa grava seu arquivo; se ele já existe, a
etapa é pulada (D20). Assim dá para ajustar o juiz, por exemplo, sem refazer a transcrição.

---

## 1.1 Extrair áudio
- **P1.1.1** ffmpeg extrai o áudio em WAV mono, 16 kHz. É o formato que Whisper e pyannote esperam. 🟡

## 1.2 Transcrever
- **P1.2.1** O idioma vem do campo `IDIOMA` do episódio, sem detecção automática. A vinheta de abertura pode enganar a detecção. Na v1 é sempre `pt` (D06, D27). ✅
- **P1.2.2** Filtro de voz (VAD Silero, já embutido no faster-whisper) ligado. Evita o Whisper inventar texto durante música e silêncio. 🟡
- **P1.2.3** Saída por palavra: texto, início, fim, confiança. ✅
- **P1.2.4** Modelo: `MODELO_WHISPER` (D08). 🧪

## 1.3 Diarizar (quem fala)
- **P1.3.1** pyannote gera trechos por falante (`SPEAKER_00`, `SPEAKER_01`…). ✅
- **P1.3.2** Cada palavra recebe o falante cujo trecho mais se sobrepõe a ela. Sem sobreposição: herda o falante da palavra anterior. 🟡
- **P1.3.3** Os rótulos viram "Falante A, B, C…", na ordem em que cada um fala pela primeira vez. Nomes reais são desnecessários: só importa o que cada um fala (D29). ✅

## 1.4 Montar frases numeradas
- **P1.4.1** Uma frase termina no primeiro destes eventos: 🟡
  1. pontuação final (`.` `?` `!`) na palavra;
  2. pausa até a próxima palavra maior que `PAUSA_QUEBRA_FRASE`;
  3. troca de falante;
  4. a frase passou de `FRASE_MAX` sem pontuação: quebra na maior pausa interna.
- **P1.4.2** Cada frase guarda: número (001, 002…), início (1ª palavra), fim (última palavra), falante (o da maioria das palavras), texto e `pausa_depois` (silêncio até a próxima frase). 🟡
- **P1.4.3** Formato que o LLM lê: ✅
  ```
  [041] (00:12:03–00:12:07) Falante B: "..."
  ```

## 1.5 Dividir em blocos
- **P1.5.1** Blocos de `BLOCO_DURACAO` com sobreposição de `BLOCO_SOBREPOSICAO` (um bloco novo começa a cada 8 min). 🟡
  - **Por que 2 min:** um corte tem no máximo 90 s. Com 2 min de sobreposição, qualquer corte possível cabe inteiro em pelo menos um bloco. Nenhum bom momento se perde na emenda entre blocos.
- **P1.5.2** Bloco sempre começa e termina no limite de uma frase. 🟡

## 1.6 Gerar candidatos (Qwen local, D33)
- **P1.6.1** Por bloco, o LLM propõe de 0 a `CANDIDATOS_POR_BLOCO_MAX` trechos. Pode devolver menos se o bloco for fraco. 🟡
  - Conta: episódio de 1h30 → ~11 blocos → até ~55 brutos → ~`CANDIDATOS_ALVO` depois dos filtros.
- **P1.6.2** O prompt explica: 🟡
  - os 5 critérios do juiz (gancho, autossuficiente, valor, fechamento, clareza), para o gerador já mirar no que vai ser cobrado;
  - a duração permitida (`DURACAO_MIN` a `DURACAO_MAX`);
  - ignorar patrocínio, propaganda, vinheta, abertura e despedida;
  - exemplos tirados das suas referências do que é interessante (D30; entram depois de definir toda a lógica).
  - Os textos do prompt ficam separados por idioma (D27).
- **P1.6.3** Resposta em JSON forçado por schema (saída estruturada do Ollama): 🟡
  ```json
  {"candidatos": [
    {"frase_inicio": 41, "frase_fim": 52,
     "titulo_provisorio": "...", "assunto": "2 a 4 palavras", "motivo": "1 frase"}
  ]}
  ```
- **P1.6.4** Validação: número de frase inexistente, fora do bloco, ou fim antes do início → descarta o candidato. 🟡
- **P1.6.5** Ollama com `NUM_CTX` (o padrão é menor que um bloco de 10 min e corta o texto sem avisar) e temperatura `TEMP_GERACAO`. 🟡
- **P1.6.6** Conversão para tempo: início = início da `frase_inicio`; fim = fim da `frase_fim`. ✅

## 1.7 Filtro de lógica (Python, sem IA)
Aplicado nesta ordem:

**P1.7.1 Duplicados** 🟡
Dois candidatos que se sobrepõem em mais de `DUPLICADO_SOBREPOSICAO` da duração do menor:
fica só o primeiro gerado. Sobreposições menores passam; o ranking resolve depois (P1.9.4).

**P1.7.2 Início limpo** ✅ regra · 🟡 listas e conserto
Comparação por palavra inteira, ignorando maiúsculas e pontuação. Acentos contam, para não
confundir "e" com "é". As listas abaixo são de português; cada idioma terá as suas (D27).
- **Bloqueio (descarta):** começa com "como eu disse", "como eu falei", "como eu te falei",
  "como você disse", "como você falou", "como a gente falou", "como falamos", "isso aí",
  "e aí ele falou", "aí ele falou", "voltando ao", "voltando pra", "nesse caso", "por isso que".
- **Conector (tenta consertar):** começa com "e", "mas", "então", "aí", "porque", "pois",
  "isso", "esse", "essa", "ele", "ela", "eles", "elas".
  → Puxa o início 1 frase para trás, se a frase anterior começar limpa e a duração
  continuar ≤ `DURACAO_MAX`. Muitas vezes a frase anterior é a pergunta do apresentador, o
  que costuma dar um ótimo começo de corte.
  → Se não der: mantém e marca `aviso_inicio = true` (aparece na aprovação humana).

**P1.7.3 Fim numa pausa** ✅ regra · 🟡 conserto
A última frase precisa ter `pausa_depois` ≥ `PAUSA_MIN_FIM`. Se não tiver:
1. estende até `AJUSTE_MAX_FRASES` frases para frente, sem passar de `DURACAO_MAX`;
2. se não achar pausa, encurta até `AJUSTE_MAX_FRASES` frases, sem ficar abaixo de `DURACAO_MIN`;
3. se ainda não achar, descarta.

**P1.7.4 Duração** ✅
Entre `DURACAO_MIN` e `DURACAO_MAX`, medida depois de todos os ajustes acima.

## 1.8 Juiz (Claude, separado da geração, D33)
- **P1.8.1** O Claude julga cada candidato isoladamente, separado de quem gerou (D22). Uma sessão do Claude Code pode julgar vários episódios em lote. ✅
- **P1.8.1a** Como acionar a sessão (manual em lote, ou automática pela linha de comando): ❓ ver Q8 em [perguntas abertas](91-perguntas-abertas.md).
- **P1.8.2** O juiz vê só o texto do corte, com os falantes, sem o que vem antes ou depois. Igual ao espectador. É isso que torna honesto o critério "autossuficiente". 🟡
- **P1.8.3** Nota de 0 a 10 por critério, cada uma com 1 frase de justificativa, usando esta régua: 🟡

  | Critério (peso) | Pergunta | 0 | 5 | 10 |
  |---|---|---|---|---|
  | Gancho (30%) | Os primeiros 3 segundos prendem? | Começa morno: cumprimento, contexto arrastado | Assunto interessante, mas demora a engatar | A primeira frase já gera curiosidade, polêmica ou emoção |
  | Autossuficiente (25%) | Quem nunca viu o episódio entende? | Depende de algo dito antes ("ele", "aquilo") sem explicar | Entende o geral, com uma ou outra referência solta | Entende tudo sem contexto nenhum |
  | Valor (20%) | O espectador ganha algo? | Conversa vazia | Algo interessante, mas comum | Informação rara, história marcante, piada boa ou opinião forte |
  | Fechamento (15%) | Termina de forma satisfatória? | Corta no meio da ideia | Termina, mas sem ponto alto | Termina com conclusão, punchline ou frase de efeito |
  | Clareza (10%) | Dá para entender a fala? | Muita gente falando junto, frases truncadas | Compreensível com algum esforço | Fala limpa e direta |

- **P1.8.4** Nota final = soma ponderada com `PESOS`, calculada no Python, não pelo LLM. ✅ pesos · 🟡 cálculo no Python
  - Ex.: gancho 8, autossuficiente 7, valor 9, fechamento 6, clareza 8 → 8×0,30 + 7×0,25 + 9×0,20 + 6×0,15 + 8×0,10 = **7,65**
- **P1.8.5** ~~Temperatura `TEMP_JUIZ`~~ ❌ Não se aplica: o Claude Code não expõe temperatura. A consistência vem da régua fixa (P1.8.3) e da calibração (P1.8.7). A régua fica num arquivo, e o Claude sempre a lê antes de julgar. 🟡
- **P1.8.6** O juiz também sugere o título (até `TITULO_MAX`) e a frase-gancho da tela (até `GANCHO_TELA_MAX`). São insumos da Parte 2. 🟡
- **P1.8.7** Calibração antes de confiar no juiz: usar as suas referências (D30). Você dá nota a ~10 cortes reais, comparamos com as notas dele e ajustamos o prompt (risco R6). 🟡

## 1.9 Ranking com diversidade
- **P1.9.1** Descarta nota final abaixo de `NOTA_MINIMA`. Melhor postar menos do que postar corte fraco. ❓ O valor será definido com as suas referências.
- **P1.9.2** O Claude agrupa os assuntos na mesma sessão do juiz (D33): olha a lista (título + assunto) de todos os candidatos e grava um grupo para cada um no `07_notas.json`. Evita que "eleição" e "política" contem como assuntos diferentes. 🟡
- **P1.9.3** Ordena por nota final. Empate: maior nota de gancho. 🟡
- **P1.9.4** Percorre a lista em ordem; um corte entra se: ✅
  - não se sobrepõe a nenhum já escolhido (nem 1 segundo);
  - o grupo de assunto dele tem menos de `MAX_POR_ASSUNTO` cortes escolhidos.
- **P1.9.5** Para ao chegar em `CORTES_POR_EPISODIO` (5, D28). Os próximos `RESERVAS` que passam nas mesmas regras viram reserva. ✅
- **P1.9.6** Se não houver cortes suficientes acima da nota mínima, entrega menos. 🟡

## 1.10 Aprovação humana
- **P1.10.1** Nada vai para a edição sem aprovação (D23). ✅
- **P1.10.2** Como aprovar (tela, planilha…): ❓ decidir na Parte 2. Por enquanto, o campo `aprovado` no JSON.

---

## Arquivos de cada etapa

| Arquivo | Conteúdo |
|---|---|
| `01_audio.wav` | Áudio mono 16 kHz |
| `02_palavras.json` | Cada palavra com início, fim e confiança |
| `03_falantes.json` | Trechos de cada falante (pyannote) |
| `04_frases.json` | Frases numeradas com início, fim, falante, texto e pausa depois |
| `05_candidatos.json` | Candidatos brutos do LLM, já convertidos para tempo |
| `06_filtrados.json` | Candidatos que passaram no filtro, com os ajustes feitos |
| `07_notas.json` | Notas por critério, justificativas, título e gancho |
| `08_cortes.json` | Top N + reservas, com o campo de aprovação |

## Formato final: `08_cortes.json`

```jsonc
{
  "episodio": "id-do-video",
  "fonte": "URL do YouTube",
  "cortes": [
    {
      "id": "c01",
      "status": "top",                 // "top" ou "reserva"
      "inicio": 723.41,                // segundos, 2 casas
      "fim": 781.90,
      "duracao": 58.49,
      "frase_inicio": 41,
      "frase_fim": 52,
      "titulo": "...",
      "gancho_tela": "...",
      "assunto": "...",
      "notas": {"gancho": 8, "autossuficiente": 7, "valor": 9, "fechamento": 6, "clareza": 8},
      "nota_final": 7.65,
      "justificativa": "...",
      "aviso_inicio": false,           // true = começa com conector que não deu para consertar
      "aprovado": null                 // null = pendente; true/false depois da revisão
    }
  ]
}
```
Tempos ficam em segundos; o código converte para hh:mm:ss só para exibir. 🟡
