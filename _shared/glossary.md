# Glossary — every concept, explained like you're new

Step cards link here instead of re-explaining. Alphabetical, plain words, no prerequisites.

---

**Backup / snapshot** — A copy of your stuff kept somewhere safe. A *backup* usually means your data (databases, files). A *snapshot* is a photo of the entire server disk at one moment. You want both.

**Bot / botnet** — A robot program (or an army of infected computers) that automatically tries passwords on every server on the internet. Not personal — you're attacked because you exist, not because of who you are.

**Brute force** — Guessing passwords by trying millions of them. Defeated by (a) not allowing password logins at all and (b) banning IPs that fail repeatedly.

**Container / Docker** — A way to package an app with everything it needs into one box that runs the same anywhere. A server often runs several containers side by side.

**Daemon / service** — A program that runs permanently in the background (web server, database…). "Restart the service" = turn that background program off and on.

**Firewall** — The bouncer deciding which network traffic may reach your server. "Default deny" = everyone is refused unless explicitly invited.

**Fail2ban** — A popular bouncer-assistant: watches login logs, and if an IP fails too many times, blocks it at the firewall for a while.

**NTP / time sync** — The protocol servers use to keep their clocks accurate by asking official time servers. Prevents a whole category of spooky bugs.

**OOM / OOM killer** — "Out Of Memory." When RAM is full, Linux's OOM killer instantly terminates a program to keep the machine alive. It doesn't ask which program you'd miss.

**Port** — A numbered door on the server. Each service listens on one: SSH = 22, websites = 80/443. Closed doors can't be attacked through.

**Restore drill** — Practicing bringing your data back from a backup. The only way to know backups work.

**Root** — The all-powerful administrator account. Convenient, dangerous, and the #1 username bots try. Daily work should happen as a normal user instead.

**SSH** — The encrypted remote-control protocol you use to type commands on your server from your laptop.

**SSH key** — A matched pair of files replacing your password: a *private* key (stays secret on your laptop) and a *public* key (lives on the server as your named badge). Practically unguessable.

**sudo** — Prefix meaning "do this one command with admin powers." Every use is logged; much safer than living as root.

**Swap** — Emergency overflow memory living on disk. Slow, but it stops the OOM killer from randomly murdering your apps during memory spikes.

**Uptime monitor** — An external service that checks your server every few seconds and alerts you when it stops answering. "External" matters: a monitor on the same server dies with it.

**VPS** — Virtual Private Server: your rented slice of a big machine in a datacenter. Yours to configure, yours to break, yours to protect.
