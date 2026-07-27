---
name: gaps
description: Work out what is actually missing from a candidate's profile relative to specific scholarships, programmes, and supervisors - which bars disqualify them outright, which minimums they can still meet in time, and which single improvement unlocks the most opportunities. Use when someone asks am I eligible, what are my chances, what is missing, what should I work on next, how do I become competitive, or whether a new test score or publication changes anything. Also use after any profile update to reflow eligibility across every saved target.
---

# recon: gaps

Computes `gap = diff(inventory, opportunity.requirements)` across every saved
target. Read `../recon/references/schema.md` for the taxonomy.

**Always recompute. Never read stored gaps.** The entire point is that changing
one inventory field reflows every target at once. A user who finally sits a
language test should see three blocked targets flip to viable in a single run.
That recompute is the product.

Stamp `stages.gaps.last_run` with the current ISO 8601 timestamp (date and time, e.g. `2026-07-20T14:32:00Z`) in `recon.json` at
the end of every run — this records only that `gaps` ran, never the computed
gaps themselves, which stay recomputed-only as above.

## Compute age at the deadline

Never against today. Store date of birth, evaluate against each opportunity's
own deadline. Age caps and birthdate cutoffs move annually, and someone
eligible today can age out of the intake they are targeting. Getting this wrong
gives a false green light on a target that is already impossible.

## Check in cost order

**1. Disqualifiers.** Binary bars: age, citizenship, residency, total years of
education, degree duration, degree recency, concurrent government funding,
military service, prior-recipient waiting periods, track conflicts. Cheap to
check and they eliminate the most work.

Say it plainly when something is a bar. A three-year bachelor against a
sixteen-year-education rule is not a gap to work on, it is a target to drop or
a case to argue with coursework evidence. Never present a hard bar as
something to improve toward - that wastes months.

Distinguish permanent bars from clocks. "Your last degree must be under six
years old and yours is four" is not a pass, it is a deadline. Surface it as
urgency.

**2. Thresholds.** Stated minimums - test scores, GPA floors, required years of
experience. Closeable, with known lead times. Attach the lead time.

**3. Competitive.** Unstated but real: typical successful applicants exceed the
user. Say when the number is published and say when it is not. An unpublished
benchmark is a finding; a fabricated one is a lie the user will act on.

**4. Differentiators.** Not required, would put the user near the top for this
specific target. Pure upside.

## Two attributes on every gap

**closeable_by_deadline** - `yes`, `no`, or `partial`, with the lead time that
makes it true. Language tests need booking and results turnaround.
Recommenders need weeks. Credential evaluation can take months. This is what
separates a real action from an optimistic one, and it is the field that makes
the roadmap honest.

**compensable_by** - which inventory items can offset this gap *for this
target*. Offsets are target-specific: where a scheme weights the research
proposal heavily, a strong proposal genuinely compensates for a middling GPA.
Against a hard threshold it compensates for nothing. Never generalise an offset
across targets.

## Cohort context

Competitiveness depends on the user's passport, not only their CV. Where a
scheme allocates by country quota, a strong applicant from a small-quota
country faces far worse odds than the same applicant elsewhere. Record it where
published and say explicitly when it is not.

## Output: sort by leverage

Not by target. Rank every gap by how many opportunities closing it would
unblock, weighted by whether those opportunities are actually good fits.

"One language test unblocks eleven of your targets, three of them in your top
tier" is the most useful sentence recon can produce. It converts a scattered
list of shortfalls into a ranked queue.

Then show, per gap: what it unblocks, the lead time, whether it can be closed
before the relevant deadlines, and what could offset it instead.

Separately list targets that are **dead for structural reasons**, with the
specific bar and its source link, so the user can verify and stop thinking
about them. Removing a target from someone's mental load is a real deliverable.

## Honesty about outcome data

Once the user has been sending, they will want to know what is working. With a
realistic reply rate of ten to twenty percent, their first twenty sends produce
two to four replies. That cannot support a claim about which approach converts
better. Surface patterns as hypotheses, say the sample is too small, and never
dress a hunch as a finding.
