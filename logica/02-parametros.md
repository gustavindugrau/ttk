# Parâmetros ajustáveis

Todos os números do pipeline ficam aqui, num lugar só. Na execução, esta tabela vira o
arquivo de configuração. Para mudar um valor, mude aqui e registre o motivo em
[01-decisoes.md](01-decisoes.md).

Legenda: ✅ Decidido · 🟡 Proposto · 🧪 A testar · ❓ Aberto

## Parte 1: identificação dos cortes

| Parâmetro | Valor | Usado em | Status |
|---|---|---|---|
| `IDIOMA` | campo de cada episódio; na v1 sempre `pt` (D06, D27) | P1.2.1 | ✅ |
| `MODELO_WHISPER` | `large-v3-turbo` ou `large-v3` (`int8_float16`) | P1.2.4 | 🧪 |
| `PAUSA_QUEBRA_FRASE` | 0,7 s | P1.4.1 | 🟡 |
| `FRASE_MAX` | 30 s | P1.4.1 | 🟡 |
| `BLOCO_DURACAO` | 10 min | P1.5.1 | ✅ |
| `BLOCO_SOBREPOSICAO` | 2 min | P1.5.1 | 🟡 |
| `MODELO_LLM` | `qwen2.5:7b` (Q4), só para gerar candidatos (D33) | 1.6 | 🟡 |
| `JUIZ` | Claude, por sessão do Claude Code (D33) | 1.8, 1.9.2 | ✅ |
| `NUM_CTX` | 8192 tokens | P1.6.5 | 🟡 |
| `CANDIDATOS_POR_BLOCO_MAX` | 5 | P1.6.1 | 🟡 |
| `CANDIDATOS_ALVO` | ~30 por episódio | P1.6.1 | ✅ |
| `TEMP_GERACAO` | 0,3 | P1.6.5 | 🟡 |
| `DUPLICADO_SOBREPOSICAO` | 80% da duração do menor | P1.7.1 | 🟡 |
| `PAUSA_MIN_FIM` | 0,5 s | P1.7.3 | ✅ |
| `AJUSTE_MAX_FRASES` | 2 | P1.7.3 | 🟡 |
| `DURACAO_MIN` | 20 s | P1.7.4 | ✅ |
| `DURACAO_MAX` | 90 s | P1.7.4 | ✅ |
| ~~`TEMP_JUIZ`~~ | não se aplica: o juiz é o Claude (D33) | P1.8.5 | ❌ |
| `PESOS` | gancho 30% · autossuficiente 25% · valor 20% · fechamento 15% · clareza 10% | P1.8.4 | ✅ |
| `TITULO_MAX` | 60 caracteres | P1.8.6 | 🟡 |
| `GANCHO_TELA_MAX` | 8 palavras | P1.8.6 | 🟡 |
| `NOTA_MINIMA` | a definir com as suas referências (ponto de partida sugerido: 6,0) | P1.9.1 | ❓ |
| `MAX_POR_ASSUNTO` | 2 | P1.9.4 | ✅ |
| `CORTES_POR_EPISODIO` | 5 (D28) | P1.9.5 | ✅ |
| `RESERVAS` | 3 | P1.9.5 | ✅ |

## Parte 2: edição
_A definir quando planejarmos a Parte 2._

## Parte 3: postagem

| Parâmetro | Valor | Usado em | Status |
|---|---|---|---|
| `POSTS_POR_DIA` | 7 | P3.1.2 | ✅ |
| `HORARIOS` | 08h, 10h, 12h, 14h, 17h, 19h, 21h | P3.1.2 | 🟡 |
| `HORARIOS_NOBRES` | 12h, 19h, 21h (palpite inicial) | P3.1.4 | 🟡 |
| `MAX_MESMO_EPISODIO_DIA` | 1 (quando possível) | P3.1.4 | 🟡 |
| `IG_ATRASO_MAX` | 60 min | P3.2.3 | 🟡 |
| `IG_TENTATIVAS` | 3 | P3.2.4 | 🟡 |
| `TIKTOK_ANTECEDENCIA_MAX` | 10 dias (limite do TikTok Studio) | P3.3.3 | ✅ |
