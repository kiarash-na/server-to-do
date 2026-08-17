---
id: 04-02
title: Container runtime — Docker
level: 4
risk: low
reversible: yes
downtime: none
time: ~10 min
os: any (Debian/Ubuntu example)
---

# 02 — Container runtime: Docker

## 🐣 The simple version
Modern apps ship in **containers** — sealed boxes holding the app plus everything it needs, so "works on my laptop" finally means "works on the server." Docker is the program that runs the boxes, and Dokploy (next card) does all of its work through Docker — this is its foundation. ⚠️ One trap to sidestep now: tutorials love adding your user to the "docker group." That group is **silently equal to full root access** — it would quietly saw off the protections Levels 0–1 built. This card deliberately does *not* do it; typing `sudo docker …` is the small price for keeping those guarantees.

## 💀 The techie version
**Goal:** Docker Engine + the Compose plugin installed from the **official Docker apt repository** (so `unattended-upgrades` from card 01-03 keeps it patched), service enabled and running.

Follow the official docs — <https://docs.docker.com/engine/install/> → your distro → "Install using the apt repository." Read them first, don't run from memory; the distro packages (`docker.io`) also work but lag behind upstream.

Do **not**: `sudo usermod -aG docker $USER` — docker-group membership is passwordless root. If a tutorial demands it, use `sudo` instead.

## ✅ Verify it worked
```bash
sudo docker run --rm hello-world   # prints "Hello from Docker!"
systemctl is-active docker          # active
docker compose version              # Docker Compose version v2.x (plugin, not old docker-compose)
```

## 🚪 Human check
You saw "Hello from Docker!" in the real output. That's the gate — the next card is where things get spicy.

## 🧯 If it went wrong
Fully reversible: `sudo apt purge docker-ce docker-ce-cli containerd.io docker-compose-plugin` (Debian/Ubuntu). Service not `active` → `sudo journalctl -u docker` says why; the classic cause on small VPSs is a leftover distro `docker.io` package fighting the official one — purge one, keep the other.
