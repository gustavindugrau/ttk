# Registro de decisões

Regras deste arquivo:
- Cada decisão tem um ID fixo (D01, D02…). Uma nova decisão recebe o próximo número livre e entra na seção do seu tema.
- Nada se apaga. Se uma decisão mudar, ela recebe ❌ e aponta para a que a substituiu.
- Legenda: ✅ Decidido · 🟡 Proposto (aguarda seu OK) · 🧪 A testar · ❓ Aberto · ❌ Substituído

## Escopo e restrições

| ID | Decisão | Por quê | Status |
|---|---|---|---|
| D01 | Custo zero; só open source; tudo local no Windows | Restrição do projeto | ✅ |
| D02 | Plano completo antes de executar; uma parte por vez | Restrição do projeto | ✅ |
| D03 | Fonte: podcasts de terceiros, baixados do YouTube, sem autorização | Sua resposta de 17/10/2026, reconfirmada na Q6 com o risco R1 aceito | ✅ |
| D04 | Volume: ~50 cortes por semana, no TikTok e no Instagram Reels | Sua resposta de 17/10/2026 | ✅ |
| D05 | Vídeo multicâmera; o 9:16 segue o rosto de quem fala, em qualquer câmera | Sua resposta de 17/10/2026 | ✅ |
| D06 | Idioma dos podcasts: português (PT-BR) na primeira versão | Q4, 17/10/2026 | ✅ |
| D27 | A lógica prevê outros idiomas: o idioma é um campo de cada episódio, e tudo que depende de idioma (listas de "início limpo", textos dos prompts, modelo de alinhamento) fica separado por idioma | Q4: você pretende usar podcasts em outros idiomas | ✅ |
| D28 | 5 cortes por episódio (≈10 episódios por semana para chegar a ~50) | Q1, 17/10/2026 | ✅ |
| D29 | Falantes identificados só como "Falante A, B, C"; nomes são desnecessários | Q2: só importa o que cada um fala | ✅ |
| D30 | Você vai passar referências do que é interessante; elas viram exemplos no prompt do gerador e do juiz e base da calibração. Isso acontece **depois de definir toda a lógica** | Q1, 17/10/2026 | ✅ |
| D31 | A seleção dentro do pipeline é feita pelo LLM local, não pelo Claude | Esclarecimento da Q1 | ❌ Substituída por D33 |
| D33 | **Modelo híbrido:** o Qwen local gera os candidatos (1.6). O Claude, por sessão do Claude Code (coberta pelo plano que você já usa, sem API paga), faz o juiz (1.8) e o agrupamento de assuntos do ranking (1.9.2). O resto continua local | Sua escolha de 17/10/2026: qualidade de julgamento melhor, com pouco uso do plano | ✅ |
| D34 | Antes de executar, o Claude te ensina a usar o Ollama/Qwen (você nunca usou) | Seu pedido de 17/10/2026 | ✅ |
| D32 | Ordem do planejamento: Parte 3 (postagem) antes da Parte 2 (edição) | Q7: postar 50/semana no TikTok é a maior incerteza | ✅ |
| D25 | A pasta `logica/` guarda todas as decisões e regras; só se executa o que estiver ✅ aqui | Seu pedido de 17/10/2026 | ✅ |

## Ferramentas

| ID | Decisão | Por quê | Status |
|---|---|---|---|
| D07 | Transcrição: faster-whisper | Grátis, local, tempo por palavra | ✅ |
| D08 | Modelo do Whisper: `large-v3-turbo` ou `large-v3`, em `int8_float16` | Os dois cabem em 6 GB; turbo é mais rápido, large-v3 um pouco mais preciso | 🧪 |
| D09 | WhisperX para alinhar palavras e atribuir falante a cada palavra | Tempos por palavra mais precisos (legenda e corte na pausa). Comparar com faster-whisper puro | 🧪 |
| D10 | Diarização: pyannote (token grátis do Hugging Face) | Grátis, local | ✅ |
| D11 | LLM local via Ollama | Grátis, local | ✅ |
| D12 | Modelo do LLM local (gerador de candidatos, D33): Qwen 2.5 7B (Q4, ~4,7 GB) | Cabe inteiro em 6 GB com contexto de 8k. Qwen3 8B (~5,2 GB) fica no limite: testar como alternativa | 🟡 |
| D13 | Edição: Python + ffmpeg | Grátis, local | ✅ |
| D14 | Render com `h264_nvenc` (encoder de vídeo da RTX) | Muito mais rápido que render na CPU | 🟡 |
| D15 | Reenquadrar 9:16 com MediaPipe | Grátis, local, roda bem na CPU | ✅ |
| D16 | Detectar troca de câmera com PySceneDetect | Vídeo multicâmera (D05) exige saber onde cada plano começa | 🟡 |
| D17 | Baixar episódios com yt-dlp | Padrão open source para YouTube | 🟡 |
| D18 | Python 3.11 | Versão com maior compatibilidade entre as bibliotecas de IA no Windows | 🟡 |

## Arquitetura

| ID | Decisão | Por quê | Status |
|---|---|---|---|
| D19 | Um modelo por vez na GPU (Whisper → pyannote → Ollama → MediaPipe), descarregando cada um antes do próximo | 6 GB de VRAM não comporta dois juntos | ✅ |
| D20 | Cada etapa grava um arquivo na pasta do episódio; se o arquivo existe, a etapa é pulada | Permite parar e retomar, e ajustar uma etapa sem refazer a transcrição | 🟡 |
| D21 | O LLM aponta NÚMEROS DE FRASE; o código converte para tempo | LLM erra timestamps | ✅ |
| D22 | O juiz é uma chamada separada da geração | Quem propõe não deve se dar nota. Com D33, é até outro modelo | ✅ |
| D23 | Aprovação humana antes de editar | Controle de qualidade | ✅ |

## Postagem

| ID | Decisão | Por quê | Status |
|---|---|---|---|
| D24 | Não usar robôs não oficiais (tiktok-uploader, instagrapi) | Violam os termos de uso e podem banir a conta | ✅ |
| D26 | Upload-Post descartado | 10 posts grátis/mês não atendem 50/semana (D04) | ✅ |
