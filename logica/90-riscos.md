# Riscos

| ID | Risco | Gravidade | Como reduzir |
|---|---|---|---|
| R1 | Direitos autorais | Alta | Ver abaixo |
| R2 | Bibliotecas da NVIDIA no Windows | Média | Validar primeiro, no teste de 10 min |
| R3 | 6 GB de VRAM | Média | Um modelo por vez (D19) |
| R4 | Tempo de máquina | Baixa | Rodar à noite |
| R5 | Postagem automática no TikTok | Alta | Planejar na Parte 3 |
| R6 | Gerador local (Qwen 7B) deixar passar bons momentos | Média | Exemplos das suas referências + calibração |
| R7 | Errar quem está falando no plano aberto | Média | Layout reserva |
| R8 | Juiz depende do Claude Code (limite do plano, internet) | Baixa | Julgar em lote; Qwen como juiz reserva |

## R1. Direitos autorais (o maior risco do projeto)
Postar ~50 cortes por semana de terceiros sem autorização (D03) tende a gerar remoção de
vídeo, áudio mutado, strikes e, com a repetição, banimento da conta. TikTok e Instagram
também reduzem o alcance de conteúdo repostado sem originalidade. Baixar do YouTube
também vai contra os termos do YouTube.

**Recomendação (custo zero):** escolher podcasts que liberam cortes publicamente. Vários
podcasts brasileiros liberam, alguns com programa oficial de cortes. Conferir na
descrição do canal ou pedir por e-mail. O pipeline não muda nada.

**Situação:** risco **aceito por você** em 17/10/2026 (Q6). Seguimos com podcasts de
terceiros sem autorização (D03).

## R2. Bibliotecas da NVIDIA no Windows
O faster-whisper na GPU precisa de cuBLAS e cuDNN, que no Windows não vêm de forma
simples pelo pip. É um ponto de dor conhecido. O teste de 10 min valida isso antes de tudo.

## R3. 6 GB de VRAM
Dois modelos carregados juntos estouram a memória. Mitigação: sequência rígida, um modelo
por vez (D19).

## R4. Tempo de máquina
Estimativa grosseira, a medir no teste: 30–60 min por episódio de 1h30. Com 5 cortes por
episódio (D28), são ~10 episódios por semana, ou ~5 a 10 h de máquina. Deixar rodando à noite.

## R5. Postagem automática no TikTok
A API oficial sem auditoria só publica como privado, e as regras da auditoria parecem não
aprovar ferramentas para postar nas próprias contas. Confirmar as regras atuais na Parte 3.
Plano B: agendar em lote pelo TikTok Studio (web, grátis).

## R6. Gerador local deixar passar bons momentos
Com o modelo híbrido (D33), o juiz deixou de ser o modelo pequeno. Isso reduziu o risco
antigo de notas todas iguais. O ponto fraco agora é o gerador: se o Qwen 7B não propuser
um bom momento, o juiz nunca o vê. Mitigações:
- exemplos das suas referências no prompt do gerador (D30);
- pedir até 5 candidatos por bloco, mais do que o necessário;
- **calibração:** comparar o que o Qwen propôs com o que você considera bom.

## R8. Juiz depende do Claude Code
O juiz consome o limite do plano que você já usa e precisa de internet. Só o texto dos
candidatos sai do PC. Mitigações: julgar vários episódios em lote numa sessão só; se o
Claude estiver indisponível, o Qwen local pode fazer o papel de juiz como reserva, com a
mesma régua.

## R7. Errar quem está falando no plano aberto
Com várias pessoas no quadro, a detecção pela boca pode errar. Mitigação: quando houver
dúvida, usar o layout reserva (plano inteiro no centro, fundo desfocado) em vez de
arriscar o rosto errado. Detalhar na Parte 2.
