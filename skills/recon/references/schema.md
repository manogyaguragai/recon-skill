# recon.json schema

Single source of truth. Every stage reads it, updates its own section, writes
it back. Anything a stage cannot verify is omitted or marked, never guessed.

## Top level

```json
{
  "version": "0.2",
  "updated": "2026-07-27",
  "stages": {
    "profile":  { "last_run": null },
    "scout":    { "last_run": null },
    "target":   { "last_run": null },
    "gaps":     { "last_run": null },
    "outreach": { "last_run": null },
    "roadmap":  { "last_run": null }
  },
  "inventory": {},
  "preferences": {},
  "systems": [],
  "opportunities": [],
  "contacts": [],
  "outreach": [],
  "settings": {}
}
```

---

## stages

Tracks whether each stage has run, independent of whatever that stage does
or does not persist elsewhere. `gaps` never stores its computed output and
`roadmap` never stores its dashboard — `last_run` is the only record either
stage leaves behind, and it is what lets `/recon:recon` route a user without
forcing them through every stage in order.

Each stage stamps its own `last_run` with a full ISO 8601 timestamp —
date **and** time, e.g. `2026-07-20T14:32:00Z` — in the same write where it
saves any other data it owns (`profile` → `inventory`, `scout` →
`preferences`/`systems`, `target` → `opportunities`/`contacts`, `outreach` →
`outreach`). `gaps` and `roadmap` stamp `last_run` with nothing else
changing, since they own no persisted section. Nothing but the owning stage
— or `/recon:recon`, when it creates a fresh `recon.json` for a brand-new
user, or `/recon:expand`, when it adds data to a section `scout` or
`target` owns and stamps that stage's `last_run` accordingly — ever writes
to this object.

Date-only is not enough: two updates on the same day must still compare
correctly, and the dispatcher's staleness check (`skills/recon/SKILL.md`)
depends on that. For example:

```json
{
  "profile":  { "last_run": "2026-07-01T09:00:00Z" },
  "scout":    { "last_run": "2026-07-02T14:15:00Z" },
  "target":   { "last_run": null },
  "gaps":     { "last_run": null },
  "outreach": { "last_run": null },
  "roadmap":  { "last_run": null }
}
```

## Provenance

Every inventory fact is an object, never a bare value:

```json
{ "value": 3.62, "provenance": "document", "source": "transcript.pdf p2", "as_of": "2026-07-27" }
```

| Marker | Meaning | May be claimed to a professor |
|---|---|---|
| `document` | extracted from an upload; `source` names file and location | yes |
| `verified_link` | a public URL confirms it - DOI, ORCID, GitHub, university page | yes |
| `stated` | the user said so, nothing backs it | **no** - confirm first |
| `inferred` | recon computed it; `source` shows the derivation | only if inputs qualify |

The rule exists so recon cannot quietly inflate a claim in an email. Ranking
and targeting may use `stated` facts freely.

## inventory

Facts about the candidate. Neutral. No judgement, no scoring - assessment is a
function of a target, computed in `gaps`.

**Eligibility spine** - the boring fields that silently disqualify. Collect
these even when the user thinks they are irrelevant:

- `date_of_birth` - never store an age
- `citizenships[]`, `country_of_residence`, `residence_since`
- `total_years_education` - 12 + tertiary; the field that decides 16-year rules
- `degrees[]` - each with `level`, `field`, `institution`, `duration_years`,
  `awarded_date`, `gpa_native`, `gpa_scale`, `gpa_converted`, `ects` if known
- `current_funding` - holding another government scholarship disqualifies from many
- `military_service` - active service bars several national schemes
- `prior_scholarships[]` - some schemes impose waiting periods on former holders

**Capability**

- `publications[]` - `venue`, `venue_type` (journal / conference / workshop /
  preprint / thesis), `role`, `doi`, `date`. Venue type matters more than count
- `research_experience[]`, `teaching_assistantships[]`
- `languages[]` - `language`, `test`, `score`, `test_date`, `expiry`.
  Language tests expire; an expired score is a gap
