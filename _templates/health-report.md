---
server: <server-name>
generated: <YYYY-MM-DD HH:MM timezone>
period: since <date of previous report, or "first report">
---

# Health report — <server-name> — <YYYY-MM-DD HH:MM>

Read-only snapshot, generated per `05_keep-watch/05_weekly-health-check.md`. File as `runs/<server-name>/health/YYYY-MM-DD_HHMM_health.md`. Fill every section with real numbers or `n/a — <reason>`; delete no section.

## Vitals
| Metric | Now | Previous | Trend |
|---|---|---|---|
| Uptime / load | | | |
| RAM used | | | |
| Swap used | | | |
| Disk / (and any other mount >70%) | | | |

## Security quick-look
- Failed SSH logins (7d): ___
- Fail2ban currently banned: ___
- Pending security updates: ___
- Anything odd in logs: ___

## Backup freshness
| Backup | Last taken | Age | Within policy? |
|---|---|---|---|
| Provider snapshot | | | |
| Off-box files | | | |
| Database dump | | | |

## Services
- Containers/services running vs expected: ___
- Restarts since last report: ___
- Uptime monitor: green / incidents: ___

## Alerts fired since last report
<each: time · what · resolved? — or "none">

## Notes & trend
<What changed since the last report. What's drifting toward a threshold. What to do before the next report — or "all quiet".>
