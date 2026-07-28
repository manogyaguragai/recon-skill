---
name: scout
description: Map which countries and funding systems are actually worth pursuing for a given candidate - comparing scholarship structures, stipend versus real cost of living, visa and post-study pathways, and what life there is genuinely like. Use when someone asks where they should apply, which country is best for a funded masters or PhD, whether a destination is worth it after funding, how far a stipend goes somewhere, or wants to compare study destinations. Also use when they have named countries themselves and want the landscape checked before committing.
---

# recon: scout

Decides which games are worth playing before any effort goes into individual
targets. Read `../recon/references/systems.md`,
`../recon/references/evidence.md`, and `../recon/references/sources.md`
first.

## Propose, then let the user steer

Recon proposes a shortlist from the inventory. Users self-select badly - they
name the countries they have heard of, which are usually the ones where their
profile is weakest, and never consider the systems where it would actually
play. Someone with a three-year bachelor and no publications will name the US
and Japan and be structurally disadvantaged in both.

So lead with an evidence-backed proposal explaining *why each system suits this
particular profile*. Then take the user's own preferences - region, country,
state, city - and add them. Never silently drop a stated preference. If it is a
poor fit, say why with sources and keep it on the list; it is their life.

## What to produce per system

**Structural fit.** Which of the four models it uses, and what that means for
this candidate. Whether supervisor contact is the route, is required, or is
discouraged. Whether their degree structure clears the common bars.

**The money, honestly.** Not the headline stipend. Whether it is a scholarship
or an employment contract, whether it is taxed, what the deductions are, and
the stipend-to-cost-of-living ratio in the actual cities involved. A fully
funded place in an expensive city can leave someone worse off than a smaller
award elsewhere.

**Life there.** Cost pressure, housing reality, visa and work rights, family
rights, whether there is a community from the user's region, and discrimination
or isolation reports. Lived accounts are the right sources here - forums,
student threads, diaspora posts. Label anecdote as anecdote and say roughly
when it was written.

**Doors opened, evidenced.** Post-study visa rights, recognition in immigration
points systems, documented placement outcomes. Not prestige vibes. If the only
argument for a destination is reputation, say that is the only argument.

**Country quota reality.** Where awards are allocated per nationality, the
user's passport changes their odds more than their CV does. Where allocations
are unpublished, say so.

## Dual ranking

Score `cost_now` and `doors_opened` separately and never collapse them into one
number. Sort by cost by default, since that is usually the binding constraint,
but always surface the frontier - the systems that win on both axes. Those are
the real finds and they are usually the unglamorous ones.

Present both scores and let the user decide. A cheaper option now and a more
door-opening option later is a genuine trade-off, not a mistake to be corrected.

## Breadth

Discovery is uncapped. Do not narrow to famous destinations - the odds are
better where nobody is looking. Actively include under-searched routes:
industrial doctorates, bilateral agreements involving the user's country,
non-university research institutes, field-specific foundations, regional
awards, and small-quota schemes with tiny applicant pools.

`sources.md` names where these under-searched routes
actually turn up: the catalogue class for programmes proper, the
primary-funder class for government and bilateral schemes, and the
applicant-side national class for the user's own ministry and embassy
lists - the most under-searched surface of all.

Write the chosen systems to `recon.json` under `systems`, each with its
evidence links, stamp `stages.scout.last_run` with the current ISO 8601 timestamp (date and time, e.g. `2026-07-20T14:32:00Z`) in
the same write, and hand off to `target`.

Record the discovery surfaces checked for each system as
`systems[].sources[]` (`schema.md`), so `target` does not have to
rediscover them.
