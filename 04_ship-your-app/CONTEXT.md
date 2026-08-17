# 04_ship-your-app — from hardened box to running app

Levels 0–3 built a safe, recoverable server. This **optional track** puts it to work: code living on GitHub, pulled and run by a self-hosted PaaS layer — Dokploy (API-first, plain Docker + Traefik underneath; Coolify is the peer alternative) — served on your own domain with real TLS. When this level is done, `git push` *is* the deploy, and the safety net from Levels 1–3 has been re-stretched to cover everything the new layer added.

## Inputs
**Working (this run):**
- `../runs/<server-name>/` — run folder; `level-0.md` … `level-3.md` must be fully checked
**Reference (every run):**
- `../_shared/safety-rules.md`
- `../_shared/glossary.md`
- `../_shared/verify-and-recover.md`

## Process
Cards `01` → `07`, in order — each assumes the previous exists:

1. **GitHub account & repo** — the code gets a home the server can pull from. Nothing touches the server; zero risk.
2. **Container runtime** — Docker, the boxes everything runs in. ⚠️ The `docker` group is silently root-equivalent; this level deliberately stays out of it.
3. **Install Dokploy** — the only curl-as-root step in the whole pipeline: shown verbatim, explicitly approved (safety rule 3).
4. **Lock down the admin panel** — Docker-published ports *bypass ufw*, so the panel is exposed by default. This card is the fix, not an extra; attaching a cloud firewall touches network rules, so the rule-4 double-gate applies (prove a fresh SSH session before trusting anything).
5. **Domain, DNS & TLS** — a name instead of an IP, a lock icon instead of a warning.
6. **Deploy the first app** — connect GitHub, push, watch it go live.
7. **Close the loop** — re-verify Levels 1–3 against the new reality: cloud firewall as the enforcing layer, backup inventory + dumps extended to Dokploy's own data, uptime monitor pointed at the app.

## Outputs
- Updated checkboxes + notes in `../runs/<server-name>/level-4.md`, one new line in `summary.md` per step

## Human check
After card 07: the human pushes a trivial change to the repo, watches it redeploy, sees it live on the domain over HTTPS — and the run folder shows the firewall, backup inventory, and monitor updated to match the new layer.
