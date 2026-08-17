---
id: 02-01
title: Swap — the OOM safety net
level: 2
risk: low
reversible: yes
downtime: none
time: ~5 min
os: any
---

# 01 — Swap: the OOM safety net

## 🐣 The simple version
When your server runs out of memory, Linux doesn't slow down — it **kills a program instantly** to survive. It might pick your database. It has done this to many people, mid-deploy, at random. **Swap** is a file on disk that acts as emergency overflow memory: slow, but slow beats murdered. On small VPSes this single file prevents the classic "why did my container die at 3am" mystery.

## 💀 The techie version
**Goal:** a swapfile of ~25–50% of RAM (2GB is right for 4–8GB RAM), active and persistent across reboots.

```bash
free -h                                  # check: swap probably 0B on a fresh VPS
sudo fallocate -l 2G /swapfile           # (or dd if fallocate unsupported)
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab   # survives reboots
```
Optional tuning: `vm.swappiness=10` in `/etc/sysctl.d/99-swap.conf` — prefer RAM, use swap only under real pressure.

## ✅ Verify it worked
```bash
free -h | grep -i swap     # Swap: 2.0Gi total
swapon --show              # /swapfile listed
grep swapfile /etc/fstab   # persistence confirmed
```
`0B used` is correct and good — swap is a seatbelt, not a seat.

## 🚪 Human check
`free -h` shows the swap line. One command, one glance, done.

## 🧯 If it went wrong
Fully reversible: `sudo swapoff /swapfile && sudo rm /swapfile`, remove the fstab line. Nothing else on the system depends on this step.
