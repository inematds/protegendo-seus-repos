# 🛡️ Protecting your repos

[![Protecting your repos](guia/assets/banner-en.jpg)](https://inematds.github.io/protegendo-seus-repos/guia/en/)

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

## What it is

This is a guide for anyone who keeps code on GitHub (or a similar service) and wants to be ready for the day their account is blocked or hacked. It explains why copying everything to another account on the same service offers little protection and outlines a three-layer setup: a copy with another company, dated snapshots on a drive you own, and, if you like, your own server. It includes the git commands for each step, the limits of free plans, and what would change if you left GitHub for good. You don’t need to install anything to read it: it lays out the approach and method, ready for you to apply.

## 📖 User guide

Complete guide (landing page + step-by-step): **https://inematds.github.io/protegendo-seus-repos/guia/en/**

The full overview, in continuous prose: [docs/VISAO.md](docs/VISAO.md) (in Portuguese).

## The idea in 30 seconds

- **Another GitHub account offers little protection:** it’s the same company, tokens are usually on the same machine, and an automatic mirror copies the damage.
- **The 3-2-1 rule:** 3 copies, 2 locations, 1 off-site — and at least one copy your everyday machine can’t delete.
- **Three layers:** GitLab on an unaffiliated account (a token that can push but not delete) + dated snapshots on a drive you own + Forgejo on a VPS (optional).
- **Test the recovery** with 3 repos before you need it.
- **Leaving for good** is costly (links, Pages, CI, deploys). What really removes the dependency is putting a **custom domain** in front of your pages.

## The process, from start to finish

| # | Step | Main command | Who does it |
|---|---|---|---|
| 1 | Prevention: 2FA, clean up tokens and keys, protect branches | account dashboard | 🧠 you |
| 2 | Create an unaffiliated backup account (new GitLab account, email, and phone number) | gitlab.com → Register | 🧠 you |
| 3 | Generate a push-only GitLab token (`read_repository` + `write_repository`) and a read-only GitHub token, stored in `tokens.env` (chmod 600) | GitLab: avatar → Edit profile → Access tokens · GitHub: Settings → Developer settings → Fine-grained tokens | 🧠 you |
| 4 | Local mirror of each repo | `git clone --mirror <url> /backup/espelho/<repo>.git` | ⚙️ script |
| 5 | Update the mirror every day | `git -C <espelho> remote update --prune` | ⚙️ timer |
| 6 | Daily snapshot | `mkdir -p <data>` + `git -C <espelho> bundle create <data>/<repo>.bundle --all` | ⚙️ timer |
| 7 | Push to GitLab (creates the private project on the first push) | `git -C <espelho> push <url-gitlab> --all` + `--tags` | ⚙️ timer |
| 8 | Weekly off-site copy: the VPS pulls the snapshots (or Forgejo) | on the VPS: `rsync -a usuario@sua-maquina:/backup/fotos/ /backup/fotos/` | ⚙️ timer |
| 9 | Test restoring 3 repos | `git clone --mirror <bundle>` + `git push <url> --all` + `--tags` | 🧠 you |

## License

Text licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Made by [INEMA.CLUB](https://inema.club) — open and free content.