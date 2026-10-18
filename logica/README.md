# Lógica do projeto: cortes de podcast → TikTok e Reels

Esta pasta guarda **todas as decisões e regras** do pipeline. É a fonte da verdade:
nada é executado sem estar escrito e aprovado aqui.

_Criada em 17/10/2026. Status geral: **planejamento**. Nada foi instalado nem executado._

## Como ler

| Arquivo | O que tem |
|---|---|
| [00-contexto.md](00-contexto.md) | Objetivo, restrições, hardware, respostas às pendências |
| [01-decisoes.md](01-decisoes.md) | Registro numerado de todas as decisões (D01, D02…) |
| [02-parametros.md](02-parametros.md) | Todos os números ajustáveis num lugar só (durações, pausas, pesos…) |
| [10-parte-1-identificacao.md](10-parte-1-identificacao.md) | Regras da Parte 1: do episódio à lista de cortes |
| [20-parte-2-edicao.md](20-parte-2-edicao.md) | Parte 2: corte, 9:16, legenda (a planejar) |
| [30-parte-3-postagem.md](30-parte-3-postagem.md) | Parte 3: postagem (a planejar) |
| [90-riscos.md](90-riscos.md) | Riscos e como reduzir cada um |
| [91-perguntas-abertas.md](91-perguntas-abertas.md) | O que falta você decidir |

## Legenda de status

| Marca | Significa |
|---|---|
| ✅ Decidido | Você decidiu, ou é imposto pelas restrições/hardware |
| 🟡 Proposto | Sugestão minha, aguarda seu OK |
| 🧪 A testar | Decide-se no teste com o trecho de 10 min |
| ❓ Aberto | Falta decidir |
| ❌ Substituído | Decisão antiga trocada por outra (fica registrada, não se apaga) |

## O pipeline em uma linha

```
YouTube → [0] Baixar → [1] Identificar cortes → aprovação humana → [2] Editar 9:16 → [3] Postar
```

## Status de cada parte

| Parte | Status |
|---|---|
| 0. Baixar episódio | 🟡 Proposto (yt-dlp) |
| 1. Identificar cortes | 🟡 Regras detalhadas; as 🟡 serão revisadas com as suas referências |
| 2. Edição | ❓ A planejar depois da Parte 3 (D32) |
| 3. Postagem | ❓ **Próxima a planejar** (D32) |

**Lembrete:** depois de definir toda a lógica, você passa as referências do que é
interessante (D30). Ver [91-perguntas-abertas.md](91-perguntas-abertas.md).

## Como trabalhamos
1. Discutimos uma parte por vez.
2. Cada decisão vira uma linha em [01-decisoes.md](01-decisoes.md); cada número vira um parâmetro em [02-parametros.md](02-parametros.md).
3. Mudou de ideia? A decisão antiga recebe ❌ e aponta para a nova. Nada se apaga.
4. Só executamos o que estiver ✅. O primeiro passo de execução será o teste de transcrição com um trecho de 10 min.
