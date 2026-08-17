---
id: 04-06
title: Deploy the first app
level: 4
risk: low
reversible: yes
downtime: none
time: ~20 min
os: any
---

# 06 — Deploy the first app: push → live

## 🐣 The simple version
The moment the whole pipeline exists for: connect your GitHub repo to Dokploy, press deploy, and your app answers on your domain — with the lock icon, on your own hardened server. From now on, "push to GitHub" and "it's live" are the same event. One rule that starts today and never ends: **passwords and API keys never go into the repo** — they go into Dokploy's environment-variable settings, which live on the server only.

## 💀 The techie version
**Goal:** a Dokploy project whose application builds from your GitHub repo, redeploys on push, and answers at `https://app.example.com`.

- In the Dokploy UI: create a **Project → Application**. Connect GitHub per the official docs (<https://docs.dokploy.com> → GitHub integration): the **GitHub App** flow (Dokploy registers an app on your account; you grant it just this repo) is the low-friction path and enables auto-deploy-on-push; a **deploy key** (a read-only SSH key added to the repo) is the minimal-access alternative.
- Set the domain to `app.example.com`, HTTPS on (Let's Encrypt — card 05 made this possible). Enable **auto-deploy on push** if you want the full push→live loop.
- Secrets go in Dokploy's **Environment** editor — never committed. Already committed one? Rotate it (the old value is burned), then scrub git history at leisure.
- App needs Postgres/MySQL/Redis? Dokploy runs them as sibling services — which makes card 07 (backups for exactly these) non-optional rather than decorative.

## ✅ Verify it worked
```bash
curl -sI https://app.example.com | head -1     # HTTP/2 200 (or your app's expected status)
sudo docker ps --format '{{.Names}} {{.Status}}'   # your app's container, Up
```

## 🚪 Human check
Change something *visible* in the repo (a heading, a version string), `git push`, watch Dokploy rebuild, see the change live on the domain. That push-to-live loop, witnessed once, is the gate — it's the thing you bought with all seven levels.

## 🧯 If it went wrong
Undeploying is one click; the server itself was never at risk. Build/deploy logs live in the Dokploy UI (and `sudo docker service logs <app>`). "Works on my laptop, fails here" is 90% missing env vars or a missing Dockerfile/build config — compare Dokploy's env editor against whatever your laptop has in `.env`.
