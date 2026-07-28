---
name: expand
description: Search for candidates not yet on a candidate's saved list, or do more research on what is already there - new countries, new scholarships, new professors, adjacent fields or funder types, versus verifying a stale link, adding an outreach hook, or promoting something from the unresearched long tail toward a full dossier. Use when someone wants more options, says the current list feels too narrow, asks what else is out there, wants another country considered, wants more schools or professors on file, or wants existing entries double-checked, refreshed, or fleshed out. Not for building the first list - use scout or target for that.
---

# recon: expand

Adds to what `scout` and `target` already found - never replaces it. Read
`../recon/references/schema.md`, `../recon/references/evidence.md`, and
`../recon/references/sources.md` first.

## Not a stage

`/recon:expand` sits outside the fixed `profile → scout → target → gaps →
outreach → roadmap` order. `/recon:recon` never routes into it and it never
auto-chains into anything else. It exists for one reason: the user already
has something on file and wants more of it, on demand.

If the dimension asked about is empty (e.g. the user asks to expand
contacts but no `opportunities[]` exist yet), say so plainly and hand off
to `scout` or `target` instead of inventing a starting point.

## Two questions, every time

Always ask both, up front. Never infer either from phrasing.

1. **Which dimension(s)?** One or more of: locations/systems,
   opportunities/scholarships, contacts/professors, field/funder-type
   breadth.
2. **Breadth, depth, or both?**
   - **Breadth** - search for candidates not yet on file at all.
   - **Depth** - do more research on what is already saved: fill in
     missing detail, re-verify a stale evidence link, add a hook, confirm
     a signal is still live, and - where the entry sits at tier C or B -
     promote it. Depth also applies to an already tier-A entry whose
     evidence is aging or thin; it is not only tier promotion.

## Dimensions

| Dimension | recon.json section | Breadth | Depth |
|---|---|---|---|
| Locations/systems | `systems[]`, `preferences.regions/countries` | new countries/systems | re-verify/enrich an existing one |
| Opportunities | `opportunities[]` | new scholarships in existing systems | fill in missing detail, re-verify, promote tier |
| Contacts | `contacts[]` | new people for existing opportunities | re-verify signal, add hooks, promote tier |
| Field/funder-type breadth | `opportunities[]`, tagged | adjacent fields, underrepresented funder types | not applicable |

## Locations/systems

**Breadth.** Existing `systems[]`, `preferences.regions/countries`, and
`preferences.exclusions` are the exclusion list - never propose what is
already there or what was already turned down. Apply the exact
evidence bar `scout` uses: structural fit, honest money, life there, doors
opened, country quota reality. Actively surface under-searched routes -
industrial doctorates, bilateral agreements, small-quota schemes - the same
doctrine `scout` follows. Accepted additions are appended, never replacing,
to `preferences.countries`/`regions` and to `systems[]`, scored on the same
`cost_now`/`doors_opened` dual ranking as everything already there.

**Depth.** Re-verify an existing system's evidence: refreshed life-there
data, updated cost-of-living, quota reality that may have shifted since it
was last checked. Show the new `as_of` date next to anything re-verified.

## Opportunities

**Breadth.** Within systems already in `systems[]`, search for
scholarships and programmes not already in `opportunities[]`, deduped by id
and name. Same structured `requirements[]` discipline as `target` - gaps
are computed by diffing against these, so prose requirements cannot be
diffed. Same standing-vs-event signal handling. New entries default to
tier C.

**Depth.** Fill in missing `requirements[]` or `financials[]`, re-verify
deadlines against the primary source, promote tier on request following
`target`'s own wave-sizing rule - state the real cost in hours before
promoting a batch.

## Contacts

**Breadth.** For opportunities already saved, search for people not
already in `contacts[]`. Full `target` doctrine applies: hunt for dead
signals deliberately, record `published_filters`, apply the
find-and-replace hook test - if a hook still makes sense with a different
person's name substituted, discard it. Peers only through professional
channels - published author lists, programme pages, public alumni
profiles - and approached as a peer asking for advice, never a pitch.

**Depth.** Re-verify a contact's signal is still live, add more `hooks[]`,
check for `published_filters` not yet recorded, promote tier. Subject to
the person-research fetch discipline below.

## Field/funder-type breadth

Breadth only; there is no depth variant of this dimension.

**Adjacent fields.** Propose fields adjacent to the one the user is
pursuing and search for opportunities in them within systems already
chosen. Save matches into `opportunities[]` with `matched_field` set to the
field they matched (see `schema.md`'s `opportunities` section). Never touch
`preferences.field` - that stays the user's primary stated field, unedited.

**Underrepresented funder types.** Within existing systems, deliberately
search funder types `opportunities[]` is thin on. If the saved list skews
toward `university`, hunt specifically for `industry`, `bilateral`,
`foundation`, `employer` routes - consistently under-searched compared with
the academic route.

**Coverage-gap mode.** Diff `discovered_via` across saved opportunities
and contacts against the classes in `sources.md`. Name the classes that
are absent and search those first - "everything on file came from two
catalogues; no national grant database or bilateral agreement has been
touched" is the kind of finding this produces. An entry with no
`discovered_via` predates the field and is unknown, not absent - do not
count it as a gap.

## Person-research fetch discipline

Both `target` and this skill's contacts dimension research real people -
LinkedIn, lab pages, staff directories - and a fetch can fail or get
rate-limited. Follow `evidence.md`'s rule exactly: attempt the fetch, cap
retries at one, fall back to a cheaper public source (lab page, staff
directory, published author list, ORCID) before trying a heavier fetch
again, never fill in a role or institution from memory when a fetch fails,
and tell the user plainly when something could not be verified. Prefer the
cheapest sufficient source generally - do not run an exhaustive fetch pass
over every candidate when a lighter check already confirms what is needed.

## Dedup and non-destruction

Every breadth search is handed the existing ids/names as an explicit
exclusion list. Depth work never deletes an entry, and never downgrades one
for lack of new evidence - but newly evidenced bad news is not a downgrade.
A contact re-verified as retired, moved on, or under a do-not-contact
policy is marked `dead` with the evidence and reason, and an opportunity
whose cycle has closed is marked accordingly, exactly as `target` already
does - finding this is most of the value, not something depth suppresses.
If the user wants to reconsider something previously excluded in
`preferences.exclusions`, that is a direct edit to preferences, not
something this skill automates - never silently override a prior "no."

## Write state

Every breadth addition to `opportunities[]` or `contacts[]` also
records `discovered_via` (`schema.md`), matching `sources.md`'s
classes.

Writes to `systems[]` or `preferences.regions/countries` stamp
`stages.scout.last_run` with the current ISO 8601 timestamp (date and
time, e.g. `2026-07-20T14:32:00Z`) in the same write. Writes to
`opportunities[]` or `contacts[]` stamp `stages.target.last_run` the same
way. This reuses `/recon:recon`'s existing staleness cascade exactly as if
`scout` or `target` had produced the change directly: a `scout` stamp
flags `target` onward (`target`, `gaps`, `outreach`, `roadmap`) as stale on
the next `/recon:recon` run, and a `target` stamp flags `gaps` onward.
