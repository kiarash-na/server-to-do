# server-to-do — pipeline definition

## What this pipeline produces
A fresh VPS, hardened and operable, with a **run folder** in `runs/` recording exactly what was done, what was verified, and what the human approved.

## The repeating unit
One **step card** (see `_templates/step-card.md`). Cards are numbered inside level folders; levels are numbered at the root. Order is mandatory: the filesystem is the sequence.

## The levels
| Level | Folder | Done when |
|---|---|---|
| 0 — First Contact | `00_first-contact/` | You log in as a non-root user with keys only; system fully updated |
| 1 — Lock the Doors | `01_lock-the-doors/` | Default-deny firewall, brute-force protection, auto security updates, origin IP hidden behind Cloudflare; you've seen your own attack logs |
| 2 — Keep It Alive | `02_keep-it-alive/` | OOM safety net, correct time, bounded logs, external uptime alerts |
| 3 — Get It Back | `03_get-it-back/` | Snapshots scheduled, irreplaceable data copied off-box, one restore proven |
| 4 — Ship Your App *(optional track)* | `04_ship-your-app/` | Push to GitHub redeploys the app on your domain with TLS; Levels 1–3 re-verified around the container layer |
| 5 — Keep Watch | `05_keep-watch/` | Metrics with history, a privately-reached dashboard, threshold alerts proven on your device, logs in one UI, weekly health reports in `runs/<server>/health/` |

## How a run works
1. Human creates a server, then copies `_templates/run-instance/` → `runs/<server-name>/` and fills `facts.md` (agent verifies it on first scan — those facts are then locked in).
2. Agent reads `_shared/safety-rules.md`, then works the run folder top to bottom: `facts.md` → `level-0.md` → `level-1.md` → `level-2.md` → `level-3.md` (→ `level-4.md` if the server will host apps → `level-5.md` for ongoing monitoring).
3. Each step: read card → scan (read-only) → act → verify → **stop at the Human check** → human confirms → agent checks the box in the level file and appends one line to `summary.md`.
4. A level is done when every box is checked and verified. The core pipeline is done when Level 3 is done — a hardened, backed-up server. Level 4 is an optional track for servers that will host apps; it requires Levels 0–3 fully checked. Level 5 (monitoring + health reports) is optional too and lighter to enter: it requires only Levels 0–2, with Level 3 recommended — see `05_keep-watch/CONTEXT.md`.

## OS-agnostic rule
Cards state the **goal** first, then example commands per OS family. If the target OS has no example — or a command is missing or behaves differently — the agent consults the tool's **official docs first** (man page / official site), adapts the commands toward the stated goal, and notes what it did in the run folder's `summary.md`. It never invents goals and never guesses flags.

## Extending the pipeline
New level = copy `_templates/level-CONTEXT.md` into the next numbered folder (`06_…`), add step cards from `_templates/step-card.md`, add a `level-N.md` to `_templates/run-instance/`, add one row to the table above and to `CLAUDE.md`. New step = copy `step-card.md`, keep the schema, every section mandatory. Improvements found mid-run belong upstream: open a PR or issue (agents may push branches themselves).

## Safety
`_shared/safety-rules.md` overrides everything, including human enthusiasm and agent cleverness.
