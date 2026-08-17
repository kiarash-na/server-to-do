---
id: 04-04
title: Lock down the admin panel
level: 4
risk: medium
reversible: yes
downtime: none
time: ~15 min
os: any
---

# 04 — Lock down the admin panel

## 🐣 The simple version
Dokploy's admin panel can create, delete, and reconfigure *everything* on the server — and right now it's sitting on port 3000, open to the whole internet, guarded only by the password you're about to invent. And here's the trap from card 01-01, now live: **Docker publishes ports around the firewall**, so "ufw is on" does *not* protect the panel. This card makes the panel reachable only by you, then puts a real password on it. Do it immediately after installing — an unguarded panel on a fresh IP gets found by scanners within hours.

## 💀 The techie version
**Goal:** the panel is not anonymously reachable from the public internet, and its account has a strong unique password.

Pick one:
- **Provider cloud firewall (recommended).** In the Hetzner/etc. panel: create a firewall allowing inbound **only 22, 80, 443** (IPv4 *and* IPv6) and attach it to the server. This layer sits *in front of* Docker's NAT tricks, so port 3000 dies for strangers no matter what Docker does. Reach the panel through an SSH tunnel when you need it:
  ```bash
  ssh -L 3000:localhost:3000 myserver    # then open http://localhost:3000
  ```
  ⚠️ Attaching the firewall changes network rules — safety rule 4's double-gate applies: after attaching, prove a **brand-new SSH session** works before trusting anything.
- **Panel on its own domain** (e.g. `admin.example.com` with TLS via card 05's machinery). Acceptable only with a strong unique password + any 2FA Dokploy offers — weaker than the firewall option, because the login page stays internet-facing and becomes a standing brute-force target.

Then, through whichever door you chose: open the panel, create the admin account, password from your password manager — unique, not memorable, not reused.

## ✅ Verify it worked
```bash
# From another machine (not the server):
nc -zv <server-ip> 3000     # cloud-firewall option: must FAIL (timeout/refused)
# domain option: https://admin.example.com must demand login before showing anything
```

## 🚪 Human check
With the SSH tunnel **closed**, `http://<server-ip>:3000` times out on your laptop. Then you open the tunnel (or the panel domain), log in with the new password, and see the dashboard with your own eyes.

## 🧯 If it went wrong
The panel is just a port — SSH is unaffected as long as the cloud firewall kept rule 22, which is why the double-gate exists. Locked out of the *panel*: detach/edit the cloud firewall in the provider's web console (that action lives in *their* panel, not on the server). Locked out of *SSH*: `../../_shared/verify-and-recover.md`, ladder step 2.
