# blood-oath — the commissioning standard

The rule: **no vessel sails without paired, agreed, signed orders at the right altitude.** Every unit of work launches under the repo's STRATEGY.md (the why) with a SHIPS-COURSE (the how), reviewed by the executor and signed by everyone, *before* any build.

## What it's made of
1. **The two templates** — the single source of the standard, versioned with a `standard: vN` line. ([`STRATEGY.template.md`](./STRATEGY.template.md) · [`SHIPS-COURSE.template.md`](./SHIPS-COURSE.template.md))
2. **Commissioning** — the human stands up the repo and seeds STRATEGY.md and the first course before the crew sails. (Building the hull is a command act; sailing it is the crew's.)
3. **A gate** *(deferred — see below)* — a check that the orders exist, are to standard, and are signed, before build.

## The decision worth copying: launch it "in spirit"
We designed the full mechanical gate — and then deliberately **deferred it.** blood-oath runs *in spirit*: the practice + transparency + sign-off, with the hard enforcement gate held back as **conditional** — built only if a real failure (a half-formed launch that nothing caught) ever proves we need it, and then the lightest version, always human-overridable.

Why: a content-checking gate is machinery, and machinery has its own failure mode — a buggy guard that wedges legitimate work costs more than the failure it prevents, and it burns the one resource (the human's attention) the whole system exists to protect. Until there's a real near-miss, **transparency is the cheaper, safer control.**

## The signoff convention
Plain lines in the document's frontmatter:

```yaml
signoffs:
  - "XO · <name> · <ISO-date>"
  - "Executor · <name> · <ISO-date>"
  - "Captain · <name> · <ISO-date>"
```

Tamper-evidence is the append-only channel + git history — **not** a content-hash. (We tried a content-hash and cut it: it's tamper-evidence *for* an enforcement gate, and we'd deferred the gate. It also failed its first live-fire — two parties couldn't reproduce the same hash from an under-specified "canonical body." Lesson: don't build the evidence for a mechanism you haven't built.)

**A signed body is frozen.** Any edit after signing gets re-affirmed in the open — never a silent "clerical" change.

## How it's used
Every piece of work carries the pair. The human + planning agent draft them with real opinions baked in; the executor **reviews and revises** the course (it's not a blank to fill); everyone signs; the human's blessing on the course is the launch. Then the crew builds within the stated latitude.

## Self-referential by design
blood-oath was authored *as* the pair it governs — its own intent doc (now STRATEGY.md) + SHIPS-COURSE, reviewed and signed exactly the way every launch is. The guard, built as the thing it guards. (And on that founding pass, three separate over-engineering instincts got caught before launch — every one by candor and transparency, not by a mechanism. That's the standard proving itself in the building.)
