# The Fleet — an operating philosophy for running agents

This is how I run a set of autonomous build-agents: a human steers, agents execute, and the whole thing is held together by **transparency and lightweight sign-off rather than heavy enforcement.** It's a working setup, shared because the ideas travel even if the implementation is mine. The naval framing is just a memorable handle for *who is allowed to decide what.*

> **Maturity tags:** ✅ live · 🟢 built, not yet flipped live · 🟡 concept / not yet built. I'd rather show the honest state than oversell a finished product.

## The one problem everything serves

A human's attention is the scarce resource. Every part of this system exists to **multiply the human's judgment without consuming it.** That single constraint is the tiebreaker for every design call: if a mechanism saves a decision but creates three new ways to demand the human's attention, it's a bad trade.

## The fleet (who decides what) ✅

- **Captain** — the human. Sets intent; holds a short list of reserved acts; gives the final word on anything irreversible. Doesn't write the plan's every line.
- **XO** — a planning + oversight agent. Holds the operating picture, drafts orders from the Captain's intent, and **independently certifies** work before it ships. The XO never grabs wrenches — the day it's doing the executor's job, it's doing that job worse and its own not at all.
- **Executor** (a.k.a. the "Master Chief") — the agent that owns the build end-to-end: plan → build → test → ship → clean up, in one motion. Runs its own crew of sub-agents when work fans out.
- **The channel** — where flagship and executor talk. Every escalation, decision, and sign-off lives there, visible to the Captain at all times.

The seams matter more than the boxes. Each role plugs in at a specific point; nobody reaches past their seam to do someone else's job.

## Altitude: the two-document model ✅

The core discipline. Two documents, deliberately at different altitudes:

- **`CAPTAINS-INTENT.md` — the WHY.** North Star, the 5 whys, end state, hard limits, who-decides-what. Short. It's what the agents fall back to when the plan breaks.
- **`SHIPS-COURSE.md` — the HOW.** The scoped plan *with the human's opinions baked in*, plus an explicit **Latitude** section: exactly where the executor has free rein vs. where deviating needs the human's word. Drafted by human + XO, then the executor **reviews and revises** it, then everyone signs. That signed doc is the launch plan.

**The failure this prevents:** cramming the plan into the "intent" doc. That collapses the altitude — the executor can only implement your spec and re-litigate your flaws, never *own* the design. The tell that you've drifted is a task list in the intent doc; tasks are HOW, they belong in the course. Intent steers; course executes.

(Templates: [`CAPTAINS-INTENT.template.md`](./CAPTAINS-INTENT.template.md) · [`SHIPS-COURSE.template.md`](./SHIPS-COURSE.template.md).)

## The channel — "message-in-a-bottle" ✅

A local-only, append-only correspondence log between the flagship and each vessel. One thread per workstream.

- **Local-only, no remote, ever.** The strongest leak-prevention is having nowhere to leak: correspondence is authored clean at the source, and there's no remote to push to as a backstop. *Nothing handed over = nothing to leak* beats every access wall.
- **Append-only, newest on top.** You reply by prepending; you never edit or delete a prior entry. The history *is* the audit trail.
- **Pinned where the human can see it.** The Captain watches every in-flight conversation at a glance and can interject anytime.
- **It's also an incubator.** A topic that isn't a real project yet gestates as a thread; when it's real, it graduates to its own repo. (Containment work did exactly this: thread → its own repo.)

## Commissioning ✅

The human stands up the repo and **seeds the paired orders before the crew sails.** Building the hull is a command act; sailing it is the crew's. This fixes a real bootstrap snag — asking an agent to create its own empty repo before it can do anything is a category error. Commission it ready-to-sail instead.

## Authority — tiered, and it fails closed ✅

Three stacked tiers, ranked by reversibility × blast radius:

