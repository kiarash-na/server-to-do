# 02_keep-it-alive — swap, time, logs, monitoring, backups

Security (Level 1) keeps strangers out. This level keeps the *server itself* from failing you: memory emergencies absorbed, clocks agreeing, disks that can't silently fill, an outside watcher that texts you when things die, and backups you've actually tested.

## Inputs
**Working (this run):**
- `../runs/<server-name>.md` — run instance; Levels 0–1 must be fully checked
**Reference (every run):**
- `../_shared/safety-rules.md`
- `../_shared/glossary.md`

## Process
Cards `01` → `05`, in order. Cards 01–03 are quick system tweaks; 04–05 are about *arrangements that exist*, not specific vendors — the goal is stated, the tool is your choice.

## Outputs
- Updated checkboxes + notes in `../runs/<server-name>.md`, section Level 2

## Human check
After card 05: the human has (a) received one real alert-test from their uptime monitor and (b) watched one backup actually restore. A pipeline is done when recovery is *proven*, not configured.
