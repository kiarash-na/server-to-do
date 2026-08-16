# runs/ — one file per server

Copy `../_templates/run-instance.md` here, named after your server:

```bash
cp ../_templates/run-instance.md ./my-server.md
```

Fill the frontmatter, take the provider snapshot, then hand the repo to your agent:
*"Run server-to-do on `runs/my-server.md` — follow CLAUDE.md."*

**Why this folder is gitignored:** run files record real IPs, usernames, and your infra's state. That's your business, not the public repo's. Keep them locally, or in a private fork.
