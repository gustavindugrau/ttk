# Perguntas abertas e lembretes

Quando uma pergunta é respondida, ela sai da lista de abertas e vira decisão em
[01-decisoes.md](01-decisoes.md) ou valor em [02-parametros.md](02-parametros.md).

## ⏰ Lembrete (para depois de definir toda a lógica)
- **Você vai me passar referências do que é interessante** (D30). Com elas:
  1. montamos os exemplos do prompt do gerador e do juiz;
  2. calibramos o juiz (P1.8.7);
  3. definimos a nota mínima (Q3);
  4. revisamos as regras 🟡 da Parte 1 (Q5).
- **Você vai me passar referências de legenda do post e de hashtags** (D40). Com elas,
  definimos quem escreve (Claude na sessão do juiz ou modelo fixo) e o formato (Q9).

- **Te ensinar a usar o Ollama/Qwen antes de executar** (D34): instalar, baixar o modelo, testar uma pergunta, entender os parâmetros.

## Abertas

| # | Pergunta | Quando decidir |
|---|---|---|
| Q9 | Legenda do post e hashtags: quem escreve e em que formato (P3.4.2) | Junto com as referências de legenda e hashtags |
| Q8 | Como acionar o Claude como juiz: você abre uma sessão por semana e eu julgo todos os episódios em lote, ou um script chama o Claude Code pela linha de comando automaticamente | Ao planejar a orquestração, depois das Partes 2 e 3 |
| Q3 | Nota mínima para um corte entrar (`NOTA_MINIMA`). Ponto de partida sugerido: 6,0 | Junto com as referências |
| Q5 | Aprovar ou mudar as regras 🟡 de [10-parte-1-identificacao.md](10-parte-1-identificacao.md) | Junto com as referências |

## Respondidas em 17/10/2026

| # | Pergunta | Resposta | Registro |
|---|---|---|---|
| Q1 | Cortes por episódio | 5 por episódio; a escolha deve seguir as suas referências do que é interessante | D28, D30 |
| Q2 | Nomes dos falantes | Desnecessários: só importa o que cada um fala | D29 |
| Q4 | Idioma | Português na v1; a lógica deve prever outros idiomas | D06, D27 |
| Q6 | Fonte dos podcasts | Manter terceiros sem autorização (risco R1 aceito) | D03 |
| Q7 | Ordem do planejamento | Parte 3 (postagem) antes da Parte 2 | D32 |
