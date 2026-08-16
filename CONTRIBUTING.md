# Contributing to server-to-do

The pipeline grows by **step cards** and **levels**. Both are templates — copy, fill, PR.

## Adding a step card
1. `cp _templates/step-card.md <level-folder>/NN_your-step.md` (next number in that folder)
2. Every section is mandatory: 🧒 simple, 🛠 techie, ✅ verify, 🚪 human check, 🧯 recovery
3. Goal first, then per-OS commands. Verify = exact command + literal expected output.
4. One human check — concrete, observable, singular.

## Adding a level
1. `cp _templates/level-CONTEXT.md NN_level-name/CONTEXT.md` (next root number)
2. Fill it, add step cards, then register the level in the tables of `CONTEXT.md`, `CLAUDE.md`, and the README diagram/table.

## The bar every PR must clear
- **Safety:** destructive or lockout-risk steps name the applicable rule from `_shared/safety-rules.md` and carry a tested rollback path
- **Dual voice:** a beginner understands 🧒; a senior respects 🛠
- **No vendor lock:** steps describe arrangements ("an external monitor"), not brands
- **Walk test:** from `CLAUDE.md`, a cold agent reaches your card in ≤2 reads and knows exactly what to do from the card alone
- Markdown only. No code, no binaries, no scripts.

Run files in `runs/` are gitignored on purpose — never commit real server state.
