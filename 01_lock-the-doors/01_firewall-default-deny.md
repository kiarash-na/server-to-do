---
id: 01-01
title: Firewall — default deny
level: 1
risk: high
reversible: yes (while a session is open)
downtime: none
time: ~10 min
os: any (ufw / firewalld / nftables examples)
---

# 01 — Firewall: default deny, allow only what you need

## 🐣 The simple version
Your server has thousands of invisible "doors" (ports), and any program can open one. A firewall is the bouncer: **every door is closed by default**, and you hand it a guest list — typically just SSH (22) and web (80/443). Anything not on the list never even reaches your programs.

⚠️ The #1 self-inflicted server injury is firewalling yourself out. We allow SSH **before** we turn the bouncer on, and we test from a new session before trusting it.

## 💀 The techie version
**Goal:** incoming default-deny; allow only 22 (+80/443 if web-serving); outgoing allowed.

```bash
# Debian/Ubuntu (ufw):
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp        # BEFORE enabling. If sshd is on a custom port, use that.
sudo ufw allow 80/tcp && sudo ufw allow 443/tcp   # only if serving web
sudo ufw enable              # answer y — your current session survives

# RHEL/Fedora (firewalld): default zone is already deny-by-default;
sudo firewall-cmd --permanent --add-service=ssh && sudo firewall-cmd --reload

# Minimalist (nftables): write a ruleset with policy drop; same guest-list logic.
```

> ⚠️ **Docker users:** Docker-published ports (`-p 8080:80`) **bypass ufw** entirely — they punch through at the NAT layer before ufw sees them. Either bind to localhost (`-p 127.0.0.1:8080:80`), front them with a reverse proxy, or enforce at your provider's cloud firewall. Cloud firewalls (Hetzner/AWS SG/etc.) are the stronger layer anyway — use both.

## ✅ Verify it worked
```bash
sudo ufw status verbose        # Status: active, Default: deny (incoming)
# From another machine:  nc -zv <server-ip> 22 (succeeds)  vs  nc -zv <server-ip> 23 (times out)
```

## 🚪 Human check
With the firewall **enabled**, open a brand-new SSH session. Only after that succeeds do you confirm the step.

## 🧯 If it went wrong
Still-open session: `sudo ufw disable` and re-do the allow rules. Locked out fully: provider web console → `ufw disable`. See `../../_shared/verify-and-recover.md`.
