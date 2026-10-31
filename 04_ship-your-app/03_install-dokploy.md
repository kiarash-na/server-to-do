---
id: 04-03
title: Install Dokploy
level: 4
risk: medium
reversible: partially
downtime: none
time: ~15 min
os: any with Docker
---

# 03 — Install Dokploy: the control panel

## 🐣 The simple version
Dokploy is the control panel that turns "I pushed code" into "my app is live": it pulls your repo, builds it, puts it behind your domain with HTTPS, and shows you logs and restart buttons. You install it once, then mostly use its web UI and API. ⚠️ This is the **only card in the whole pipeline that runs a script from the internet as root** — the most powerful and least reversible kind of command there is. That's a trust decision, not a command: the human sees the exact line and says yes before it runs. If that yes doesn't come, the level stops here.

## 💀 The techie version
**Goal:** Dokploy installed and its stack healthy, per the official docs.

**Read <https://docs.dokploy.com> → installation first** — the canonical installer lives there and changes over time. At writing time it is:
```bash
curl -sSL https://dokploy.com/install.sh | sh
```
What you're trusting, stated plainly: that script, as root, initializes Docker Swarm and deploys the Dokploy services (app + its postgres + redis + Traefik). Want to look before you leap? `curl -sSL https://dokploy.com/install.sh | less` — the safety rules approve of reading.

Notes:
- The installer **publishes ports**, including the admin panel on `3000` — published by Docker, which means *around* ufw (card 01-01's warning). Exposed-by-default is expected at this point; card 04 locks it down **immediately after**. Do not pause between the two cards.
- Peer alternative: **Coolify** (<https://docs.coolify.io>) — same goal, same kind of installer, same card-04 obligation. The cards here stay valid; swap the commands.
- **Reboot race.** Traefik, and any compose app attached to `dokploy-network`, runs as a plain container on a swarm overlay network. After a reboot (including a provider-snapshot shutdown) or a Docker daemon restart (including a Docker package upgrade), it can lose the race to the network and exit with `network dokploy-network not found`. Docker never retries that error, even with `restart: always`. Every site goes down, and Cloudflare shows **521**.
  - Immediate fix: `sudo docker start dokploy-traefik`.
  - Durable fix: a boot job ordered after `docker.service` that waits until the network exists, then starts only the containers whose start error was "network … not found".

## ✅ Verify it worked
```bash
sudo docker service ls    # dokploy, dokploy-postgres, dokploy-redis, traefik — all replicated 1/1
sudo docker node ls       # this node: STATUS Ready, AVAILABILITY Active, MANAGER STATUS Leader
```
After every reboot or Docker upgrade, for the life of the server:
```bash
sudo docker ps -a --filter name=dokploy-traefik --format '{{.Names}} {{.Status}}'
# Expected: dokploy-traefik Up …   (NOT "Exited (128)": see the reboot race above)
```

## 🚪 Human check
Two gates, both mandatory: (1) the human was shown the exact installer line *before* it ran and explicitly approved it; (2) after the run, `docker service ls` shows every service at `1/1`.

## 🧯 If it went wrong
Partially reversible: a failed run usually leaves Swarm initialized and some services deployed. Back to post-card-02 state: `sudo docker service rm $(sudo docker service ls -q)` then `sudo docker swarm leave --force`. Worst case: the rule-7 snapshot. A service stuck below `1/1` → `sudo docker service logs <name>` — on very small VPSs the usual cause is plain RAM exhaustion (Level 2's swap card earns its keep here).
