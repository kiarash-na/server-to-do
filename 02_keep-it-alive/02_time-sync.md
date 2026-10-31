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
Re-run this after **every provider-firewall change** (card 01-05). That's when time sync breaks silently.

## 🚪 Human check
Both lines say yes/active. Instant gate.

## 🧯 If it went wrong
If sync shows `no`:
1. Outbound UDP 123 isn't blocked by Level 1's rules. Outgoing should be allowed; re-check card 01-01.
2. Check that replies actually arrive: `sudo chronyc ntpdata | grep -E "Remote address|Total RX"`. If every source shows `Total RX: 0` and `ufw` logs no blocks for them, an **upstream provider firewall** is dropping the replies. Some provider firewalls track connection state for TCP only, so UDP replies (NTP, DNS to outside resolvers) need an explicit inbound rule. Read your provider's firewall docs to confirm.
   - Pin chrony's client port. Add `acquisitionport 1123` in a file under `/etc/chrony/conf.d/`, then `sudo systemctl restart chrony` (directive: chrony.conf docs, <https://chrony-project.org/documentation.html>).
   - Add a provider inbound rule: UDP, source port 123 → destination port 1123, accept, for IPv4 and IPv6. Save it.
   - This exposes nothing new. chrony is client-only, NTS rejects forged replies, and ufw still drops anything unsolicited.

Worst case it drifts slowly; nothing breaks today.
