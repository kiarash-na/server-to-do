---
id: 03-02
title: Inventory — know what can't be rebuilt
level: 3
risk: none
reversible: yes
downtime: none
time: ~15 min
os: any
---

# 02 — Inventory: know what can't be rebuilt

## 🐣 The simple version
You can't back up what you haven't named. Imagine the server vanishes right now and you get a fresh empty one: what would you *actually* lose? Some things are fine — the operating system, the apps, anything you can reinstall. Other things are gone forever — your database, the photos users uploaded, that config file you tuned for three evenings. This step is just making that list, on paper (well, in the run folder). Every backup you set up after this exists to protect something on the list.

## 💀 The techie version
**Goal:** a written inventory of irreplaceable data, recorded in the run folder (`level-3.md` notes or `summary.md`).

Walk the server and sort what you find into *rebuildable* vs *irreplaceable*. The usual irreplaceable suspects:

1. **Databases** — find them: running DB containers/services (`docker ps`, `systemctl list-units | grep -iE 'mysql|maria|postgres|mongo|redis'`)
2. **Volumes & uploaded files** — app data directories, bind mounts, anything users or the app *created* (`docker volume ls`, app data dirs)
3. **Config & secrets** — env files, compose files, Caddyfile/nginx confs, API tokens (this repo's run folder already captures some)
4. **Off-box state** — DNS records, provider firewall rules: not on the server at all, so note them here

Rule of thumb: if `rm -rf` on the server would lose it, it's on the list. List each item with its path/source — cards 03–04 will read this inventory as their to-do list.

## ✅ Verify it worked
Every irreplaceable item is written down with a concrete path or source — "the database" is not an entry, "postgres container `db`, volume `pgdata`" is.

## 🚪 Human check
The human reads the inventory and answers: "if the server died tonight, is anything I care about *missing* from this list?" One honest no = add it before moving on.

## 🧯 If it went wrong
A list can't break anything. A list you *forgot something on* is the failure mode — re-run this card whenever you add a new app or database to the server.
