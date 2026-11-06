# 🛡️ Protegiendo tus repos

[![Protegiendo tus repos](guia/assets/banner-es.jpg)](https://inematds.github.io/protegendo-seus-repos/guia/es/)

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

## Qué es

Esta es una guía para quienes guardan código en GitHub (o en cualquier servicio parecido) y quieren estar preparados para el día en que les bloqueen o les hackeen la cuenta. Explica por qué copiar todo a otra cuenta del mismo servicio ofrece poca protección y presenta un diseño de tres capas: una copia en otra empresa, fotos con fecha en un disco propio y, si quieres, un servidor propio. Incluye los comandos de git para cada paso, los límites de los planes gratuitos y lo que cambiaría si dejaras GitHub definitivamente. No necesitas instalar nada para leerla: presenta la visión y el método, listos para que los apliques.

## 📖 Guía de uso

Guía completa (landing + paso a paso): **https://inematds.github.io/protegendo-seus-repos/guia/es/**

La visión completa, en texto corrido: [docs/VISAO.md](docs/VISAO.md) (en portugués).

## La idea en 30 segundos

- **Otra cuenta en GitHub ofrece poca protección:** es la misma empresa, los tokens suelen estar en la misma máquina y un espejo automático también copia el daño.
- **Regla 3-2-1:** 3 copias, 2 lugares, 1 fuera de casa — y al menos una copia que tu máquina de todos los días no pueda borrar.
- **Tres capas:** GitLab en una cuenta sin vínculos (token que permite enviar, pero no borrar) + fotos con fecha en un disco propio + Forgejo en una VPS (opcional).
- **Prueba la restauración** con 3 repos antes de necesitarla.
- **Migrar definitivamente** cuesta caro (enlaces, Pages, CI, deploy). Lo que realmente te quita la dependencia es un **dominio propio** delante de las páginas.

## El proceso, de principio a fin

| # | Paso | Comando principal | Quién lo hace |
|---|---|---|---|
| 1 | Prevención: 2FA, limpieza de tokens y claves, protección de ramas | panel de la cuenta | 🧠 tú |
| 2 | Crear la cuenta de respaldo sin vínculos (GitLab, correo y teléfono nuevos) | gitlab.com → Register | 🧠 tú |
| 3 | Generar el token de GitLab solo para enviar (`read_repository` + `write_repository`) y el de GitHub solo de lectura, guardados en `tokens.env` (chmod 600) | GitLab: avatar → Edit profile → Access tokens · GitHub: Settings → Developer settings → Fine-grained tokens | 🧠 tú |
| 4 | Espejo local de cada repo | `git clone --mirror <url> /backup/espelho/<repo>.git` | ⚙️ script |
| 5 | Actualizar el espejo todos los días | `git -C <espelho> remote update --prune` | ⚙️ timer |
| 6 | Foto del día | `mkdir -p <data>` + `git -C <espelho> bundle create <data>/<repo>.bundle --all` | ⚙️ timer |
| 7 | Enviar a GitLab (crea el proyecto privado en el primer envío) | `git -C <espelho> push <url-gitlab> --all` + `--tags` | ⚙️ timer |
| 8 | Copia semanal fuera de casa: la VPS descarga las fotos (o Forgejo) | en la VPS: `rsync -a usuario@sua-maquina:/backup/fotos/ /backup/fotos/` | ⚙️ timer |
| 9 | Probar la restauración con 3 repos | `git clone --mirror <bundle>` + `git push <url> --all` + `--tags` | 🧠 tú |

## Licencia

Texto bajo [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Hecho por [INEMA.CLUB](https://inema.club): contenido abierto y gratuito.