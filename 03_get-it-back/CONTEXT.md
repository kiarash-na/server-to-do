# 03_get-it-back — snapshots, off-box backups, proven restore

Levels 0–2 protect the server. This level protects you from *losing* the server entirely — disk death, fat-fingered command, bad deploy, provider incident. It builds backups in layers, easiest first, ending with the only thing that counts: a restore you actually watched succeed.

## Inputs
**Working (this run):**
- `../runs/<server-name>/` — run folder; `level-0.md`, `level-1.md`, `level-2.md` must be fully checked
**Reference (every run):**
- `../_shared/safety-rules.md`
- `../_shared/glossary.md`

## Process
Cards `01` → `05`, in order — each layer assumes the previous one exists:

1. **Provider snapshots** — the built-in undo button. Easiest, but it lives at the same provider as the server.
2. **Inventory** — write down what can't be rebuilt. You can't back up what you haven't named.
3. **Off-box file backups** — the inventory's files, copied somewhere the provider's bad day can't reach.
4. **Database dumps** — logical dumps feeding into that same off-box flow.
5. **Restore drill** — proof. The level is not done when backups are *configured*; it's done when one is *restored*.

The rule behind the layering is **3-2-1**: 3 copies of your data, on 2 different systems, 1 of them somewhere else entirely. Everything is stated OS- and provider-agnostic: the goal is fixed, the tool (snapshot panel, restic, rclone, rsync, pg_dump…) is your choice.

## Outputs
- Updated checkboxes + notes in `../runs/<server-name>/level-3.md`, one new line in `summary.md` per step

## Human check
After card 05: the human watched real data come back from a restored backup. Until that moment the pipeline stays unfinished — no matter how green the backup dashboard looks.
