---
id: 01-03
title: Automatic security updates
level: 1
risk: low
reversible: yes
downtime: none
time: ~10 min
os: any
---

# 03 — Automatic security updates

## 🐣 The simple version
New security holes are found every week. You will forget to patch. The server won't. This step makes security fixes install themselves overnight, so "did I patch that thing in the news?" is always answered with "already done".

## 💀 The techie version
**Goal:** security updates applied automatically on a schedule; reboots stay manual (or scheduled) — never mid-day surprises.

```bash
# Debian/Ubuntu:
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure -plow unattended-upgrades   # answer Yes
# Config: /etc/apt/apt.conf.d/50unattended-upgrades (security origins are on by default)

# RHEL/Fedora:
sudo dnf install dnf-automatic -y
# /etc/dnf/automatic.conf → apply_updates = yes
sudo systemctl enable --now dnf-automatic.timer

# Alpine: apk add apk-cron equivalents vary; a weekly `apk upgrade` cron is acceptable.
```

## ✅ Verify it worked
```bash
systemctl is-active unattended-upgrades   # or dnf-automatic.timer → active
# Debian/Ubuntu dry run proving config parses:
sudo unattended-upgrade --dry-run --debug 2>&1 | tail -3
```

## 🚪 Human check
The service/timer is `active`. That's the whole gate — this step is low-risk.

## 🧯 If it went wrong
Nothing here can strand you. A bad auto-update is handled by the distro's package tools (`apt --fix-broken install`); the restore drill in Level 2 card 05 is the deeper safety net.
