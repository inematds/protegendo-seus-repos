# 🛡️ Protegendo seus repos

[![Protegendo seus repos](guia/assets/banner.jpg)](https://inematds.github.io/protegendo-seus-repos/guia/)

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

## O que é

Este é um guia para quem guarda código no GitHub (ou em qualquer serviço parecido) e quer estar pronto para o dia em que a conta for bloqueada ou invadida. Ele explica por que copiar tudo para outra conta do mesmo serviço protege pouco e mostra um desenho em três camadas: uma cópia em outra empresa, fotos datadas num disco seu e, se quiser, um servidor próprio. Traz os comandos do git para cada passo, os limites dos planos grátis e o que mudaria se você saísse do GitHub de vez. Não precisa instalar nada para ler: é a visão e o método, prontos para você aplicar.

## 📖 Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/protegendo-seus-repos/guia/**

A visão inteira, em texto corrido: [docs/VISAO.md](docs/VISAO.md).

## A ideia em 30 segundos

- **Outra conta no GitHub protege pouco:** é a mesma empresa, os tokens costumam estar na mesma máquina e um espelho automático copia o estrago.
- **Regra 3-2-1:** 3 cópias, 2 lugares, 1 fora de casa — e pelo menos uma cópia que a sua máquina do dia a dia não consegue apagar.
- **Três camadas:** GitLab numa conta sem vínculo (token que envia mas não apaga) + fotos datadas num disco seu + Forgejo numa VPS (opcional).
- **Teste a volta** com 3 repos antes de precisar dela.
- **Migrar de vez** custa caro (links, Pages, CI, deploy). O que tira a dependência de verdade é um **domínio próprio** na frente das páginas.

## O processo, do início ao fim

| # | Passo | Comando principal | Quem faz |
|---|---|---|---|
| 1 | Prevenção: 2FA, faxina de tokens e chaves, proteção de branch | painel da conta | 🧠 você |
| 2 | Criar a conta de backup sem vínculo (GitLab, e-mail e telefone novos) | gitlab.com → Register | 🧠 você |
| 3 | Gerar o token do GitLab só de envio (`read_repository` + `write_repository`) e o do GitHub só de leitura, guardados em `tokens.env` (chmod 600) | GitLab: avatar → Edit profile → Access tokens · GitHub: Settings → Developer settings → Fine-grained tokens | 🧠 você |
| 4 | Espelho local de cada repo | `git clone --mirror <url> /backup/espelho/<repo>.git` | ⚙️ script |
| 5 | Atualizar o espelho todo dia | `git -C <espelho> remote update --prune` | ⚙️ timer |
| 6 | Foto do dia | `mkdir -p <data>` + `git -C <espelho> bundle create <data>/<repo>.bundle --all` | ⚙️ timer |
| 7 | Enviar para o GitLab (cria o projeto privado no 1º envio) | `git -C <espelho> push <url-gitlab> --all` + `--tags` | ⚙️ timer |
| 8 | Cópia semanal fora de casa: a VPS puxa as fotos (ou Forgejo) | na VPS: `rsync -a usuario@sua-maquina:/backup/fotos/ /backup/fotos/` | ⚙️ timer |
| 9 | Testar a restauração com 3 repos | `git clone --mirror <bundle>` + `git push <url> --all` + `--tags` | 🧠 você |

## Licença

Texto sob [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Feito pelo [INEMA.CLUB](https://inema.club) — conteúdo aberto e gratuito.
