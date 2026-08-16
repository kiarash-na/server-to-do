---
id: 01-04
title: Know who is knocking
level: 1
risk: none
reversible: n/a (read-only)
downtime: none
time: ~5 min
os: any
---

# 04 — Know who is knocking

## 🧒 The simple version
This step changes nothing — it just *shows* you the internet. You'll look at your login log and see hundreds or thousands of failed attempts from strangers. That's not a breach; that's background radiation. Seeing it once teaches you two things permanently: why Levels 0–1 mattered, and what "normal" looks like — so one day you'll spot "not normal".

## 🛠 The techie version
**Goal:** read the auth log; confirm failures exist but zero unexpected successes.

```bash
# systemd systems:
sudo journalctl -u ssh --since "24 hours ago" | grep -cE "Invalid user|Failed"
sudo journalctl -u ssh --no-pager | grep -oE "from [0-9.]+" | sort -u | wc -l   # unique IPs

# The line that must contain ONLY you:
sudo journalctl -u ssh --no-pager | grep "Accepted" | grep -oE "for \S+ from [0-9.]+" | sort | uniq -c

# non-systemd: same ideas against /var/log/auth.log or /var/log/secure
```
Expect: hundreds+ failures, dozens–thousands of unique IPs, `Accepted` lines only for your user from your IPs.

## ✅ Verify it worked
You've read the output and can answer: *how many unique IPs tried me? who successfully logged in?* Write both numbers into the run instance — they're your first baseline.

## 🚪 Human check
The human looks at the numbers themselves and says the magic words: "so that's why." If any `Accepted` login is unfamiliar → stop, that's an incident, not a checklist item (`../../_shared/verify-and-recover.md`).

## 🧯 If it went wrong
Read-only step; nothing to break. If the log is empty on day one, revisit after 24h — the bots always come.
