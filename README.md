# fleet-templates

Two paired operating documents for steering agent work — the **why** and the **how**, kept at different altitudes on purpose.

These are the templates behind a small "fleet" model for running autonomous build-agents: a human sets intent and holds a few reserved decisions, a planning agent translates that into a reviewable plan, and an executing agent builds it. The whole thing is held together by transparency and lightweight sign-off, not heavy enforcement.

## The two documents

**`CAPTAINS-INTENT.md` — the WHY.** North Star, the 5 whys, end state, hard limits, who-decides-what. Short by design. It's what the agents fall back to when the plan breaks. (Closest analog: an OKRs / north-star file — but strictly *why*, never *how*.)

**`SHIPS-COURSE.md` — the HOW.** The scoped plan *with the human's opinions baked in*, plus an explicit **Latitude** section: exactly where the executor has free rein vs. where deviating needs the human's word. Drafted by human + planning agent, then the executor **reviews and revises** it, then everyone signs — that signed doc is the launch plan.

### Why split them

The failure mode that motivated this: cramming the plan into the "intent" doc. That collapses the altitude — the executor can only implement your spec and re-litigate your flaws, never *own* the design. Separating why-from-how fixes it. **Intent steers; course executes.** A task list in the intent doc is the tell that you've drifted — tasks are HOW, they belong in the course.

## The roles (the "fleet")

- **Captain** — the human. Sets intent; holds a short list of reserved acts (e.g. merging to main, anything touching production, redefining intent, outward commitments). Non-delegable.
- **XO** — a planning/oversight agent. Holds the operating picture, drafts orders, **independently certifies** work before it ships. Never grabs wrenches.
- **Executor** — the agent (or agent + crew) that owns the build end-to-end: plan → build → test → ship.
- **The channel** — an append-only log where every escalation, decision, and sign-off lives, visible to the human at all times.

## How to use

1. Copy both templates into the project.
2. Fill every `{{FILL: …}}` sentinel — that's also the dumb mechanical check for "filled vs. stub": zero sentinels left = ready to sign.
3. Human + XO draft, executor reviews and revises the course, everyone signs the `signoffs:` block (plain `role · name · ISO-date`).
4. Human's blessing on the course = launch.

## Principles it encodes

- **Latitude is explicit.** Free vs. fixed is stated, not assumed — that line is where autonomy actually lives.
- **Transparency is the control.** Everything in the channel; full candor from message one; agents surface the uncomfortable truth unprompted. This is what holds the standard in place of a hard enforcement gate.
- **Defer hard gates.** Build the lightest mechanism that's correct; add enforcement only once a real failure proves you need it. A buggy guard that wedges legitimate work costs more than the failure it prevents.
- **A signed body is frozen.** Any edit after signing gets re-affirmed in the open — never a silent "clerical" change.

---

Shared from a working setup; adapt freely. The naval framing is just a memorable handle for "who's allowed to decide what" — the load-bearing ideas are the **why/how split**, the **explicit latitude**, and **transparency over enforcement**.
