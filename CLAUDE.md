# CLAUDE.md · Cartela_Cores

Instruções operacionais para o Claude Code trabalhar neste repositório.

## Fluxo Git (Mac + Windows)

Este projeto é trabalhado em dois computadores (MacBook e PC Windows), sincronizados pelo GitHub: https://github.com/jeffreiry/cartela-cores (branch `main`).

- **Ao começar qualquer sessão, antes de editar:** rodar `git status` e `git pull` (use `git pull --rebase` se houver commits locais). Avisar o autor se o pull trouxer commits novos ou conflitos.
- **Ao terminar:** conferir `git status`, resumir o que mudou e lembrar o autor de commitar e dar `git push` — o commit e o push só acontecem quando o autor pedir. Trabalho não enviado fica preso em uma máquina.
- **Push recusado:** rodar `git pull --rebase` e tentar de novo. Nunca usar `push --force` sem confirmação explícita do autor.
- **`.env` não vai para o GitHub:** quando entrar uma credencial nova, atualizar à mão nos dois computadores e manter o `.env.example` com os nomes das variáveis (sem valores).
- **Fim de linha:** o `.gitattributes` usa `* text=auto`. Não commitar mudanças que sejam só CRLF/LF.
