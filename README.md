# ttk — cortes de podcast para TikTok e Reels

Pipeline local e de custo zero que transforma episódios de podcast do YouTube em cortes
verticais (9:16) para TikTok e Instagram Reels.

**Status: planejamento.** Toda a lógica está em [logica/](logica/README.md).

## Usar em outra máquina

### Primeira vez (configuração única)

1. Instale o Git: <https://git-scm.com/download/win> (pode aceitar tudo no padrão).
2. Abra um terminal na pasta onde quer o projeto e clone:

   ```bash
   git clone https://github.com/gustavindugrau/ttk.git
   ```

3. No app do Claude, abra a pasta `ttk` que foi criada. O Claude lê o
   [CLAUDE.md](CLAUDE.md) sozinho e já sabe o contexto e as regras do projeto.
4. No primeiro envio (`git push`), o Windows abre uma janela de login do GitHub:
   entre com a conta **gustavindugrau**. Depois disso, ele lembra.

### Sem poder instalar nada?

Use o Claude Code na web: <https://claude.ai/code>. Ele trabalha direto neste repositório
do GitHub, sem instalar nada na máquina.

### Toda vez que for trabalhar

| Quando | O que fazer |
|---|---|
| Ao abrir | O Claude já roda `git pull` sozinho. Se ele avisar de algum erro, resolva antes de editar. |
| Ao terminar | Peça ao Claude: **"sobe o que fizemos"** (ele faz commit e `git push`). |

Se esquecer de enviar numa máquina, a outra vai trabalhar numa versão velha e pode dar conflito.

## O que fica fora do git

- Vídeos e áudios: pesados demais (o GitHub recusa arquivos acima de 100 MB).
- Chaves e senhas: ficam em `.env`. O repositório é público.

Os dois já estão bloqueados no [.gitignore](.gitignore).