- `standardised_tests[]` - GRE, GMAT, subject tests, with dates and expiry
- `technical[]`, `repositories[]`, `open_source[]`
- `awards[]`, `honours[]`, `competitions[]`
- `entrepreneurship[]`, `volunteering[]`, `work[]`
- `coursework[]` - syllabi and contact hours. Feeds credential equivalence,
  which is what decides whether a 3-year bachelor clears a 16-year rule
- `writing_sample` - 200+ words the user actually wrote. Used by `outreach`
  for voice calibration. Not optional if outreach is going to be used

**Constraints**

- `self_contribution_ceiling` - max the user can pay, with currency
- `dependents`, `mobility_constraints`, `earliest_start`, `latest_start`

**Meta**

- `completeness[]` - what recon has asked for and not received. This is a gap
  in recon's knowledge, not a gap in the candidate. Keep the distinction.

## preferences

User-stated targeting preferences: `regions[]`, `countries[]`, `exclusions[]`,
`degree_level`, `field`, `priorities` (ranked: cost, prestige, speed,
immigration pathway, research fit).

Recon proposes systems from the inventory; the user adds and removes freely.
Never silently drop a user's stated preference - if their choice is a poor fit,
say why with evidence and keep it in the list.

## systems

Country or funding-system entries `scout` proposes and the user
accepts. `systems.md` describes the doctrine behind these entries, not
a JSON shape; this schema fixes one shared field:

`sources[]` - optional. The live discovery surfaces `scout` already
checked for this country, each as `{ "class": "...", "surface": "..."
}` matching `sources.md`. `scout` records this in the same write as the
system itself; `target` checks it before searching for a class fresh,
so it reuses what `scout` already found instead of rediscovering the
same boards.

## opportunities

Scholarships, grants, programmes, funded positions, industrial doctorates,
employer-funded routes. Not professor-shaped - `funder_type` is one of
`government`, `university`, `research_council`, `foundation`, `industry`,
`employer`, `bilateral`.

```json
{
  "id": "mext-research-2027",
  "name": "...", "funder_type": "government", "country": "...",
  "degree_levels": ["masters", "phd"], "tracks": [],
  "signal": { "type": "standing", "evidence_url": "...", "verified_on": "..." },
  "requirements": [],
  "financials": {},
  "deadlines": [],
  "life": {},
  "tier": "A",
  "matched_field": null,
  "discovered_via": { "class": "primary_funder", "surface": "mext.go.jp" }
}
```

`matched_field` - set only when this opportunity was found via a
field-breadth search in a field adjacent to the user's primary stated
field (`preferences.field`). `null` for opportunities found the ordinary
way.

`discovered_via` - the source class and specific surface this entry
was found through, e.g. `{ "class": "primary_funder", "surface":
"mext.go.jp" }`. `class` is one of the nine slugs listed near the top
of `sources.md`. Exists so `expand` can compute which classes are
already represented and which are not.

**signal.type** - two kinds, decaying differently:

- `event` - new award, new hire, posted vacancy, lab expansion. Recency
  matters; confidence decays with age but never reaches zero, since an old
  award can still mean a well-resourced group
- `standing` - recurring programme, annual call, doctoral training group,
  endowed fellowship. Age is irrelevant. What matters is whether the **current
  cycle is open**, which must be verified, not inferred from history

Standing capacity is the class most searches miss: a programme running annual
intakes since 2010 generates no recent news and looks dead to a freshness
filter. Check for the live call.

**requirements[]** - structured, because gaps are computed by diffing against
these. Prose requirements cannot be diffed.

```json
{
  "field": "total_years_education", "operator": ">=", "value": 16,
  "kind": "disqualifier", "track": "research",
  "source_url": "https://...", "as_of": "2026-07-27"
}
```

Every requirement carries its source and date. Re-verify before a user acts.

**financials** - `stipend`, `currency`, `period`, `tuition_covered`,
`travel`, `insurance`, `self_contribution_required`, `taxed`,
`cost_of_living_index`, `stipend_to_col_ratio`. Note whether the award is a
scholarship or an employment contract; in employment systems, gross is not net.

**life** - the qualitative layer. Sentiment on cost of living, whether the
stipend is survivable in that specific city, visa and work rights, post-study
pathway, discrimination reports, community presence from the user's region.
Anecdote is the correct source here; label it as anecdote.

