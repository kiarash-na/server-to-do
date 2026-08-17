# Run: <server-name>

State of one server through the pipeline. The agent updates this file after every verified step — this file is the pipeline's memory. Never commit it upstream (see `.gitignore`).

## Server facts — lock these in before card 00-01
The agent fills/verifies every row during the first scan. After that, these values are frozen facts — commands get adapted to *this* server, not to a generic one.

| Fact | Value | How to check |
|---|---|---|
| Hostname | | `hostname` |
| Public IP | | `curl -4 ifconfig.me` |
| Provider | | (human fills in) |
| OS & version | | `lsb_release -a` or `cat /etc/os-release` |
| Kernel / arch | | `uname -sr && uname -m` |
| Package manager | | apt / dnf / apk — decides command examples |
| Init system | | `ps -p 1 -o comm=` |
| Init access as | | root / other user |
| SSH port | 22 unless changed | `sshd -T \| grep ^port` |
| Sudo user (after level 0) | | fill in at card 00-02 |
| Snapshot taken | **no** — flip to yes BEFORE card 00-01 | safety rule 7 |
| Agent running this | | who/what + date started |

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
<Anything that deviated from the cards: OS adaptations, failures + recoveries, deliberate skips with reasons. Future-you will read exactly this section. If a deviation would help other servers too, open a PR or issue upstream.>
