# 02_keep-it-alive — swap, time, logs, monitoring

Security (Level 1) keeps strangers out. This level keeps the *server itself* from failing you: memory emergencies absorbed, clocks agreeing, disks that can't silently fill, and an outside watcher that texts you when things die.

## Inputs
**Working (this run):**
- `../runs/<server-name>/` — run folder; `level-0.md` and `level-1.md` must be fully checked
**Reference (every run):**
- `../_shared/safety-rules.md`
- `../_shared/glossary.md`

## Process
Cards `01` → `04`, in order. Cards 01–03 are quick system tweaks; 04 is about an *arrangement that exists*, not a specific vendor — the goal is stated, the tool is your choice.

## Outputs
- Updated checkboxes + notes in `../runs/<server-name>/level-2.md`, one new line in `summary.md` per step

## Human check
After card 04: the human has received one real alert-test from their uptime monitor — an alert that arrived on *their* phone/inbox, not a green dashboard.
