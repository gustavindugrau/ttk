# Contexto

## Objetivo
Pipeline local que baixa um episódio de podcast do YouTube, identifica ~30 momentos
clipáveis, escolhe os melhores, edita em formato vertical (9:16) e prepara/posta no
TikTok e no Instagram Reels.

## Restrições (obrigatórias)
- CUSTO ZERO: nada pago, nem API paga (OpenAI, Claude API, Gemini pago etc.).
- Apenas ferramentas open source, rodando LOCALMENTE no PC (Windows).
- Plano completo e fundamentado ANTES de executar qualquer coisa.
- Ir por partes, um passo de cada vez, simples e claro.

## Pendências respondidas (17/10/2026)

| # | Pergunta | Resposta | Impacto |
|---|---|---|---|
| 1 | Hardware | RTX 4050 Laptop **6 GB de VRAM**, i7-13620H, 16 GB RAM, 392 GB livres | Tudo roda local, mas um modelo de cada vez na GPU. LLM limitado a ~7–8B. |
| 2 | Direitos | Podcasts de **terceiros, sem autorização**, baixados do YouTube | Maior risco do projeto (ver [90-riscos.md](90-riscos.md)). |
| 3 | Volume | **~50 cortes/semana** (~7/dia, ~215/mês) | Upload-Post grátis (10/mês) descartado. Postagem vira o gargalo. |
| 4 | Formato do vídeo | **Multicâmera**: troca o tempo todo entre plano aberto e close | O 9:16 precisa detectar quem fala em cada plano. Parte mais difícil da edição. |

## Hardware (lido no PC em 17/10/2026)

| Item | Valor |
|---|---|
| GPU | NVIDIA RTX 4050 Laptop, 6 GB de VRAM, driver 546.18, CUDA 12.3 |
| CPU | Intel i7-13620H, 10 núcleos / 16 threads |
| RAM | 16 GB |
| Disco | 392 GB livres no C: |

## Software no PC (17/10/2026)
- Instalado: git.
- Falta (instalar só na hora de executar): Python 3.11, ffmpeg, Ollama, yt-dlp.
- O `python` que aparece no PATH é só o atalho da Microsoft Store, não um Python instalado.
- Recomendado, não obrigatório: atualizar o driver NVIDIA.

## Referências open source (inspiração, não obrigatórias)
- podcli (github.com/nmbrthirteen/podcli): AGPL, local, tem MCP, roda no Windows; publica só no YouTube.
- OpenShorts (github.com/mutonby/openshorts): MIT, Docker, tem MCP; usa Gemini e Upload-Post.
- opensource-clipping (github.com/NaufalRizqullah/opensource-clipping): Python simples; usa Gemini.

Atenção: whisperai.com e Fireflies NÃO são o Whisper. São serviços pagos de terceiros.
O Whisper real é github.com/openai/whisper (open source, MIT).
