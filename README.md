<div align="center">

# ✅ server-to-do

**The fresh-VPS to-do list you hand to an AI agent.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Structure: ICM](https://img.shields.io/badge/structure-ICM-blueviolet)](https://github.com/RinDig/icm-architect)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
![No code, only markdown](https://img.shields.io/badge/code-none%2C%20only%20markdown-blue)

*Spin up a server. Clone this. Tell your agent: "run it."*
*One verified step at a time — with a human gate on everything that can hurt.*

</div>

---

Every new VPS starts the same way: a root password, a thousand bots already knocking, and a checklist you're *sure* you'll remember. You won't. **server-to-do** is that checklist, written down: OS-agnostic, goal-first, safety-obsessed, structured with [ICM](https://github.com/RinDig/icm-architect) so any agent can walk it while you approve each step. Every card explains itself twice — once 🐣 simple, once 💀 technical.

```mermaid
flowchart LR
    A[🖥️ fresh VPS] --> L0
    subgraph L0[00 · FIRST CONTACT]
        A1[SSH keys] --> A2[sudo user] --> A3[root & passwords off] --> A4[full update]
    end
    L0 --> L1
    subgraph L1[01 · LOCK THE DOORS]
        B1[firewall: default deny] --> B2[brute-force bouncer] --> B3[auto security updates] --> B4[read your attack logs] --> B5[origin hidden behind Cloudflare]
    end
    L1 --> L2
    subgraph L2[02 · KEEP IT ALIVE]
        C1[swap safety net] --> C2[time sync] --> C3[log rotation] --> C4[external uptime monitor]
    end
    L2 --> L3
    subgraph L3[03 · GET IT BACK]
        D1[provider snapshots] --> D2[know your data] --> D3[off-box file backups] --> D4[database dumps] --> D5[restore drill]
    end
    L3 --> L4
    subgraph L4[04 · SHIP YOUR APP · optional]
        E1[GitHub account & repo] --> E2[Docker] --> E3[Dokploy] --> E4[panel locked down] --> E5[domain + TLS] --> E6[push → live] --> E7[close the loop]
    end
    L4 --> L5
    subgraph L5[05 · KEEP WATCH]
        F1[metrics collected] --> F2[dashboard you can open] --> F3[alerts that find you] --> F4[logs in one place] --> F5[weekly health report]
    end
    L5 --> D[🚀 your app, live on your own hardened server]
```

| Level | Mission | Time |
|---|---|---|
| **00 · First Contact** | Keys in, root out, system updated | ~30 min |
| **01 · Lock the Doors** | Firewall, bouncer, auto-updates, meet your attackers, Cloudflare lock | ~50 min |
| **02 · Keep It Alive** | Swap, time, logs, uptime alerts | ~45 min |
| **03 · Get It Back** | Snapshots, off-box backups, a restore you've watched work | ~60 min |
| **04 · Ship Your App** *(optional)* | GitHub → Dokploy → push-to-live on your domain with TLS | ~2 h |
| **05 · Keep Watch** | Metrics, dashboard, alerts that find you, logs in one place, weekly health reports | ~1.5 h |
| **06+ · Going Pro** | *your ideas — see [Contributing](#contributing)* | — |

## 🚀 For humans

```bash
git clone https://github.com/kiarash-na/server-to-do.git
cd server-to-do
cp -r _templates/run-instance runs/my-server   # fill runs/my-server/facts.md
```

Take your provider's **snapshot** (the undo button), then work the levels top to bottom — or let an agent drive 👇.

## 🤖 For AI agents

> *"Follow `CLAUDE.md`, obey `_shared/safety-rules.md`, run the pipeline on `runs/my-server/`. Stop at every human check."*

That's the whole prompt. The agent works the numbered cards, verifies each with read-only commands, and stops for your yes before anything risky. Progress lives in the run folder — any agent can resume from the level checkboxes, and `summary.md` shows what happened at a glance.

## 🛡 The safety contract

[`_shared/safety-rules.md`](_shared/safety-rules.md) — seven rules binding human and agent alike: scan before change · one step at a time · destructive = explicit yes, shown verbatim · double-gate on lockouts · verify or it didn't happen · failed verify = stop · snapshot first.

## Contributing

Something unclear, outdated, or missing? **Open an issue or a PR — humans and agents both welcome.** Agents: if you adapted or improved a step during a run, push it as a PR so the next server gets it too. New steps: copy `_templates/step-card.md`. New levels: copy `_templates/level-CONTEXT.md`. Details in [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE) — take it, fork it, run it on everything you own.