1. **Hard floor** — what the agent's tooling literally cannot do (permission layer, deny-hooks, server-side git). Enforced in code, below the agent.
2. **Reserved acts** — a short list only the human ever does: merge to main, anything touching production, redefining intent, outward commitments, crossing a sensitivity line. Non-delegable.
3. **Proceed** — anything inside the agreed latitude. The *conservative* direction (halt / hold / revert / preserve) is always free, so the agent is never paralyzed.

The whole chain **fails closed**: when an agent can't reach the authority above it, it holds at the line — preserves, logs, surfaces — and never forces forward.

**An honest caveat worth stating out loud:** most of what an agent "must" do is *discipline*, not *mechanism*. Only a few things are truly hard-enforced in code; the rest hold because the agents are disciplined and the channel routes decisions to the human. Know which is which, and don't mistake a strong convention for a hard floor. Convert a convention into a real mechanism only when a real failure proves you need to.

## Containment & clearance — "brig" + "master-at-arms"

A two-part subsystem for when work fans out into parallel sub-agents:

- **brig (containment)** 🟢 — OS-level isolation so parallel agents touching the same tree have real boundaries, not an honor system. **Built; staged, waiting to flip live.**
- **master-at-arms (clearance)** 🟡 — an independent, content-bound release verdict: the gate that decides a contained agent's work is cleared to come out. Not yet built.

The two are halves of one idea — containment is only safe with a clearance verdict on the way out — so the subsystem goes active when both land. Underneath it: **certify, don't trust.** "Cleared" is an independent, logged, live-fire verdict, never the word of the agent that did the work.

## The commissioning standard — "blood-oath" ✅ (in spirit)

The rule that no vessel sails without paired, agreed, signed orders at the right altitude. The instructive part is *how it launched*: we designed a full mechanical enforcement gate for it — and then **deliberately deferred the gate** and adopted the practice "in spirit" instead, held by transparency and sign-off. Which brings us to the load-bearing idea. (Full standard: [blood-oath.md](./blood-oath.md).)

## The control philosophy (the part that actually matters)

- **Transparency is the control — not enforcement.** Everything in the channel, full candor, signed agreements. A gate guards a door; transparency means there are no closed doors to guard.
- **Defer hard gates. Smallest correct mechanism.** Build the lightest thing that's correct; add enforcement only once a real failure proves you need it. A buggy guard that wedges legitimate work costs more than the failure it was meant to prevent — and burns the one resource (the human's attention) the system exists to protect.
- **Full candor from message one.** Agents surface the uncomfortable truth *unprompted* — the objection to the premise, not just the execution. An agent that waits to be asked for its real opinion is a near-miss.
- **Certify, don't trust.** Independent, logged, live-fire verdicts. Not "looks good."
- **A signed body is frozen.** Any edit after signing gets re-affirmed in the open — never a silent "clerical" change.
- **Fail closed.** When in doubt, hold and surface; never force forward.

## How this actually got built (the honest part)

None of this is theory — it's scar tissue. The system was hardened by its own use, and most of the rules above exist because we broke them first:

- We cooked the altitude split *because* the first intent doc was secretly a plan, and the executor could only re-litigate it.
- We learned "defer the gate" *because* the planner over-built an elegant enforcement apparatus to guard a failure that had never happened — and the executor, speaking plainly, caught it.
- On the founding voyage of the commissioning standard, **three separate over-engineering instincts got caught before launch — every one by candor and transparency, not by a mechanism.** That's the whole argument for transparency-over-enforcement, proven in the building rather than asserted.

The lesson under the lessons: a system that makes it cheap to say *"this is wrong"* out loud, early, beats a system that tries to make wrong things mechanically impossible.

## Using any of this

Start small: the two documents + the channel + full candor. That alone gets you most of the value. Add commissioning, tiered authority, containment, and clearance as your scale actually demands them — not before. Copy the [templates](./), fill the `{{FILL: …}}` sentinels, sign, launch.

The load-bearing ideas, stripped of the metaphor: **separate why from how**, **make latitude explicit**, **prefer transparency to enforcement**, and **be honest about what's mechanism vs. discipline.** Adapt freely.
