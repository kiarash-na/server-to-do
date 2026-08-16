# server-to-do — pipeline definition

## What this pipeline produces
A fresh VPS, hardened and operable, with a **run instance** file in `runs/` recording exactly what was done, what was verified, and what the human approved.

## The repeating unit
One **step card** (see `_templates/step-card.md`). Cards are numbered inside level folders; levels are numbered at the root. Order is mandatory: the filesystem is the sequence.

## The levels
| Level | Folder | Done when |
|---|---|---|
| 0 — First Contact | `00_first-contact/` | You log in as a non-root user with keys only; system fully updated |
| 1 — Lock the Doors | `01_lock-the-doors/` | Default-deny firewall, brute-force protection, auto security updates; you've seen your own attack logs |
| 2 — Keep It Alive | `02_keep-it-alive/` | OOM safety net, correct time, bounded logs, external uptime alerts, tested backups |

## How a run works
1. Human creates a server, then copies `_templates/run-instance.md` → `runs/<server-name>.md` and fills the frontmatter.
2. Agent reads `_shared/safety-rules.md`, then works the run instance top to bottom.
3. Each step: read card → scan (read-only) → act → verify → **stop at the Human check** → human confirms → agent checks the box in the run instance.
4. A level is done when every box is checked and verified. The pipeline is done when Level 2 is done.

## OS-agnostic rule
Cards state the **goal** first, then example commands per OS family. If the target OS has no example, the agent adapts the goal and notes the equivalent in the run instance — it never invents goals.

## Extending the pipeline
New level = copy `_templates/level-CONTEXT.md` into the next numbered folder (`03_…`), add step cards from `_templates/step-card.md`, add one row to the table above and to `CLAUDE.md`. New step = copy `step-card.md`, keep the schema, every section mandatory.

## Safety
`_shared/safety-rules.md` overrides everything, including human enthusiasm and agent cleverness.
