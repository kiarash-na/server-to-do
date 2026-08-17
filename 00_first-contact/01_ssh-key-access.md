---
id: 00-01
title: SSH key access
level: 0
risk: low
reversible: yes
downtime: none
time: ~10 min
os: any
---

# 01 — SSH key access

## 🐣 The simple version
Right now you probably log into your server with a password. Passwords can be guessed — and as you'll see in Level 1, thousands of bots try, every single day. An **SSH key** is a pair of files: one stays secret on your computer, one goes on the server like a named badge. No badge, no entry — guessing is mathematically hopeless.

Goal: you can log into the server with a key, from your own machine.

## 💀 The techie version
**Goal:** key-based auth working for the initial login user.

On your **local** machine (not the server):
```bash
# If you don't have a key yet:
ssh-keygen -t ed25519 -C "you@this-machine"

# Copy the public badge to the server:
ssh-copy-id user@<server-ip>        # macOS/Linux/WSL
# Windows PowerShell fallback: type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh user@<server-ip> "cat >> ~/.ssh/authorized_keys"
```
Then set up an alias so `ssh myserver` just works:
```
# ~/.ssh/config
Host myserver
    HostName <server-ip>
    User user
    IdentityFile ~/.ssh/id_ed25519
```

## ✅ Verify it worked
```bash
ssh myserver 'whoami && hostname'
```
Expected: your username and the server's hostname, **without** being asked for the server password.

## 🚪 Human check
Open a brand-new terminal, `ssh myserver` again. If you get in with no password prompt, tell the agent to proceed.

## 🧯 If it went wrong
`ssh -v myserver` shows which keys were offered. Most common cause: the server only allows keys for a different user, or `~/.ssh` permissions on the server are wrong (`700` for the folder, `600` for `authorized_keys`). Details: `../../_shared/verify-and-recover.md`.
