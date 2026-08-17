# runs/ — one folder per server

Copy `../_templates/run-instance/` here, named after your server:

```bash
cp -r ../_templates/run-instance ./my-server
```

Fill **facts.md** (the agent verifies it on first scan), take the provider snapshot, then hand the repo to your agent:
*"Run server-to-do on `runs/my-server/` — follow CLAUDE.md."*

## What's inside a run folder
| File | Job |
|---|---|
| `facts.md` | Frozen server facts — locked in before card 00-01 |
| `level-0.md` … `level-5.md` | Step checkboxes — the pipeline's state (level-4 is the optional app-hosting track; level-5 is monitoring, open once levels 0–2 are checked) |
| `summary.md` | Super compact log: one line per verified step + incidents. Read this first to know where things stand. |
| `health/` | Timestamped health reports (Level 5) — newest file = current state, the series = the trend |

**Why this folder is gitignored:** run folders record real IPs, usernames, and your infra's state. That's your business, not the public repo's. Keep them locally, or in a private fork.
