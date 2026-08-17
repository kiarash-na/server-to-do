# Verify & recover — generic patterns when a card's own section isn't enough

Step cards carry their own "Verify" and "If it went wrong" sections — use those first. This file is the deeper reference for the three classic disasters.

---

## 1. "I locked myself out of SSH"

**Prevention (already in the pipeline):** cards 00-03 and 01-01 use the double-gate — a new session must work before the old one closes. If you're here, that gate was skipped. Never skip it again.

**Recovery ladder, easiest first:**
1. **Old session still open?** You win. Undo the last change from it, then redo it properly.
2. **Provider web console** — every VPS provider (Hetzner, DigitalOcean, AWS Lightsail…) has a browser-based console that behaves like a physical monitor+keyboard. Log in there, fix sshd/firewall directly.
3. **Provider rescue mode** — boot a temporary recovery OS, mount your disk, edit `/etc/ssh/sshd_config*` or disable the firewall, reboot normally.
4. **Snapshot restore** — roll back to the snapshot safety-rules rule 7 told you to take. You lose changes since the snapshot, not the server.
5. Rebuild. Painful, but with Level 3 backups done, survivable — which is exactly why the pipeline ends there.

## 2. "The firewall ate my app"

Symptom: SSH works, but your website/app is unreachable.

- `sudo ufw status numbered` — is the app's port allowed?
- **Docker gotcha:** a port published with `-p` bypasses ufw entirely; conversely, an app *not* published but expected through a proxy needs the proxy's port allowed, not its own.
- From outside: `nc -zv <ip> <port>` or an online port checker — open vs. filtered tells you whether it's the firewall at all.
- Fix by adjusting rules; `sudo ufw delete <rule-number>` removes a mistake.

## 3. "Disk full / weird crashes after deploy"

- `df -h` — any filesystem at 100%? That's the crash cause, nearly always.
- Biggest suspects: `sudo du -sh /var/lib/docker /var/log /tmp 2>/dev/null | sort -rh`
- Container logs unbounded → see Level 2 card 03. Old Docker images → `docker system prune -af` (**confirm with the human first; never `--volumes`**).
- Mystery process deaths with RAM near full → the OOM killer. `sudo journalctl -k | grep -i "killed process"` confirms. Level 2 card 01 (swap) is the prevention.

---

**The meta-rule:** every recovery path above ends at "snapshot" or "backup." Verification catches problems early; backups end them late. You need both, which is why they're both in the pipeline.
