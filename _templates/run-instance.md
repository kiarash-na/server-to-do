---
server: <hostname>
ip: <server-ip>
provider: <hetzner | digitalocean | aws | …>
os: <ubuntu 26.04 | debian 13 | rocky 10 | …>
started: <YYYY-MM-DD>
agent: <who/what is running this>
snapshot_taken: no        # flip to yes BEFORE card 00-01 (safety rule 7)
---

# Run: <server-name>

State of this server through the pipeline. The agent updates this file after every verified step — this file is the pipeline's memory. Never commit it upstream (see `.gitignore`).

## Level 0 — First Contact
- [ ] 01 SSH key access — notes:
- [ ] 02 Non-root sudo user — username:
- [ ] 03 Root + password login disabled — verified from new session:
- [ ] 04 First full update — reboot needed?:

## Level 1 — Lock the Doors
- [ ] 01 Firewall default-deny — allowed ports:
- [ ] 02 Brute-force protection — tool:
- [ ] 03 Automatic security updates — mechanism:
- [ ] 04 Know who is knocking — unique attacker IPs seen: ___ | successful logins: only me? yes/no

## Level 2 — Keep It Alive
- [ ] 01 Swap — size:
- [ ] 02 Time sync — synchronized: yes/no
- [ ] 03 Log rotation — docker capped: yes/no/na
- [ ] 04 External uptime monitor — tool: ___ | test alert received: yes/no
- [ ] 05 Backups — layers: ___ | restore drill date + result:

## Incidents & adaptations
<Anything that deviated from the cards: OS adaptations, failures + recoveries, deliberate skips with reasons. Future-you will read exactly this section.>
