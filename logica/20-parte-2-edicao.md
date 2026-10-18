# Parte 2: edição

Status: ❓ **a planejar depois da Parte 3** (D32). Abaixo está só o esboço.

**Entrada:** vídeo do episódio + cortes aprovados (`08_cortes.json`).
**Saída:** um vídeo vertical 9:16 pronto por corte.

## Obrigatório vindo da Parte 3
- **Originalidade (R9):** cada corte precisa acrescentar algo próprio, ou o Instagram e o
  TikTok param de recomendar a conta. Decidir aqui o que acrescentar.
- Entregar, por corte, o arquivo de vídeo final e o texto da legenda para o pacote semanal (P3.3.2).

## O que precisa existir
1. Corte do trecho (com pequena margem antes e depois da fala).
2. Reenquadrar 9:16 seguindo o rosto de quem fala, em qualquer câmera (D05).
3. Legenda palavra por palavra.
4. Título/gancho na tela.
5. Barra de progresso.
6. Tela rápida de aprovação: ~50–65 cortes por semana para revisar.

## Esboço do reenquadramento multicâmera
1. PySceneDetect divide o corte em planos (cada troca de câmera). (D16)
2. Em cada plano, o MediaPipe acha os rostos:
   - 1 rosto (close) → segue esse rosto;
   - 2 ou mais rostos (plano aberto) → escolhe quem mexe a boca enquanto há fala;
   - nenhum rosto, ou dúvida → layout reserva: plano inteiro no centro, fundo desfocado (R7).
3. Suavizar o movimento do enquadramento para não tremer.
4. Processar só os cortes aprovados, nunca o episódio inteiro.
5. Plano B se a detecção pela boca falhar: TalkNet-ASD (github.com/TaoRuijie/TalkNet-ASD).

## Perguntas para quando planejarmos esta parte
- Estilo da legenda: palavra por palavra destacada, quantas palavras por vez, fonte, cor, posição.
- Título/gancho: fica o tempo todo ou só nos primeiros segundos?
- Barra de progresso: onde e de que cor?
- Margens do corte: quanto antes da primeira palavra e depois da última?
- Tela de aprovação: página local no navegador ou planilha?
