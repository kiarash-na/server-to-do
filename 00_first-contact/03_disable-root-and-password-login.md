---
id: 00-03
title: Disable root login and password auth
level: 0
risk: high
reversible: yes (while a session is open)
downtime: none (new connections only)
time: ~5 min
os: any
---

# 03 — Disable root login and password auth

## 🐣 The simple version
Now that your personal account works with a key, we weld two doors shut: "log in as root" and "log in with a password". After this, the only way in is your key, as your named user. Every password-guessing bot on Earth becomes harmless background noise.

⚠️ This is the one step that can lock you out if done carelessly. That's why we test the new door **before** closing the old one.

## 💀 The techie version
**Goal:** `PermitRootLogin no`, `PasswordAuthentication no`, `PubkeyAuthentication yes`.

```bash
# Pre-flight: confirm card 02 verify passed from a SECOND session. Do not proceed on faith.

# Debian/Ubuntu (drop-in wins over main config):
sudo tee /etc/ssh/sshd_config.d/10-hardening.conf <<'EOF'
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
EOF

# RHEL/Fedora: same drop-in path works on modern OpenSSH; otherwise edit /etc/ssh/sshd_config directly.

sudo sshd -t                 # syntax check — MUST print nothing
sudo systemctl reload ssh    # or: sshd on some distros
```

## ✅ Verify it worked
Keep your current session open. From a new terminal:
```bash
ssh myserver 'echo new session works'                       # must succeed
ssh root@<server-ip> 'echo this should fail'                # must FAIL
sshd -T | grep -iE 'permitrootlogin|passwordauthentication' # on server: no / no
```
Expected: new user session works, root login refused, effective config shows `no`/`no`.

## 🚪 Human check
You personally opened a fresh session as your user **after** the reload, and root was refused. Say it out loud, then let the agent check the box.

## 🧯 If it went wrong
Your still-open original session is the lifeline — `sudo rm /etc/ssh/sshd_config.d/10-hardening.conf && sudo systemctl reload ssh`. If all sessions are lost: provider web console / recovery mode, see `../../_shared/verify-and-recover.md`.
