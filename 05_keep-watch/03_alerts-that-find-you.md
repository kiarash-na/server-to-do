---
id: 05-03
title: Alerts that find you
level: 5
risk: none
reversible: yes
downtime: none
time: ~20 min
os: any
---

# 03 — Alerts that find you

## 🐣 The simple version
A dashboard you have to remember to open is a chore you'll abandon by week two. Flip it around: the *server* watches the graphs, and when something crosses a line — disk almost full, memory pinned, an app down — it **messages you** on a channel you actually read. You stop watching the server; the server watches itself. Like the uptime monitor in Level 2, an alert you've never received is a rumor, so this card ends with a real test message on your real device.

## 💀 The techie version
**Goal:** threshold breaches on the card-01 metrics push notifications to a channel the human reads daily — proven by a live test.

Minimum alert set (goal-first; names vary by tool):
- **Disk** > 85% on any filesystem (the classic silent killer)
- **RAM** > 90% sustained (minutes, not a spike) and/or swap in heavy use
- **Load/CPU** pegged for > 5 min
- **TLS cert** expiry < 14 days (if Level 4 / a domain exists)
- **Container/service down** (if Level 4 landed — a dead container is a dead app)

Channels: pick one the human genuinely reads — email **plus** Telegram/Discord/Slack/push where possible. The tool stays open: **provider metric alerts** (many consoles alert on CPU/disk/bandwidth by email or webhook), **Netdata alarm notifications** (built on cards 01–02), **Grafana alerting**, **Uptime Kuma** (monitors + alerts in one), or a small cron script — all fine if the goal lands. **Already covered?** If your provider's alerting can cover the host-side thresholds (disk/RAM/CPU) and a test alert arrives, check those off as `covered-by: provider` — container/cert alerts may still need an on-box tool if you have apps (Level 4). Keep alert credentials in env/config outside any repo.

## ✅ Verify it worked
Trigger a real test: most tools have a "send test notification" (e.g. Netdata's `alarm-notify.sh test`), or temporarily set an absurd threshold (disk > 1%) and wait for the fire. The notification must **arrive**, not just be sent — check the actual device/inbox, then restore the sane threshold.

## 🚪 Human check
The human received the test alert on a device they carry, and can say which channel future alerts will arrive on.

## 🧯 If it went wrong
Nothing on the server was harmed. Alert not arriving: check spam, bot tokens/chat IDs, and the tool's notification logs. Restore any temporary threshold you set — a forgotten disk > 1% alarm is a pager that never shuts up.
