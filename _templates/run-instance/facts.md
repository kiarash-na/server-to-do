# <server-name> — server facts

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
| Initial access as | | root / other user |
| SSH port | 22 unless changed | `sshd -T \| grep ^port` |
| Sudo user (after level 0) | | fill in at card 00-02 |
| Snapshot taken | **no** — flip to yes BEFORE card 00-01 | safety rule 7 |
| Agent running this | | who/what + date started |
