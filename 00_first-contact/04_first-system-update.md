---
id: 00-04
title: First full system update
level: 0
risk: low
reversible: partially (snapshots help)
downtime: possible (kernel update → reboot)
time: ~10–30 min
os: any
---

# 04 — First full system update

## 🧒 The simple version
Your server image was baked weeks or months ago. Since then, security holes were found and fixed — but the fixes aren't on your machine yet. This step downloads all of them in one go. Do it now, before the server matters, because it's the one moment a surprise reboot hurts nobody.

## 🛠 The techie version
**Goal:** package index refreshed, all upgrades applied, reboot if the kernel changed.

```bash
# Debian/Ubuntu:
sudo apt update && sudo apt full-upgrade -y

# RHEL/Fedora/Rocky:
sudo dnf upgrade -y

# Alpine:
apk update && apk upgrade

# Arch:
sudo pacman -Syu
```
Then:
```bash
ls /var/run/reboot-required 2>/dev/null && echo "reboot needed"   # Debian/Ubuntu signal
needs-restarting -r 2>/dev/null                                   # RHEL family signal
# If either says reboot: sudo reboot — it's safe, you have no users yet.
```

## ✅ Verify it worked
```bash
# Debian/Ubuntu: 0 upgradable packages
apt list --upgradable 2>/dev/null | grep -c upgradable
# All: you're back in after reboot (if one happened) and uname -a shows a kernel
```
Expected: `0` (or only held-back phased packages), session reconnects fine.

## 🚪 Human check
After any reboot: you logged back in as your user, unaided. That also re-proves cards 01–03 survived a restart.

## 🧯 If it went wrong
A broken upgrade almost always shows itself in `sudo apt --fix-broken install` / `sudo dnf check`. Provider snapshot restore is the big undo — `_shared/safety-rules.md` rule 7 is why we took one.
