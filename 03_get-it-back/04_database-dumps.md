---
id: 03-04
title: Database dumps — logical backups that survive restores
level: 3
risk: low (reads the live database; brief extra load)
reversible: yes
downtime: none
time: ~20 min
os: any
---

# 04 — Database dumps: logical backups that survive restores

## 🐣 The simple version
Copying a database's raw files while it runs is like photocopying a document while someone is writing on it — you can get a half-written page that won't open later. Instead, every database has a built-in "export everything as a clean text snapshot" command (`pg_dump`, `mysqldump`, sqlite's `.backup`). This step schedules that export, so your database gets a clean, portable copy every day — one that can even be restored into a *different version* of the database. No database on the server? This card is an honest `n/a` — mark it and move on.

## 💀 The techie version
**Goal:** every database from the card-02 inventory dumped on a schedule, with the dump files landing where card 03's off-box job picks them up.

- **Dump, don't copy:** `pg_dump` / `mysqldump` / `mariadb-dump` / `sqlite3 … .backup` — per the DB's official docs. Dumps are logical: version-tolerant and restore-safe in a way raw file copies are not.
- **Databases in containers:** run the dump *through* the container (`docker exec <db> pg_dump -U …`) rather than reaching into volume files.
- **Timing:** schedule the dump to finish *before* card 03's off-box job starts, so each night's off-box copy includes a fresh dump. Timestamp filenames; keep several.
- **Load:** dumps read the whole DB — on big databases schedule for quiet hours.

## ✅ Verify it worked
1. Run the dump once by hand: the output file exists, is **non-empty**, and starts with sane content (`head` shows SQL statements / a valid archive header — not an error message).
2. The schedule entry exists, and the dump directory is on card 03's backup path list.

## 🚪 Human check
The human sees one real dump file with today's date and a believable size. (Opening it is card 05's drill — here we just prove it exists on schedule.)

## 🧯 If it went wrong
Dumps are read-only against the live database — worst case is wasted disk, cleaned by deleting the dump files. Credentials in dump scripts belong in a root-readable file, not in the crontab line. If the dump errors, the DB's official docs for the dump tool are the first stop.
