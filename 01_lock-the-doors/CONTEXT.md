# 01_lock-the-doors — firewall, brute-force protection, auto-updates, log awareness, hidden origin IP

Level 0 made entry safe for *you*. This level makes the server hostile to everyone else: a firewall that refuses everything by default, a bouncer that bans password-guessers, security patches that install themselves, the habit of looking at who's knocking — and finally, the server's real IP disappearing behind Cloudflare.

## Inputs
**Working (this run):**
- `../runs/<server-name>/` — run folder; `level-0.md` boxes must all be checked
**Reference (every run):**
- `../_shared/safety-rules.md`
- `../_shared/verify-and-recover.md`

## Process
Cards `01` → `05`, in order. Card 01 (firewall) is the second lockout-risk step in this pipeline — its double-gate (test SSH from a new session before trusting the ruleset) is mandatory, not advisory. Card 05 (Cloudflare lock) assumes a public website; no website → mark it n/a and move on.

## Outputs
- Updated checkboxes + notes in `../runs/<server-name>/level-1.md`, one new line in `summary.md` per step

## Human check
After card 05: the human has seen their real auth-log numbers (there will be attacks — that's normal), confirms a fresh SSH session still works with the firewall active, and has watched their own website load through Cloudflare while `dig` shows no origin IP.
