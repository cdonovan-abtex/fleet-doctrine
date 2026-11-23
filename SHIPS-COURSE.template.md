---
standard: v2
type: SHIPS-COURSE
project: "{{FILL: project / workstream name}}"
date: "{{FILL: ISO date}}"
signoffs: []   # one line per signer at agreement: role · name · ISO-date
---

# SHIPS-COURSE — {{FILL: project name}}

> The HOW, carrying command's opinions + explicit latitude. Human+XO draft → the executor reviews/revises → all agree = the launch plan. Conforms to the repo's STRATEGY.md.
> (Non-stub = every {{FILL: …}} sentinel replaced and each section carries real substance.)

## End State: this voyage
{{FILL: what is observably true when this voyage is done, the success condition. A line or two, never a task list (tasks are the course below).}}

## Conformance to Strategy
{{FILL: the STRATEGY.md limits and fork test this course certifies against (the XO certifies here)}}

## The Course — scoped plan + our opinions
{{FILL: the plan, WITH command's strong opinions on approach / architecture / tradeoffs — not a blank for the executor to fill}}

## Latitude — free vs fixed
{{FILL: the heart of the doc. FIXED = deviation needs the human's word. FREE = the executor's own judgment. State both explicitly.}}

## Milestones / Waypoints
{{FILL: the ordered checkpoints from launch to done}}

## Risks & Rough Waters — and escalation
{{FILL: what could go wrong, and where the line is to signal the human rather than improvise past a fixed point}}

## Definition of Done — Test Protocol
{{FILL: the bar, upfront. "Solid" = passed this protocol, not anyone's word.}}

## Dispatch (mechanical)
{{FILL at dispatch, not before: repo/branch · paths · test commands}}

## Agreement — the launch plan
- [ ] XO signoff (conformance certified)
- [ ] Executor signoff (reviewed + revised; buildable as written)
- [ ] Human's blessing (launch)

Signoffs recorded in the frontmatter `signoffs:` block as plain `role · name · ISO-date` lines. Tamper-evidence is the append-only channel + git history — not a content-hash. If anything changes after signing, surface it and re-sign in the open; never silently edit a signed body.
