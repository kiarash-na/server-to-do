---
id: 02-02
title: Time sync
level: 2
risk: none
reversible: yes
downtime: none
time: ~5 min
os: any
---

# 02 — Time sync

## 🐣 The simple version
If your server's clock drifts, weird things happen: logins fail for no reason, certificates look expired, logs from two machines can't be compared, scheduled jobs fire at the wrong moment. **Time sync** makes your server quietly ask official time servers "what time is it?" every few minutes, forever. Set it once, never think about it again.

## 💀 The techie version
**Goal:** an NTP client enabled and synchronized; timezone set deliberately (UTC is the sane server default).

```bash
# systemd systems (most distros) — usually already on:
sudo timedatectl set-ntp true
sudo timedatectl set-timezone UTC        # optional but recommended

# If missing: Debian/Ubuntu → sudo apt install chrony;  RHEL → chrony is default;
# Alpine → setup-ntp. Enable and start the service.
```

## ✅ Verify it worked
```bash
timedatectl | grep -E "synchronized|NTP"
# Expected: System clock synchronized: yes / NTP service: active
```

## 🚪 Human check
Both lines say yes/active. Instant gate.

## 🧯 If it went wrong
If sync shows `no`: check outbound UDP 123 isn't blocked by the firewall rules from Level 1 (outgoing should be allowed — re-check card 01-01). Worst case it drifts slowly; nothing breaks today.
