---
id: 02-05
title: Backups — and the restore drill
level: 2
risk: none (configuration) / medium (drill touches data)
reversible: yes
downtime: none
time: ~45 min (including the drill)
os: any
---

# 05 — Backups, and the restore drill that proves them

## 🐣 The simple version
Everything before this step protects the server. This step protects you *from losing the server entirely* — disk death, fat-fingered command, bad deploy, provider incident. The rule professionals use is **3-2-1**: 3 copies of your data, on 2 different systems, 1 of them somewhere else entirely. And the secret everyone learns the hard way: **an untested backup is not a backup** — it's a hope. So this step ends with you actually restoring one.

## 💀 The techie version
**Goal:** automated, off-box, redundant backups of everything that can't be rebuilt — plus one proven restore.

What needs backing up (think in *data*, not servers):
1. **Databases** — scheduled dumps (`pg_dump`, `mysqldump`), not raw file copies
2. **Volumes / uploaded files / app state**
3. **Config** — env files, compose files, DNS notes (this repo's run instance helps here)

Layers that work well together:
- **App/platform level:** your panel or scripts dump DBs + volumes to object storage (S3-compatible, provider storage box…)
- **Machine level:** provider snapshots (Hetzner/AWS/etc.) — the blunt, complete undo button
- **Redundancy:** two different destinations, so one company's bad day can't take both

Automation: schedule it (cron, panel scheduler, snapshot automation). A backup you must remember to run is a backup that stops happening.

## ✅ Verify it worked — the restore drill
1. Pick last night's DB dump. Restore it into a **scratch** database/container (never over live data).
2. Run one query against the restored data and see real rows.
3. Record the date + what you restored in the run instance.

Repeat the drill occasionally (quarterly is fine). The drill *is* the step; the schedule is just plumbing.

## 🚪 Human check
The human watched real data come back from a restored backup. Until that moment, this box stays unchecked — no matter how green the backup dashboard looks.

## 🧯 If it went wrong
Drill failed = the best possible news: you found out *now*. Fix the backup job, re-run, re-drill. Never delete or overwrite your only copy during experiments.
