---
id: 04-01
title: GitHub account & repo
level: 4
risk: none
reversible: yes
downtime: none
time: ~15 min
os: n/a (external service)
---

# 01 — GitHub account & repo: a home for the code

## 🐣 The simple version
Your app is code sitting on your laptop. The server needs to fetch that code from somewhere you can *both* reach — that's GitHub: a free online home for code, and the "connect repo → auto-deploy" button every deploy tool speaks. If you already have a GitHub account and a repo with your app, this card is a 2-minute check; if not, it's account creation plus your first push. Either way: turn on **two-factor authentication** — your GitHub account can now deploy to your server, which makes it part of your server's security.

## 💀 The techie version
**Goal:** a GitHub account with 2FA enabled, holding the app's repository (private is fine), with the latest code pushed.

- Create the account at <https://github.com>, then **Settings → Password and authentication → Enable 2FA** (authenticator app, not SMS).
- Create the repository (private), then push from your **dev machine** (not the server):
```bash
git init && git add -A && git commit -m "first commit"
git remote add origin git@github.com:<you>/<app>.git   # or the https URL
git push -u origin main
```
- Pushing needs auth from your dev machine: a GitHub SSH key (docs.github.com → "Connecting to GitHub with SSH") — same concept as card 00-01, different host. Nothing GitHub-related gets installed on the server in this card; card 06 wires the server's access.

## ✅ Verify it worked
```bash
# On your dev machine:
git ls-remote origin        # prints your branch names + commit hashes
```

## 🚪 Human check
You log into github.com, see your repo with your actual code in it, and 2FA shows as enabled. Existing-account humans: same check, 2 minutes.

## 🧯 If it went wrong
Nothing on the server was touched — the failure modes are all local git auth. `git push` rejected → the SSH key isn't on your GitHub account yet; GitHub's SSH docs walk it. Committed a secret by accident? Rotate it now, scrub later — card 06 keeps secrets out of the repo from here on.
