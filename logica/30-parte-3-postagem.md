# Parte 3: postagem

**Objetivo:** publicar ~50 cortes por semana (7 por dia) em 1 conta do TikTok e 1 do Instagram.
**Entrada:** vídeos 9:16 prontos (Parte 2) + `08_cortes.json` aprovado.
**Saída:** Reels publicados no Instagram, pacote semanal do TikTok e registro de tudo que foi postado.

Status: 🟡 planejada em 18/10/2026. Pesquisa das regras feita nas fontes oficiais nessa data (ver no fim).
Legenda: ✅ Decidido · 🟡 Proposto · 🧪 A testar · ❓ Aberto

## O que a pesquisa mostrou (18/10/2026)

| Rede | Via automática oficial | Conclusão |
|---|---|---|
| TikTok | App sem auditoria só publica **privado** (até 5 contas/dia). A auditoria lista como inaceitável uma "ferramenta para subir conteúdo nas contas que você ou sua equipe gerencia" e um "app que copia conteúdo de outras plataformas". O envio como rascunho também exige aprovação | **Fechada para este projeto.** Agendar manualmente no TikTok Studio (D36) |
| Instagram | API oficial: até 100 posts/24h; envio do arquivo direto do PC (upload "resumable"); conta Profissional ligada a uma Página do Facebook | **Aberta.** Postagem automática (D35) |

## 3.0 Ciclo semanal

```
1x por semana (ex.: domingo)
 ├─ Parte 1 local: processar ~10 episódios (rodar à noite)
 ├─ Sessão do Claude: julgar e ranquear em lote
 ├─ Você: aprovar os cortes
 ├─ Parte 2: editar os ~50 aprovados
 ├─ 3.1 montar a agenda da semana
 ├─ 3.3 você agenda o TikTok no TikTok Studio (~1 h)
 └─ 3.2 o Instagram posta sozinho ao longo da semana
```
🟡 O dia da semana é sugestão.

## 3.1 Agenda da semana
- **P3.1.1** Todos os cortes aprovados e editados entram numa fila única (`postagem/fila.json`). ✅
- **P3.1.2** `POSTS_POR_DIA` por dia, nos `HORARIOS` fixos (D39), 7 dias por semana = 49 vagas. ✅
- **P3.1.3** O mesmo corte vai no mesmo dia e horário nas duas redes (D38). 🟡
- **P3.1.4** Distribuição: 🟡
  1. ordena a fila por nota final;
  2. os melhores cortes vão para os `HORARIOS_NOBRES` (palpite inicial: 12h, 19h e 21h; ajustar pelos dados);
  3. no máximo `MAX_MESMO_EPISODIO_DIA` corte do mesmo episódio por dia, quando possível. Assim cada dia mistura episódios.
- **P3.1.5** Sobrou vaga (semana com menos cortes)? Fica vazia; não se repete corte. 🟡

## 3.2 Instagram: postagem automática (API oficial, D35)
- **P3.2.1** Um script no seu PC, acionado pelo Agendador de Tarefas do Windows, posta cada corte no horário. 🟡
  - **Atenção:** a API do Instagram não agenda sozinha; quem dispara é o seu PC. **O PC precisa estar ligado e com internet nos horários** (risco R10).
- **P3.2.2** Passos de cada post: enviar o arquivo direto do PC (upload "resumable", sem link público) → esperar o Instagram processar → publicar → gravar o link no registro. ✅
- **P3.2.3** Se o PC estava desligado no horário: posta assim que ligar, se o atraso for até `IG_ATRASO_MAX`; senão, o corte vai para a próxima vaga livre. 🟡
- **P3.2.4** Falhou? Tenta `IG_TENTATIVAS` vezes; depois marca `erro` no registro para você ver. 🟡
- **P3.2.5** O script respeita o limite de 100 posts/24h da Meta (usamos 7). ✅
- **P3.2.6** Credenciais (token da Meta) ficam num arquivo `.env` local, nunca nesta pasta. 🟡
- **Configuração única** (eu te guio na execução): 🧪
  1. Conta do Instagram como Profissional (Criador ou Empresa).
  2. Página do Facebook ligada a ela.
  3. App no Meta for Developers, em modo de desenvolvimento, com você como administrador.
  4. Gerar um token de acesso que não expire (ou rotina de renovação).

## 3.3 TikTok: agendamento manual no TikTok Studio (D36)
- **P3.3.1** A conta do TikTok precisa ser Criador ou Empresa (o agendamento não aparece em conta pessoal). ✅
- **P3.3.2** O pipeline monta um pacote por semana: 🟡
  ```
  postagem/semana-2026-43/tiktok/
    seg_08h00_c03.mp4
    seg_08h00_c03.txt      ← legenda + hashtags, prontas para copiar
    ...
    checklist.csv           ← dia, hora, arquivo, coluna "agendado"
  ```
  O nome do arquivo já diz o dia e o horário: é só seguir em ordem.
- **P3.3.3** Você agenda tudo pelo TikTok Studio no computador, 1 vez por semana (o limite é 10 dias de antecedência). ~1 h por semana. ✅
- **P3.3.4** Depois de agendar, marca `agendado` no `checklist.csv`. O registro (3.5) lê dali. 🟡
- **P3.3.5** Nada de robôs não oficiais para automatizar isso (D24). ✅

## 3.4 Legenda do post e hashtags
- **P3.4.1** Sem crédito ao podcast original (D37). ✅
- **P3.4.2** Quem escreve e em que formato: ❓ será definido com as suas referências de legenda e hashtags (lembrete em [91-perguntas-abertas.md](91-perguntas-abertas.md)).
- **P3.4.3** Mesma legenda nas duas redes, salvo limite de tamanho de alguma delas. 🟡

## 3.5 Registro de postagens
- **P3.5.1** Tudo vai para `postagem/registro.csv`: corte, episódio, rede, data/hora prevista, data/hora real, status (`agendado`, `postado`, `erro`), link. 🟡
- **P3.5.2** Serve para não postar o mesmo corte duas vezes e, mais tarde, para ajustar os `HORARIOS` pelo alcance. 🟡

## Fontes (consultadas em 18/10/2026)
- TikTok, regras da Content Posting API: https://developers.tiktok.com/doc/content-sharing-guidelines
- TikTok, envio como rascunho: https://developers.tiktok.com/doc/content-posting-api-get-started-upload-content
- Meta, publicação de conteúdo no Instagram: https://developers.facebook.com/docs/instagram-platform/content-publishing
- Agendamento no TikTok Studio (guia não oficial): https://www.hopperhq.com/blog/how-to-schedule-tiktok-posts-desktop-mobile/
- Instagram e contas agregadoras (abr/2026): https://techcrunch.com/2026/04/30/instagram-restricts-reach-of-content-aggregators-in-new-crackdown/ e https://petapixel.com/2026/04/30/new-instagram-policies-target-reposted-content/
