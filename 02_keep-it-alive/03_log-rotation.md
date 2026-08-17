---
id: 02-03
title: Log rotation
level: 2
risk: low
reversible: yes
downtime: brief (container runtime restart if configuring Docker)
time: ~10 min
os: any
---

# 03 — Log rotation

## 🐣 The simple version
Programs write diary entries (logs) constantly. Without limits, a chatty or error-looping program can fill your entire disk in a night — and a full disk crashes databases and deploys in the ugliest ways. **Log rotation** means: keep the last few diary volumes, throw away the rest, automatically. Disk stays predictable forever.

## 💀 The techie version
**Goal:** every log source has a size/count cap.

System logs: `logrotate` + journald already rotate on all major distros — verify, don't rebuild:
```bash
journalctl --disk-usage                 # bounded by SystemMaxUse in /etc/systemd/journald.conf
ls /etc/logrotate.d/ | head             # entries exist
```

**Docker (the usual silent killer):** the default json-file driver is *unbounded*. Cap it:
```json
// /etc/docker/daemon.json
{"log-driver": "json-file", "log-opts": {"max-size": "50m", "max-file": "3"}}
```
Then `sudo systemctl restart docker` (brief blip; containers with restart policies return). Note: applies to containers **created after** the change — recreate existing ones on your next deploy to adopt it.

## ✅ Verify it worked
```bash
cat /etc/docker/daemon.json                              # cap present (if using Docker)
sudo docker inspect <any-new-container> --format '{{.HostConfig.LogConfig.Config}}'  # shows max-size
journalctl --disk-usage                                  # sane number, not GBs runaway
```

## 🚪 Human check
If Docker was restarted: your apps are back up (check your site / `docker ps`). Confirm, then proceed.

## 🧯 If it went wrong
A malformed `daemon.json` prevents Docker from starting — fix the JSON (`python3 -m json.tool < /etc/docker/daemon.json` to validate) or delete the file and restart Docker. System logrotate was never touched.
