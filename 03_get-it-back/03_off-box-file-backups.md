---
id: 03-03
title: Off-box file backups — a second home for your data
level: 3
risk: low (reads data; sends it off-box)
reversible: yes
downtime: none
time: ~30 min
os: any
---

# 03 — Off-box file backups: a second home for your data

## 🐣 The simple version
Your provider snapshot (card 01) lives in the same building as your server. This step puts a copy in a *different* building: your files and volumes, copied automatically on a schedule to storage that isn't this provider — object storage (any S3-compatible), a cheap storage box, even another server you own. ⚠️ Because this copy travels over the network and sits on someone else's disk, it must be **encrypted** — the card's design makes encryption part of the setup, not an afterthought.

## 💀 The techie version
**Goal:** the file/volume/config items from the card-02 inventory, copied on a schedule to a destination outside this provider, encrypted at rest, with retention.

- **Pick a destination:** S3-compatible object storage, a storage box, a second machine — anything that's *not this provider's snapshot system*.
- **Pick a tool with encryption built in:** restic, borg, kopia, or rclone with a crypt remote are the usual choices. Read the chosen tool's official docs; never pipe raw tarballs to untrusted storage.
- **Schedule it:** cron/systemd timer, daily is the sane default. A backup you must remember to run is a backup that stops happening.
- **Retention:** keep a spread (e.g. 7 daily, 4 weekly) — you need to reach back past the day you noticed the problem.

## ✅ Verify it worked
1. Run the backup job **once by hand** — it must complete clean before the schedule is trusted.
2. List what's at the destination (restic `snapshots`, rclone `ls`, …) — you see a real, dated backup containing the inventory's paths.
3. The schedule entry exists (crontab/timer) — automation confirmed, not just a manual run.

## 🚪 Human check
The human knows *where the second home is* — they can name the destination and see the fresh backup listed there. (Knowing how to get data *back* out is card 05's job.)

## 🧯 If it went wrong
Fully reversible: remove the schedule entry, delete the destination data if you like. A failed first run is normal (permissions, paths inside containers) — fix, re-run, only then trust the schedule. Never point the job at a destination that already holds your only copy of something.
