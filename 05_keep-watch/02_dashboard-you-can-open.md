---
id: 05-02
title: A dashboard you can open
level: 5
risk: low
reversible: yes
downtime: none
time: ~20 min
os: any
---

# 02 — A dashboard you can open

## 🐣 The simple version
Numbers piling up on the server are useless if looking at them requires a black terminal and three commands. A **dashboard** turns the vital signs from card 01 into graphs you can open in a browser — "disk at 61% and climbing" becomes a picture you understand in two seconds. You might already have one: your provider's console and panels like Dokploy show graphs out of the box. The only hard rule: however you reach the graphs, that path must be private — never a new door open to the internet.

## 💀 The techie version
**Goal:** the human can browse historical graphs of the card-01 metrics from their own device, **without any new publicly exposed port**.

The tool stays deliberately open — *any* of these satisfies the card, roughly in order of effort:

**Already covered?**
- **Provider console** — its metrics/monitoring tab *is* a dashboard, private by design (behind your provider login — 2FA on). If card 01 was covered this way, this card usually is too: verify and check the box as `covered-by: provider console`.
- **Dokploy / PaaS** (Level 4) — its monitoring views cover the container side, reached through your already-locked-down panel (Level 4 card 04).

**Or pick your own** — suggestions, not prescriptions:
- **Nothing new**: an **SSH tunnel** to the card-01 tool's built-in UI — `ssh -L 19999:localhost:19999 user@server`, open `http://localhost:19999` locally. Zero install, zero exposure (e.g. Netdata's UI, but any local web UI works this way).
- **Private overlay network** (Tailscale/WireGuard): bind the UI to the overlay IP — reachable from your devices only.
- **Authenticated reverse proxy** (if Level 4 landed): route a UI through the existing Traefik/Dokploy layer on a subdomain **with auth in front**.
- **Dedicated dashboard** (e.g. Grafana reading Prometheus/node_exporter) if you want custom graphs — runs fine as a container; follow its official docs for the data source.

## ✅ Verify it worked
From your laptop: the dashboard loads through your chosen path and shows graphs **with history** (not just the current instant).
From outside (phone on mobile data): anything self-hosted is dead to the public internet. (Provider-console dashboards are exempt — their "private path" is the provider login itself.)

## 🚪 Human check
The human personally opens the graphs on their own device and sees at least one metric with real history — and can state *how* they got in (console login, tunnel, panel…), proving the path is one only they have.

## 🧯 If it went wrong
Read-only by nature; nothing on the server breaks. Tunnel won't load? Check the card-01 service is running and the local port isn't taken. Accidentally exposed something? Remove the proxy route/firewall rule — the metric history itself is harmless.
