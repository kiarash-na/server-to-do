# server-to-do — agent entry point

You are an AI agent about to set up a fresh VPS by walking this repo. This file only routes — it holds no content.

## Where you are
An ICM pipeline for turning a fresh VPS into a safe, monitored, backed-up server. Numbered folders = ordered levels. Each level has a `CONTEXT.md` contract and numbered step cards. Step cards are the unit of work.

## Read next (in order)
1. `CONTEXT.md` — how the pipeline works, what "done" means
2. `_shared/safety-rules.md` — **mandatory before touching any server** — hard rules you must not break
3. The run instance the human points you at (`runs/<server-name>.md`) — it tells you which steps are already done
4. The level folder matching the first undone step → its `CONTEXT.md`, then the step card

## Level map
| Folder | Job |
|---|---|
| `00_first-contact/` | Get in safely, replace root, first update |
| `01_lock-the-doors/` | Firewall, brute-force protection, auto-updates, log awareness |
| `02_keep-it-alive/` | Swap, time sync, log rotation, uptime monitoring, backups |

## Rules of the house
- One step at a time, in numbered order. Never skip ahead.
- Every step card ends in a **Human check** — stop there and report.
- Facts live in exactly one place. Follow links instead of re-reading everything.
- New here? The human-facing story is `README.md`. Explanations live in `_shared/glossary.md`.

*Future levels (`03_`, `04_`…) may exist — same rules apply.*
