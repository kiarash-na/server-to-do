# Safety rules — read before touching any server

These rules bind the human *and* the agent. A step card can add gates; it can never remove one of these. When in doubt, the stricter reading wins.

## The seven invariants

1. **Scan before you change.**
   Every action begins read-only (`cat`, `status`, `df`, `ps`, `journalctl`). Understand the current state, then act. Never run a mutating command to "find out" something.

2. **One step at a time, in numbered order.**
   The folder numbers are the sequence. No batching steps, no skipping ahead because something looks easy, no "while we're in there" extras.

3. **Destructive commands require explicit human confirmation.**
   Anything that deletes, overwrites, drops, prunes, or mass-modifies gets shown to the human *verbatim* first, with a plain-language explanation of what it irreversibly does. Approval in one step never carries into the next.

4. **Lockout-danger steps use the double-gate.**
   When touching SSH config or firewall rules: make the change, then prove access from a **new, separate session** before closing or trusting anything. The original session stays open as the escape hatch until the new one is verified. (Cards 00-03 and 01-01 carry this risk; any future card that can sever access inherits it.)

5. **Every step ends with its own verification.**
   Run the card's "Verify it worked" section and record the real output in the run folder. "It probably worked" does not check a box.

6. **Failed verification = stop.**
   Report what happened, follow the card's "If it went wrong" section. Never improvise silent fixes, never retry-loops, never paper over an error to keep the pipeline moving. A stopped pipeline is a success; a lying one is a failure.

7. **Take the provider snapshot before you start.**
   If the VPS provider offers snapshots/backups, trigger one before card 00-01. It is the universal undo button, and it exists precisely for the day rules 1–6 weren't enough.

## For AI agents specifically
- You are an operator, not an owner: propose, explain, await the human's word at every Human check.
- Quote real command output in your reports — never paraphrase a failure into sounding like success.
- If a card doesn't cover the situation (different OS, missing tool), **read the tool's official docs first** (man page / official site), then adapt the *commands* toward the stated *goal* and write what you did into the run folder. Never invent new goals, never guess flags.
- If you improved a step, found a gap, or hit an edge case: tell the human, then open a PR or issue upstream (you may push branches yourself). Fixes belong to every future server, not just this one.
- If anything looks like an active intrusion or someone else's data, stop immediately and tell the human. Checklists end where incidents begin.
