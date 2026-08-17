---
id: 04-05
title: Domain, DNS & TLS
level: 4
risk: none
reversible: yes
downtime: none
time: ~20 min + DNS wait
os: n/a (registrar + DNS)
---

# 05 — Domain, DNS & TLS: a name and a lock icon

## 🐣 The simple version
`http://167.235.x.x` is not a website; `https://app.yourname.com` is. Three ingredients: a **domain** (a few dollars a year at any registrar — Porkbun, Namecheap, Cloudflare…), a **DNS record** pointing the name at your server's IP (the internet's phonebook entry), and a **certificate** so browsers show the lock instead of a scary warning. Dokploy fetches and renews the certificate *for free, automatically* — but only once the first two exist, so this card comes before deploying anything.

## 💀 The techie version
**Goal:** an `A` record (`app.example.com` → server IPv4) resolving publicly, ports 80/443 open, and Traefik (installed by Dokploy) able to obtain Let's Encrypt certificates.

- At your registrar's DNS page: `A  app  <server-ipv4>`, TTL 300 while setting up. If the server has IPv6 (Hetzner hands out a /64 by default) and you add an `AAAA` record, remember Level 1 must cover IPv6 too — ufw does when `IPV6=yes` in `/etc/default/ufw`, and cloud-firewall rules are **per address family**: a rule allowing only IPv4 leaves the IPv6 door standing open.
- Open the web ports on the host firewall as well (covers anything not inside Docker):
  ```bash
  sudo ufw allow 80/tcp && sudo ufw allow 443/tcp
  ```
- DNS propagates in minutes, occasionally hours. **Confirm the record resolves before asking Dokploy for a certificate** — Let's Encrypt rate-limits failures, and "it didn't work yet" is nearly always "DNS hasn't spread yet."

## ✅ Verify it worked
```bash
dig +short app.example.com      # returns your server's IP
nc -zv <server-ip> 80           # succeeds
sudo ufw status | grep -E '80|443'   # both allowed
```

## 🚪 Human check
`dig` from your own laptop shows the server IP, and if you gave the Dokploy panel its own domain in card 04, `https://admin.example.com` loads with a lock icon. Otherwise: DNS + ports confirmed, ready for the deploy.

## 🧯 If it went wrong
Nothing here is destructive — it's registrar settings and two firewall allowances. Certificate failures later are 90% "DNS not propagated yet" or "80/443 blocked somewhere"; Traefik's logs say which: `sudo docker service ls` for its name, then `sudo docker service logs <traefik-service>`.
