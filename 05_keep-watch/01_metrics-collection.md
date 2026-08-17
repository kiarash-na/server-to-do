---
id: 05-01
title: Metrics collection
level: 5
risk: low
reversible: yes
downtime: none
time: ~20 min
os: any
---

# 01 — Metrics collection

## 🐣 The simple version
Right now, if your server gets slow at 2am, there's no recording of what happened — no way to know if it was memory, disk, or one greedy app. A **metrics collector** is a tiny background program that takes your server's vital signs (CPU, RAM, disk, network) every few seconds and keeps the history. It's a fitbit for your server. ⚠️ One caution: the vital-signs page reveals details about your machine, so it must stay private — reachable from localhost or through your firewall, never open to the internet. The card is designed that way.

## 💀 The techie version
**Goal:** an always-on agent samples system metrics (CPU, RAM, swap, disk I/O + usage, network, per-process/per-container) and retains history, with its endpoint **not publicly reachable**.

**Already covered?** Check before installing anything:
- **Provider console** — most VPS providers (Hetzner, DigitalOcean, …) collect host metrics automatically; look for a "Metrics"/"Monitoring" tab. Covers CPU/RAM/disk/network; per-process usually not — decide if that gap matters to you.
- **Dokploy / PaaS layer** (Level 4) — shows per-container CPU/RAM stats, which covers the app half.

If the goal is met by what exists, skip to **Verify** with the existing tool and check the box as `covered-by: ___`. Otherwise, example self-hosted paths, pick one (goal-first; adapt per OS):
- **Netdata** — one package, covers metrics + dashboard + alarms (cards 01–03). Distro package or official install script (show any curl-pipe install verbatim and get approval first — safety rule 3). Listens on `:19999`; bind it to localhost or restrict with the Level 1 firewall.
- **node_exporter** (Prometheus ecosystem) — single binary, metrics on `:9100`. Pairs with card 02's Grafana. Also localhost/firewall-only.
- Docker available (Level 4)? Both run fine as containers — but a container seeing host metrics needs host mounts (`/proc`, `/sys`); follow the tool's official docs, don't guess flags.

Whichever you pick: enable it as a service so it survives reboots.

## ✅ Verify it worked
```bash
systemctl status netdata            # or node_exporter — active (running)
curl -s localhost:19999/api/v1/info | head    # Netdata answers (adjust port/tool)
curl -s localhost:9100/metrics | head         # node_exporter answers
ss -tlnp | grep -E '19999|9100'     # bound to 127.0.0.1, NOT 0.0.0.0 — or firewall blocks it
```
From an outside machine (or your phone on mobile data): the port must be **unreachable**.

**Provider-covered instead?** Open the console's metrics tab: graphs exist, show history (not just the current instant), and keep updating. The "not publicly reachable" goal is met by design — metrics sit behind your provider login, which should have 2FA on.

## 🚪 Human check
The human confirms (or watches the agent prove) that the metrics endpoint answers from the server but is dead from the public internet.

## 🧯 If it went wrong
Nothing destructive happened. Stop/disable the service (`sudo systemctl disable --now <service>`), uninstall the package, remove any firewall exception you added. Port accidentally public? Close it first (firewall rule or rebind), then continue.
