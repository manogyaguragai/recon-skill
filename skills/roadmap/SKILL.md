---
name: roadmap
description: Build a deadline-backwards plan for each scholarship, programme, or funded position a candidate is pursuing - what has to happen by when, in what order, and which gaps cannot be closed in time. Use when someone asks what to do next, how to prepare for an application, whether they can make a deadline, what their timeline looks like, or wants a plan to become competitive for a specific opportunity. Also use when they want to see all their applications and deadlines in one place.
---

# recon: roadmap

Turns gaps into a dated plan, one per opportunity. Run `gaps` first, or run it
inline - the roadmap is meaningless without current gap data.

## Plan backwards from the deadline

The deadline is the only fixed point. Everything else is placed by working
backwards from it with real lead times:

- language and standardised tests - registration windows, sitting dates, score
  reporting turnaround, and validity periods
- recommendation letters - weeks of notice, and recommenders need briefing
  material
- credential evaluation and document legalisation - frequently months, and the
  step most often discovered too late
- research proposals - drafting, supervisor feedback, revision
- supervisor contact - in systems where an acceptance letter is required before
  the application, this must complete *before* the deadline, not near it
- translations, apostilles, medical certificates, financial documentation

Working backwards is what makes an unclosable gap visible. Forwards planning
lets someone believe they will handle a four-month credential evaluation in
three weeks. Backwards planning shows the collision.

## Mark what cannot be made

Be direct. If a gap cannot close before a deadline, say so and give the two
real options: target the next cycle, or drop it. Do not produce an
aspirational plan that quietly requires something impossible - that costs
someone a year, and they only find out at the end.

Distinguish clearly:

- **Actionable now** - closeable, with the lead time and the start-by date
- **Deferred** - the target is right, the cycle is wrong. Next intake, with the
  date to begin
- **Dead** - structurally barred. Show the bar and its source and remove it
  from the plan

## Sequence by leverage

Where one action serves several opportunities, put it first. A single language
test that unblocks eleven targets outranks a paper that unblocks two, even if
the paper feels more impressive. Carry the leverage ordering from `gaps`
straight into the sequence.

## Critical path

Name the one thing that, if it slips, takes the whole application down. Users
optimise the visible, satisfying work - polishing a statement of purpose - and
miss the boring document that takes twelve weeks. Say which is which.

## Output

Per opportunity: a backwards-planned timeline anchored on its deadline, with
each item showing its lead time, start-by date, dependencies, and evidence
link for any date drawn from an official source.

Across opportunities: a combined view sorted by deadline, so collisions between
applications are visible. Two applications needing the same recommender in the
same fortnight is a real problem and should surface before it happens.

Verify every deadline against its primary source before presenting a plan built
on it, and show the `as_of` date. A plan built on a cached deadline is worse
than no plan.

## Rendering

The visual is generated from `recon.json`, not authored by hand each run - see
`scripts/render_dashboard.py`. The dashboard is a renderer over state and is
disposable. Regenerate it freely; never treat it as the source of truth and
never let a user edit it expecting the change to persist.

Stamp `stages.roadmap.last_run` with the current ISO 8601 timestamp (date and time, e.g. `2026-07-20T14:32:00Z`) in `recon.json`
at the end of every run — the dashboard itself stays disposable and
unstored, as above; only the timestamp is new.
