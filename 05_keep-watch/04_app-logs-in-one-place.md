---
id: 05-04
title: App logs in one place
level: 5
risk: low
reversible: yes
downtime: none
time: ~15 min
os: any
---

# 04 — App logs in one place

## 🐣 The simple version
When your app misbehaves, the explanation is almost always in its diary — the logs. But diaries are scattered: one per app, one per container, plus the system's own. Hunting through them over SSH at 11pm is archaeology. A **log viewer** gathers the recent diaries into one searchable page: pick an app, scroll, search, done. ⚠️ Diaries can contain secrets (tokens, emails), so this page follows the same rule as the dashboard: private access only.

## 💀 The techie version
**Goal:** recent logs from every service/container on the box are browsable and searchable from one UI, reached via a private path (SSH tunnel, overlay network, or authenticated proxy — card 02's rules).

Example paths, pick one:

**Already covered?**
- **Dokploy / PaaS** (Level 4) — per-service log views in the panel you already locked down. If your apps all live there, the goal may be met: run **Verify** against the panel and check the box as `covered-by: dokploy`.

**Or pick your own** — suggestions, not prescriptions:
- **Dozzle** — one small container, live web UI over all Docker logs. The 5-minute win if you have Docker. Bind privately or front it with auth.
- **Grafana Loki** — log aggregation with labels + search, reuses a Grafana from card 02. Heavier, more powerful.
- **The floor** (no new software): `journalctl -u <service>` + `docker logs <container>` are searchable via `grep` — acceptable only if services are few; the UI options are worth it past two apps.

Rotation is already handled (Level 2 card 03) — this card is about *reading*, not storing.

## ✅ Verify it worked
Open the viewer through its private path; confirm: (a) every running container/service appears, (b) new log lines show up live (restart an app and watch), (c) search finds a known string. From the public internet: anything self-hosted is unreachable. (Panel-covered? Privacy was already proven in Level 4 card 04 — just confirm (a)–(c) in the panel.)

## 🚪 Human check
The human finds one real log line from their own app in the viewer — e.g. a startup message or a request they just made — using the private access path themselves.

## 🧯 If it went wrong
Pure addition; nothing existing was modified. Viewer misbehaving: stop/remove its container or package. Exposed publicly: pull the route/firewall rule immediately — treat anything that flowed through it as potentially seen (Level 1 mindset).
