# 05_keep-watch — see your server, get paged, keep a ritual

Levels 0–4 built a hardened, backed-up server that may even be serving an app — but it's *blind*. The uptime monitor (Level 2) only answers "is it up?". This level answers "how is it doing?": metrics with history, a dashboard you can open, alerts that find you before users do, logs in one searchable place, and a weekly health-check ritual whose reports land in the run folder.

## Inputs
**Working (this run):**
- `../runs/<server-name>/` — run folder; `level-0.md` … `level-2.md` must be fully checked (Level 3+ recommended; Level 4 not required)
**Reference (every run):**
- `../_shared/safety-rules.md`
- `../_shared/glossary.md`
- `../_templates/health-report.md` — the report format card 05 writes into the run folder

## Process
Cards `01` → `05`, in order — each assumes the previous exists:

1. **Metrics collection** — an agent recording CPU/RAM/disk/network with history. Low risk; the one ⚠️ is that metrics endpoints reveal system internals, so they bind to localhost or sit behind the firewall — never naked on the internet.
2. **A dashboard you can open** — graphs reachable from the human's own device *without* exposing a new public port (SSH tunnel, VPN, or authenticated reverse proxy). Attaching anything to the network re-triggers the rule-4 mindset: prove the dashboard is unreachable from the open internet before checking the box.
3. **Alerts that find you** — thresholds that push to a channel the human actually reads. Same gate as Level 2 card 04: a real test alert must arrive on a real device.
4. **App logs in one place** — recent logs from every service/container searchable in one UI. ⚠️ Logs can contain secrets: same exposure rules as the dashboard.
5. **The weekly health check** — a read-only report the agent generates on demand (weekly suggested), saved as `runs/<server-name>/health/YYYY-MM-DD_HHMM_health.md`. Pure reads — zero risk, no gate needed beyond the human reading the first one.

Cards 01–04 name example tools (Netdata, node_exporter + Grafana, Dozzle…) but the *arrangement* is the step — same philosophy as Level 2 card 04. Tools that bundle several cards (e.g. Netdata covers 01–03) are fine: verify each card's goal independently.

**Skip rule — verify, don't rebuild.** Each card's goal may already be met before you install anything: **provider consoles** (Hetzner, DigitalOcean…) ship host metrics, graphs, and metric alerts out of the box; **a PaaS layer like Dokploy** (Level 4) shows per-container stats and logs. If an existing arrangement verifiably meets a card's **goal**, don't install a duplicate — run the card's verify section against what exists, then check the box with a `covered-by: ___` note in the level file. Skipping is only valid per card, with proof; card 05 (the report ritual) is never skipped — it's what reads all the others.

## Outputs
- Updated checkboxes + notes in `../runs/<server-name>/level-5.md`, one new line in `summary.md` per step
- A `health/` folder in the run folder holding timestamped health reports (card 05)

## Human check
After card 05: the human has opened the dashboard on their own device, received one real threshold test-alert, found one of their own app's log lines in the log viewer — and read the first health report sitting in `runs/<server-name>/health/`.
