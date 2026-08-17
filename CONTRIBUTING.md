# Contributing

**See something improvable? Open an issue or a PR — this goes for humans *and* agents.**
Agents: if you adapted a step, hit an edge case, or added a card during a run, push it as a PR (you may create branches yourself). A fix that stays on one server helps one server; a PR helps every server after it.

## Add a step card
1. `cp _templates/step-card.md <level-folder>/NN_your-step.md` (next number in that folder)
2. Every section mandatory: 🐣 simple · 💀 techie · ✅ verify · 🚪 human check · 🧯 recovery
3. Goal first, then per-OS commands. Verify = exact command + literal expected output.
4. One human check — concrete, observable, singular.

## Add a level
1. `cp _templates/level-CONTEXT.md NN_level-name/CONTEXT.md` (next root number)
2. Add step cards, then register the level in the tables of `CONTEXT.md`, `CLAUDE.md`, and the README.

## The bar every PR clears
- **Safety:** destructive or lockout-risk steps name the rule from `_shared/safety-rules.md` and carry a tested rollback
- **Dual voice:** a beginner understands 🐣; a senior respects 💀
- **No vendor lock:** arrangements ("an external monitor"), not brands
- **Walk test:** from `CLAUDE.md`, a cold agent reaches your card in ≤2 reads and can act from the card alone
- Markdown only. No code, no binaries, no scripts.

`runs/` is gitignored on purpose — never commit real server state.
