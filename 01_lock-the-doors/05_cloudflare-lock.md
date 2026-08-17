---
id: 01-05
title: Cloudflare lock — hide the origin IP
level: 1
risk: medium
reversible: yes
downtime: none (if verified before confirming)
time: ~20 min
os: any + provider panel + Cloudflare dashboard
---

# 05 — Cloudflare lock: hide your server's real IP

## 🐣 The simple version
Right now your server's IP is in the public phone book — anyone can look it up and knock *directly*, skipping every shield you put in front. The Cloudflare lock fixes that in two halves:

1. **Proxy:** your domain's DNS answers with *Cloudflare's* IPs, not yours. Visitors hit Cloudflare; Cloudflare forwards to your server.
2. **Lock:** your firewall learns to **slam the door on web traffic that didn't come from Cloudflare**. Bots scanning your raw IP find a dead end — they can't even reach your app to try.

We set the lock at your **provider's firewall panel first**, because it sits outside the server and can't be bypassed by anything happening inside it (Docker-published ports sneak past in-server firewalls — card 01 warned about this). The in-server firewall is the backup net.

⚠️ A lock on an address everyone already knows is theater. If this IP has ever been in public DNS, it's saved in history databases. Perfect case: lock down an IP that was never published.

## 💀 The techie version
**Goal:** origin accepts 80/443 *exclusively* from Cloudflare edge IPs; direct-IP web access is dead. SSH rules from cards 00–01 stay untouched.

1. **Proxy the DNS.** Site added to Cloudflare, A/AAAA records set to *proxied* (orange cloud). Confirm the site loads normally through the proxy before touching any firewall.
2. **Provider cloud firewall (primary enforcement).** Inbound rule: allow 80+443/tcp **only** from the ranges at <https://www.cloudflare.com/ips/> — both the IPv4 *and* IPv6 lists. (Hetzner/DigitalOcean/AWS SG/OVH all have this panel; if a provider truly lacks one, the in-VM layer below becomes the only net.)
3. **In-VM mirror (backstop).** ufw example: `sudo ufw allow from <range> to any port 80,443 proto tcp` for each range, then delete card 01's blanket `allow 80`/`allow 443`. Ranges change rarely but they *do* change — Cloudflare serves them as text/JSON, so a tiny scheduled refresh job keeps the lists honest.
4. **Optional, stronger:** Authenticated Origin Pulls — Cloudflare proves itself with a client certificate (mTLS), so even a correct-looking IP without the cert is refused.

**Official docs:**
- IP ranges (canonical, machine-readable): <https://www.cloudflare.com/ips/>
- Allowlisting Cloudflare IPs at your origin: <https://developers.cloudflare.com/fundamentals/concepts/cloudflare-ip-addresses/>
- Proxied vs DNS-only records: <https://developers.cloudflare.com/dns/proxy-status/>
- Authenticated Origin Pulls: <https://developers.cloudflare.com/ssl/origin-configuration/authenticated-origin-pull/>

## ✅ Verify it worked
```bash
dig +short yourdomain.com        # returns Cloudflare IPs (104.x / 172.6x.x …), NEVER your server IP
curl -I https://yourdomain.com   # 200 — through the shield works
curl -I --max-time 5 http://<server-ip>   # from another machine: times out — around the shield is dead
```
Plus: the provider firewall panel shows the Cloudflare-ranges rule. (Website broken? You locked out Cloudflare itself — stale or IPv4-only ranges. Re-sync from the docs above.)

## 🚪 Human check
The human opens the website in their own browser and it loads normally, and has seen the `dig` output showing Cloudflare IPs — not the origin's. (No public website on this box? Mark this step n/a in the level file.)

## 🧯 If it went wrong
Emergency undo, fastest first: in Cloudflare DNS flip the record to **DNS only** (grey cloud) and remove the origin-lock firewall rules — you're back to card 01's state in minutes. If the IP was already leaked before the lock (check `dig` history sites like crt.sh), the real fix is rotation: new IP from the provider, redo the lock, never let the new one touch public DNS. See `../../_shared/verify-and-recover.md`.