**Dual ranking.** Score two axes independently and never collapse them:
`cost_now` (out of pocket after funding) and `doors_opened` (evidenced -
documented placement outcomes, recognition in immigration points systems,
post-study visa rights - not vibes about prestige). Sort by cost by default.
Surface the frontier: options that win on both axes are the real finds and are
usually unglamorous.

**tier** - engagement depth, not quality. Discovery is uncapped; the user's
hours are the real constraint.

- `A` - full dossier, papers read, custom draft. Sized to real capacity
- `B` - logged with enough evidence to decide later. Promotable
- `C` - long tail. Listed and sourced but unresearched. The map, not the plan

Promote C to B to A on request and in waves. Never hide C. Never half-do A.

## contacts

`role` is one of `pi`, `admissions`, `coordinator`, `recruiter`,
`industry_sponsor`, `peer`.

```json
{
  "id": "...", "name": "...", "role": "pi", "institution": "...",
  "opportunity_ids": [], "signal": {},
  "published_filters": [],
  "hooks": [],
  "status": "live",
  "dead_reason": null,
  "discovered_via": { "class": "bibliographic", "surface": "openalex.org" }
}
```

`discovered_via` - the source class and specific surface this contact
was found through, same shape and purpose as `opportunities`'
`discovered_via` above.

`published_filters[]` - instructions the person has published for prospective
students. Required subject line, do-not-email policy, application form to use
instead. Obey exactly.

`hooks[]` - specific, verifiable, non-generic angles for outreach. Each needs a
`source_url`. A hook that survives find-and-replace of the person's name is not
a hook; discard it.

`status` - `live`, `dead`, `deferred`. Hunt for dead signals actively: not
recruiting this cycle, moving institution, group winding down, do-not-contact
policy. Record `dead_reason` with evidence. Not wasting a user's best hours on
a dead lead is most of recon's value.

Peers - someone from the user's region already inside a target programme - are
found through professional channels only, and approached as a peer asking for
advice, never pitched. It converts better and it is the only defensible way to
do it.

## gaps (computed, not stored raw)

`gap = diff(inventory, opportunity.requirements)`, recomputed on every run.
Never freeze gaps - the point is that updating one inventory field reflows every
target at once.

| kind | meaning | action |
|---|---|---|
| `disqualifier` | binary bar - age, citizenship, degree length, concurrent funding | drop or defer. Never "work on this" |
| `threshold` | stated minimum - IELTS 6.0, 2 years experience | closeable on a known timeline |
| `competitive` | unstated but real - typical admit exceeds the user | needs offsetting |
| `differentiator` | absent, not required, would top the pile here | pure upside |

Check in that order; the cheap checks eliminate the most work.

Each gap also carries:

- `closeable_by_deadline` - `yes` / `no` / `partial`, with the lead time that
  makes it true. This is what separates a roadmap action from a fantasy
- `compensable_by[]` - inventory items that can offset it **for this target**.
  Offsets are target-specific: a strong research plan can compensate for a
  mediocre GPA in schemes that weight the proposal heavily, and compensates for
  nothing against a hard threshold
- `cohort_context` - competitiveness depends on the user's passport, not only
  their CV. Where a scheme allocates by country quota, a strong applicant from
  a small-quota country faces far worse odds than the same applicant elsewhere.
  Record it when known and say when it is unpublished

`/recon:gaps` output sorts by **leverage** - how many opportunities each gap
unblocks - not by target. "This one test unblocks eleven of your targets" is
the most useful sentence recon can produce.

## outreach

One record per contact per attempt: `draft`, `assets[]`, `sent_at`,
`channel`, `replied_at`, `outcome`, `followups[]`, `next_action_due`.

Follow-up norm: one short polite reminder after 7-10 days, one or two maximum,
never more. Recon computes when the window opens; the user decides.

## settings

- `gmail_connected` - asked at first use of `outreach`, not at intake. Never
  re-ask once answered. "No" is a fully supported path
- `tracking_mode` - `manual`, `gmail`, `mcp`
- `capacity_per_wave` - how many tier A targets the user can genuinely handle
