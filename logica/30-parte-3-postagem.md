# Parte 3: postagem

Status: ❓ **a planejar**. Abaixo está só o esboço. As regras das plataformas mudam, então
precisam ser conferidas nas fontes oficiais antes de decidir.

**Entrada:** vídeos 9:16 prontos + título, legenda do post e hashtags.
**Meta:** ~50 cortes por semana (~7 por dia) no TikTok e no Instagram Reels (D04).

## Já decidido
- Não usar robôs não oficiais (tiktok-uploader, instagrapi) (D24).
- Upload-Post descartado: 10 posts grátis/mês não atendem 50/semana (D26).

## Caminhos grátis a avaliar

| Rede | Caminho | O que se sabe (confirmar) |
|---|---|---|
| Instagram | API oficial (conta Profissional) | Dá para automatizar; o limite diário é bem acima de 7 posts. |
| TikTok | API oficial | Sem auditoria, só publica como privado. As regras da auditoria parecem não aprovar ferramentas para postar nas próprias contas (R5). |
| TikTok | Enviar como rascunho pela API | Você só toca em "publicar" no app. Confirmar se exige auditoria. |
| Ambas | Agendamento manual em lote | O pipeline entrega tudo pronto e você agenda 1 vez por semana no TikTok Studio (web) e no Meta Business Suite. Ambos grátis. |

## Perguntas para quando planejarmos esta parte
- Postar em uma conta por rede ou em várias?
- Horários de postagem: fixos ou espalhados ao longo do dia?
- Legenda do post e hashtags: geradas pelo LLM ou modelo fixo?
- Crédito ao podcast original na legenda?
