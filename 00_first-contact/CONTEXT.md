# 00_first-contact — get in safely, replace root, first update

A fresh VPS hands you the master key (root, often password login). This level replaces that with a named human, a key-only door, and a fully patched system — without locking anyone out.

## Inputs
**Working (this run):**
- `../runs/<server-name>.md` — the run instance (provider, IP, OS, login details)
**Reference (every run):**
- `../_shared/safety-rules.md` — read first, obey always
- `../_shared/glossary.md` — when a card uses an unfamiliar word
- `../_shared/verify-and-recover.md` — when a verify step fails

## Process
Work the cards in order: `01` → `02` → `03` → `04`. Each card carries its own simple/techie/verify/human-check sections. Do not reorder: card 03 assumes cards 01–02 are done and verified.

## Outputs
- Updated checkboxes + notes in `../runs/<server-name>.md`, section Level 0

## Human check
After card 04: the human confirms they can open a **new** session as the non-root user and run one sudo command. Only then is Level 0 done.
