---
id: 00-02
title: Create a non-root sudo user
level: 0
risk: low
reversible: yes
downtime: none
time: ~5 min
os: any (commands shown for Debian/Ubuntu; RHEL/Fedora use wheel)
---

# 02 — Create a non-root sudo user

## 🐣 The simple version
`root` is the server's god-mode account — it can delete everything with one typo, and every bot on the internet tries to log in as "root" first. So we make a normal person account for daily work, and give it a **sudo** badge: it can borrow god-mode for one command at a time, leaving a log entry each time.

Goal: a named user exists, can sudo, and has your SSH key.

## 💀 The techie version
**Goal:** non-root user in the sudo/wheel group, with your public key authorized.

Run as root on the server:
```bash
# Debian/Ubuntu:
adduser deploy                 # or your name; sets password + home dir
usermod -aG sudo deploy

# RHEL/Fedora/Rocky:
adduser deploy && passwd deploy
usermod -aG wheel deploy

# Authorize YOUR key for the new user (copy from root's, same badge):
mkdir -p /home/deploy/.ssh
cp ~/.ssh/authorized_keys /home/deploy/.ssh/
chown -R deploy:deploy /home/deploy/.ssh
chmod 700 /home/deploy/.ssh && chmod 600 /home/deploy/.ssh/authorized_keys
```
Update your local `~/.ssh/config` to use the new username.

> 🤖 **Agent-driven run?** If you want the agent to execute privileged cards itself, the human may grant passwordless sudo: `echo '<user> ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/90-<user>-automation && sudo chmod 440 /etc/sudoers.d/90-<user>-automation` (verify with `sudo visudo -cf`). Explicit human choice, revocable by deleting the file. Never weaken SSH auth instead — key-only stays the rule.

## ✅ Verify it worked
From a **new** local terminal (keep the root session open!):
```bash
ssh deploy@<server-ip> 'whoami && sudo -n true 2>/dev/null && echo "sudo OK (passwordless)" || sudo true && echo "sudo OK"'
```
Expected: `deploy`, then a sudo success line.

## 🚪 Human check
Confirm you can get in as the new user and sudo works. The old root session stays open until Level 0 card 03 is verified — that is your escape hatch.

## 🧯 If it went wrong
You're still in as root — nothing is lost. Re-check the group (`groups deploy`) and the `.ssh` permissions; 90% of failures are wrong ownership on `authorized_keys`.
