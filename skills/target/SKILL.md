---
name: target
description: Find the specific opportunities and named people worth contacting for funded graduate study - professors with fresh grant money, recruiting labs, doctoral training programmes, admissions coordinators, industry sponsors, and peers already inside a programme. Use when someone asks which professors to email, who to contact about a PhD or funded masters, how to find a supervisor, which labs are hiring, where to find scholarships in a given country or field, or wants a shortlist of people and programmes to approach.
---

# recon: target

Turns chosen systems into named opportunities and named people, each with
evidence that it is live. Read `../recon/references/schema.md`,
`../recon/references/evidence.md`, and `../recon/references/sources.md`
first.

## Two signal classes, decaying differently

**Event signals** - a grant awarded recently, a new faculty hire, a posted
vacancy, a lab expanding, a cohort graduating out. Query the award databases
directly rather than guessing: NSF Award Search, CORDIS, GEPRIS, KAKENHI, UKRI
Gateway to Research, and the national equivalent wherever the user is looking.
A principal investigator whose multi-year award started six to eighteen months
ago has budgeted headcount and a staffing problem.

Freshness raises confidence but its absence proves nothing. An award that began
years ago can still mean a well-resourced group. Decay confidence with age;
never zero it.

**Standing signals** - recurring programmes, annual calls, doctoral training
groups, endowed fellowships, institutional schemes that have run for a decade.
These produce no news and are invisible to freshness-based searching, which is
exactly why they are under-applied. A scheme running since 2010 will have no
2026 announcement and may still be perfectly live.

For standing signals, verify the **current cycle is open**. Do not infer from
history - training groups do end, and a final cohort is a real thing.

## Targets are not all professors

`funder_type` spans government, university, research council, foundation,
industry, employer, and bilateral. `contact.role` spans principal investigator,
admissions officer, programme coordinator, recruiter, industry sponsor, and
peer.

Employers who fund study and educational routes that lead to employment are
legitimate targets and are consistently under-searched compared with the
academic route.

## Peers

Someone from the user's region who is two years into the exact programme is
frequently worth more than the professor. They answer honestly, they know what
the application actually rewards, and they can refer.

Find them through professional channels only - published author lists,
programme pages, public alumni profiles, association directories. Do not
cross-reference pseudonymous accounts to real identities and do not compile
personal details. Approach as a peer asking for advice, never as a pitch. It is
the only defensible method and it converts better anyway.

## Published filters

Search for each contact's own instructions to prospective students. Required
subject lines, do-not-email policies, forms to use instead, stated windows.
Record them in `published_filters` and obey them exactly. Following one is free
conversion. Ignoring one is a self-inflicted disqualification.

## Hunt for dead targets deliberately

Actively look for reasons *not* to contact someone: not recruiting this cycle,
moving institution, group winding down, on leave, retiring, an explicit
do-not-contact policy, funding ended. Mark them `dead` with the evidence and
the reason.

This is unglamorous and it is most of the value. A user has a limited number of
good hours; spending them on a lab that closed is the expensive failure.

## Hooks

For each live contact, record specific angles for outreach, each with a source
URL. The best material is a limitation or open thread from work published in
the last one to three years.

Apply the find-and-replace test now, not at drafting time: if the hook would
still make sense with a different person's name substituted, it is not a hook.
Discard it and read more, or mark the contact as needing deeper research before
it can be promoted.

## Tiering

Discovery is uncapped - list everything found, with sources. Engagement is
tiered, because the constraint is the user's hours, not recon's reach.

- **A** - full dossier, papers read, hooks verified, ready to draft. Sized to
  what the user can genuinely do in one wave
- **B** - enough evidence recorded to decide later. Promotable on request
- **C** - the long tail. Listed and sourced, unresearched. This is the map

State the real cost when presenting: a hundred targets is not a hundred emails,
it is a hundred hours, because doing it properly means reading several papers
per person. Let the user set wave size with that number in front of them.
Never hide tier C and never half-do tier A.

## Coverage

Check `systems[].sources[]` first - surfaces `scout` already found
there for this system count toward the classes below without
rechecking.

Before completing, confirm at least one surface was reached from each
of: vacancy board, capacity database, primary funder, structured
programme, institute-direct, and applicant-side national
(`sources.md`). Report any class that could not be covered, and why.

Reach at least one surface not listed in `sources.md`. That file is a
floor - stopping at it reproduces the narrow search this stage exists
to avoid.

## Write state

Write `opportunities` and `contacts` to `recon.json`, stamping
`stages.target.last_run` with the current ISO 8601 timestamp (date and time, e.g. `2026-07-20T14:32:00Z`) in the same write.
Every new entry records `discovered_via` - the source class and surface
it came from (`schema.md`).
