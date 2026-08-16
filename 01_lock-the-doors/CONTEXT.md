# 01_lock-the-doors — firewall, brute-force protection, auto-updates, log awareness

Level 0 made entry safe for *you*. This level makes the server hostile to everyone else: a firewall that refuses everything by default, a bouncer that bans password-guessers, security patches that install themselves, and the habit of looking at who's knocking.

## Inputs
**Working (this run):**
- `../runs/<server-name>.md` — run instance; Level 0 boxes must all be checked
**Reference (every run):**
- `../_shared/safety-rules.md`
- `../_shared/verify-and-recover.md`

## Process
Cards `01` → `04`, in order. Card 01 (firewall) is the second lockout-risk step in this pipeline — its double-gate (test SSH from a new session before trusting the ruleset) is mandatory, not advisory.

## Outputs
- Updated checkboxes + notes in `../runs/<server-name>.md`, section Level 1

## Human check
After card 04: the human has seen their real auth-log numbers (there will be attacks — that's normal) and confirms a fresh SSH session still works with the firewall active.
