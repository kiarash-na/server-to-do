# server-to-do — agent entry point

You're an AI agent turning a fresh VPS into a hardened server by walking this repo. This file only routes — zero content here.

## Read next (in order)
1. `CONTEXT.md` — how the pipeline works, what "done" means
2. `_shared/safety-rules.md` — **mandatory before touching any server**
3. `runs/<server-name>/` — the run folder your human points at; `summary.md` says what's already done
4. First unchecked step → its level's `CONTEXT.md` → the step card

## Level map
| Folder | Job |
|---|---|
| `00_first-contact/` | Get in safely, replace root, first update |
| `01_lock-the-doors/` | Firewall, brute-force protection, auto-updates, log awareness, Cloudflare origin lock |
| `02_keep-it-alive/` | Swap, time sync, log rotation, uptime monitoring |
| `03_get-it-back/` | Provider snapshots, inventory, off-box backups, DB dumps, restore drill |
| `04_ship-your-app/` | *Optional track:* GitHub → Docker → Dokploy → your app live on a domain with TLS |
| `05_keep-watch/` | Metrics + dashboard, alerts that find you, logs in one place, weekly health reports |

## House rules
- One step at a time, in numbered order. No skipping, no "while we're in there" extras.
- Every card ends in a **Human check** — stop there, report, wait.
- Stuck? Command missing, weird output, different OS? **Read the tool's official docs first** (man page / official site), then adapt toward the card's stated goal. Never guess flags.
- Improved a card or added a step mid-run? **Open a PR** (you may push branches yourself) or file an issue — fixes belong upstream, not on one server.
- Facts live in exactly one place. Follow links; don't re-read everything.

*Future levels (`06_`…) may exist — same rules apply.*
