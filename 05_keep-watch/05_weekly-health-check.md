---
id: 05-05
title: The weekly health check
level: 5
risk: none
reversible: yes
downtime: none
time: ~10 min
os: any
---

# 05 — The weekly health check

## 🐣 The simple version
All the instruments are in — this card is the habit that makes them matter. Once a week (suggested; run it any time), the agent takes a **read-only snapshot of the server's health** — vitals, security quick-look, backup freshness, service state — and files it as a dated report in the run folder. Five minutes of reading, and you always know your server's story: what's normal, what's drifting, what needs attention before it's an emergency. Every command is look-don't-touch, so this is the safest card in the whole repo.

## 💀 The techie version
**Goal:** an on-demand, read-only health report saved to `runs/<server-name>/health/YYYY-MM-DD_HHMM_health.md`, filled in from `_templates/health-report.md`.

The report is **ICM-structured**: one folder per server already exists (`runs/<server-name>/`), reports live in its `health/` subfolder, filename carries the timestamp, frontmatter carries the server name — so any future agent can read the series newest-first and spot trends without re-scanning the box.

How to fill it (all read-only — examples per goal, adapt per OS):
- **Vitals:** `uptime`, `free -h`, `df -h`, `vmstat 1 3`
- **Security quick-look:** failed SSH logins (`journalctl -u ssh --since "7 days ago" | grep -c Failed`), fail2ban counters, pending security updates
- **Backup freshness:** last provider snapshot date, last off-box backup, last DB dump (from Level 3 arrangements / run-folder notes)
- **Services:** `docker ps` (running vs. expected, restart counts), uptime-monitor status
- **Alerts:** what fired since the last report (from card 03's tool)
- **Notes & trend:** compare against the previous report in `health/` — disk growth, RAM creep, anything new

Cadence: weekly is the suggestion; the human can ask for a report any time ("run the health check on `runs/<server>/`") and the same card applies. One summary.md line per *card*, not per report — the reports speak for themselves in `health/`.

## ✅ Verify it worked
```bash
ls -t ../../runs/<server-name>/health/    # newest report on top, timestamped filename
head -8 <newest report>                   # frontmatter: server name + generated timestamp
```
Every section filled with real numbers (or a stated `n/a` with reason) — no placeholders left.

## 🚪 Human check
The human reads the first generated report end-to-end and confirms they understand what each section would look like when something is *wrong*.

## 🧯 If it went wrong
Impossible to break the server — pure reads. A wrong report? Delete the file, rerun. Missing data in a section means that level's arrangement decayed (e.g. backups stopped) — that's the report doing its job; fix the underlying level card.
