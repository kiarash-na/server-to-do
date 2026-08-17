---
id: 03-05
title: The restore drill — prove it or it doesn't count
level: 3
risk: medium (drill touches data — always into scratch, never over live)
reversible: yes
downtime: none
time: ~30 min
os: any
---

# 05 — The restore drill: prove it or it doesn't count

## 🐣 The simple version
Everyone who has lost data learned this sentence the hard way: **an untested backup is not a backup — it's a hope.** So the final step isn't setting anything up; it's *practicing the disaster*. Take last night's backup, bring it back in a safe sandbox (never on top of the real thing), and look at real, actual data with your own eyes. ⚠️ The one rule that makes this safe: restores go into a **scratch** place — a temporary database, a temporary folder — never over live data.

## 💀 The techie version
**Goal:** one real restore, verified with real data, recorded in the run folder.

1. **Pick a recent backup** — last night's dump or snapshot from cards 01/03/04.
2. **Restore into scratch only:** a throwaway database/container (`docker exec` a restore into `scratch_restore` DB), a temp directory for files. Live data is never the target.
3. **Prove it's real:** run one query against the restored DB and see rows you recognize; open a restored file and see its content. Row count `> 0`, file non-empty.
4. **Record it:** date + what was restored → run folder `summary.md`. Then delete the scratch copy.

Repeat the drill occasionally — quarterly is fine. The drill *is* the step; the schedules from cards 01–04 are just plumbing.

## ✅ Verify it worked
A query against the *restored* copy returns real rows (or a restored file shows real content) — output seen, not assumed. The `summary.md` line for this step names what was restored and the date of the backup it came from.

## 🚪 Human check
The human watched real data come back from a restored backup. Until that moment, this box stays unchecked — no matter how green the backup dashboard looks. This is also the end-of-level gate: the pipeline is done when recovery is *proven*, not configured.

## 🧯 If it went wrong
A failed drill is the **best possible news**: you found out *now*, with zero data lost. Fix the backup job, re-run it, re-drill. Common causes: dumps silently empty for weeks, wrong credentials at the destination, "backups" that were only ever local. Never delete or overwrite your only copy during experiments — scratch means scratch.
