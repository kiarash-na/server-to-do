---
id: 04-07
title: Close the loop — re-verify Levels 1–3
level: 4
risk: low
reversible: yes
downtime: none
time: ~20 min
os: any
---

# 07 — Close the loop: re-verify Levels 1–3

## 🐣 The simple version
You just bolted a whole new layer onto the server — and Levels 1–3 were finished *before* it existed. The firewall audit predates Docker's port tricks. The backup list has never heard of Dokploy's own database. The uptime monitor is still watching a server, not your app. This card is a short audit that pulls the new layer under the old protections. Skip it, and you have a great app on a silently unprotected server — which is worse than no app, because now there's something to lose.

## 💀 The techie version
**Goal:** Levels 1–3 re-verified against the post-Dokploy server, with the run folder updated to match. Four checkpoints:

1. **Firewall (Level 1).** Docker-published ports bypass ufw, so the cloud firewall from card 04 is now the enforcing layer. Its inbound allow-list must be *exactly* `22, 80, 443` — both IPv4 and IPv6. Compare against reality: `sudo docker ps` → the PORTS column. Every published port is either 80/443, explained in the run folder, or removed.
2. **Inventory (card 03-02).** Add: Dokploy's own data (its postgres volume + `/etc/dokploy`), every app database, every upload/data volume (`sudo docker volume ls`). The inventory card already says "re-run me when you add an app" — this is that moment.
3. **Backups (cards 03-03 / 03-04).** Extend the off-box job to the new paths; add a dump schedule for *each* new database — Dokploy's own postgres included, since it holds your entire panel configuration.
4. **Monitoring (card 02-04).** One external check per public app URL, and a fresh test alert (the monitor proving it still knows how to reach you).

## ✅ Verify it worked
```bash
sudo docker ps --format '{{.Names}} {{.Ports}}'   # every published port: 80/443 or explained
sudo docker volume ls                              # every volume: on the inventory or intentionally rebuildable
```
Plus, in the run folder: the inventory lists the Dokploy + app volumes, and the level-3 notes show the extended backup job. The monitoring service shows the new check **green**.

## 🚪 Human check
You read two things with your own eyes: the updated inventory (nothing you care about missing) and the cloud firewall's rule list (three inbound rules, nothing more) — and the uptime monitor shows your app green.

## 🧯 If it went wrong
Nothing new was created here, so the risks are old ones in new places: a firewall-rule mistake → card 01-01's recovery and `../../_shared/verify-and-recover.md`; a missed backup path → caught by this card instead of on disaster day, which is precisely the point.
