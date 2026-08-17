---
id: 03-01
title: Provider snapshots — the built-in undo button
level: 3
risk: none
reversible: yes
downtime: none
time: ~10 min
os: any
---

# 01 — Provider snapshots: the built-in undo button

## 🐣 The simple version
Almost every VPS provider (Hetzner, DigitalOcean, AWS, OVH, Contabo…) has a **snapshot** feature: a photo of your server's entire disk, taken from their control panel. If anything ever goes catastrophically wrong, you click "restore" and the server travels back in time. This step does two things: take one snapshot **right now**, and turn on the provider's **automatic snapshot schedule** so a fresh one is always waiting. ⚠️ One honest limitation: snapshots live at the *same provider* as your server. If the provider itself has a very bad day, snapshots go down with it — that's why the next cards exist.

## 💀 The techie version
**Goal:** one manual snapshot taken today, plus automatic snapshots scheduled with a retention you chose (daily with 7 kept is a sane default).

This happens in the provider's panel/API, not on the server — nothing to install. Notes:
- Some providers charge for snapshot storage; check the price page before enabling the schedule.
- Snapshots are crash-consistent, not application-consistent — fine for whole-machine rollback, not a substitute for the database dumps in card 04.
- Provider "backups" (weekly auto-images) and "snapshots" (manual, on-demand) are often separate features; enable whichever scheduled option exists.

## ✅ Verify it worked
In the provider panel: the snapshot list shows your manual snapshot with today's date, and the automated schedule shows as **enabled** with a retention count. Write the provider's snapshot feature name into `facts.md` if it isn't there yet.

## 🚪 Human check
The human opens the provider panel with their own eyes and sees: one snapshot dated today + the schedule switched on. Panel screenshots from the agent don't count — the point is the human knows where this lives.

## 🧯 If it went wrong
Nothing on the server changed, so nothing to roll back. If the provider has no snapshot feature at all: note that in `summary.md` (incidents & adaptations) and lean harder on cards 03–04 — they become your only copies.
