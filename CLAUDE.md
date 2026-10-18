# Instruções para o Claude — projeto de cortes de podcast

Este arquivo é lido automaticamente pelo Claude ao abrir a pasta. Ele vai junto pelo git,
então vale em qualquer máquina. **Regras e contexto do projeto ficam aqui ou em `logica/`,
nunca só na memória local do Claude** (ela não sincroniza entre máquinas).

## Como falar com o usuário

- Sempre em português (PT-BR), simples e claro.
- Plano completo e fundamentado **antes** de executar qualquer coisa. Avançar uma parte por vez.
- Não instalar nem rodar nada do pipeline sem o aval dele. Verificações só de leitura são aceitáveis.

## O projeto

Pipeline local (Windows, custo zero, só open source) que baixa episódios de podcast do YouTube,
acha cortes com LLM local (Ollama), edita em 9:16 e posta no TikTok e no Instagram Reels.

**A pasta `logica/` é a fonte da verdade.** Começar sempre por [logica/README.md](logica/README.md).
Toda nova decisão vai para os arquivos dessa pasta.

### Convenções da pasta `logica/`

- Decisões numeradas `D01…` (em `01-decisoes.md`), regras da Parte 1 como `P1.x.y`,
  riscos `R1…`, perguntas `Q1…`. Números ajustáveis vão em `02-parametros.md`.
- Status: ✅ decidido · 🟡 proposto · 🧪 a testar · ❓ aberto · ❌ substituído.
- Nada se apaga: decisão trocada recebe ❌ e aponta para a nova.
- Só se executa o que estiver ✅.

### Onde paramos (atualizar ao fim de cada sessão)

- Em 2026-10-17 a Parte 1 foi detalhada; Q1, Q2, Q4, Q6 e Q7 respondidas (D27–D32).
- **Próximo passo:** planejar a Parte 3 (postagem), depois a Parte 2 (edição).
- Modelo híbrido (D33): o Qwen local gera os candidatos e o Claude, via sessão do Claude Code,
  faz o juiz e agrupa os assuntos.
- Antes de executar, ensinar o usuário a usar o Ollama/Qwen, que ele nunca usou (D34).
- **Lembrar o usuário**, quando toda a lógica estiver definida, de passar as referências do que
  é interessante. Elas servem para os exemplos do prompt, a calibração do juiz, a nota mínima (Q3)
  e a revisão das regras 🟡 (Q5).

## Trabalho em duas máquinas

O usuário alterna entre a máquina principal (em casa) e outra quando está fora.

- **Início da sessão:** um hook (`.claude/settings.json`) roda `git pull --ff-only` sozinho.
  Se ele falhar (sem internet, conflito, alterações locais), avisar o usuário antes de mexer em
  qualquer arquivo.
- **Fim da sessão:** atualizar a seção "Onde paramos" acima e oferecer commit + `git push`.
  Nunca encerrar com trabalho sem enviar sem avisar.
- **Planejamento** (`logica/`) pode ser feito em qualquer máquina.
- **Execução do pipeline** (Ollama/Qwen, transcrição, edição) só na máquina principal:
  notebook com RTX 4050 6 GB (ver `logica/00-contexto.md`). Na outra máquina, não instalar nada
  do pipeline sem o usuário pedir.

## O que nunca vai para o git

O repositório é **público**.

- Vídeos e áudios (episódios baixados, cortes renderizados): o `.gitignore` já bloqueia.
- Chaves, senhas e tokens (TikTok, Instagram, etc.): sempre em `.env`, que está no `.gitignore`.
  Nunca escrever segredos em arquivos versionados.
