---
id: 02-04
title: External uptime monitoring
level: 2
risk: none
reversible: yes
downtime: none
time: ~15 min
os: n/a (external service)
---

# 04 — External uptime monitoring

## 🐣 The simple version
If your server dies at 3am, how do you find out? From an angry user? Tomorrow morning? An **uptime monitor** is a service *somewhere else on the internet* that knocks on your server's door every 30 seconds and messages you the moment nobody answers. The key word is **external** — a monitor running on your server dies together with your server.

## 💀 The techie version
**Goal:** an off-box checker hits a public URL (or port) on an interval and alerts you via a channel you actually read.

What to configure, in any tool you like (UptimeRobot, Better Stack, Healthchecks.io, self-hosted Uptime Kuma on *another* machine, …):
- **Check:** HTTPS request to your main URL, expect status 200, interval ≤ 1 min
- **Alerts:** email **plus** one push channel (SMS/Slack/Discord/app) — test it
- **Coverage:** one check per public service, not just the homepage
- Optional but excellent: a **status page** for future users

The tool is deliberately not prescribed — the *arrangement* is the step. Vendor tutorials age; the goal doesn't.

## ✅ Verify it worked
Trigger a real alert test (most tools have one, or briefly point the check at a dead port) and confirm the notification arrives on your phone/inbox. A monitor that has never successfully alerted you is a rumor, not a monitor.

## 🚪 Human check
You received the test alert on a device you carry. That's the gate.

## 🧯 If it went wrong
Nothing on the server was touched. If alerts don't arrive, it's a config issue in the monitoring service — check spam folders and notification permissions.
