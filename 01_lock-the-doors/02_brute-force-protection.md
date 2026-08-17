---
id: 01-02
title: Brute-force protection
level: 1
risk: low
reversible: yes
downtime: none
time: ~10 min
os: any
---

# 02 — Brute-force protection

## 🐣 The simple version
Right now, robots are typing passwords into your server — hundreds of times a day, forever. They can't get in (Level 0 killed password login), but each attempt wastes resources and clutters your logs. A **brute-force blocker** watches the logs and automatically bans any IP that fails too many times — like a bouncer who remembers faces.

## 💀 The techie version
**Goal:** repeated auth failures → temporary IP ban, applied at the firewall.

```bash
# Debian/Ubuntu:
sudo apt install fail2ban -y
sudo systemctl enable --now fail2ban
# Default sshd jail works out of the box on systemd systems.

# RHEL/Fedora:
sudo dnf install fail2ban -y
# Then enable the sshd jail in /etc/fail2ban/jail.local:
#   [sshd]
#   enabled = true
sudo systemctl enable --now fail2ban
```
Sane defaults to consider in `jail.local`: `bantime = 1h`, `maxretry = 5`, and **always** `ignoreip = 127.0.0.1/8 ::1` — add your own static IP only if you truly have one (a stale entry here locks *you* out later).

## ✅ Verify it worked
```bash
sudo fail2ban-client status            # lists jails, expect: sshd
sudo fail2ban-client status sshd       # shows failed/banned counters
```

## 🚪 Human check
The sshd jail exists and counters are non-zero (proof the bots found you already — and the bouncer is working).

## 🧯 If it went wrong
Banned yourself? From another network/phone hotspot: `sudo fail2ban-client set sshd unbanip <your-ip>`. Or wait out the bantime. No damage is permanent here.
