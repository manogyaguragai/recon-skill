---
name: recon
description: Research and connect - find fully funded masters/PhD opportunities worldwide, work out who to actually contact, and write outreach that gets replies. Use this whenever someone mentions applying abroad, funded masters or PhD positions, scholarships, grants, assistantships, stipends, DAAD, MEXT, GKS, Fulbright, Chevening, Erasmus, emailing professors, finding a supervisor, research positions abroad, or studying overseas without paying for it. Also use when someone asks what their chances are, whether they are eligible for something, what is missing from their profile, or how to make themselves competitive for graduate funding. Trigger even if they only describe the situation ("I want to do a PhD but cannot afford it", "how do I get someone to fund my masters") without naming a scholarship.
---

# Recon

Research + connect. A staged system for going from a raw candidate profile to
replies from people who can fund a graduate degree.

The premise: fully funded places exist in far greater numbers than most
applicants can see, and most of them are lost not to weak profiles but to bad
targeting, invisible eligibility rules, and outreach that reads like it was
mass produced. Recon attacks those three failures in that order.

## Stages

Each stage is independently callable directly (`/recon:profile`,
`/recon:scout`, etc.) — a user who knows where they are can always jump
straight there. Do not force a user through all of them in order when they
invoke a specific stage by name.

| Stage | Command | Takes | Produces |
|---|---|---|---|
| Profile | `/recon:profile` | CV, papers, transcripts, links, answers | a neutral evidence-backed inventory |
| Scout | `/recon:scout` | inventory + any user preferences | country/funding landscape worth playing |
| Target | `/recon:target` | inventory + chosen systems | named people and programmes, with live-signal evidence |
| Gaps | `/recon:gaps` | inventory + all saved targets | what is missing, ranked by how much it unblocks |
| Outreach | `/recon:outreach` | inventory + one target | a draft that survives a human reading it |
| Roadmap | `/recon:roadmap` | inventory + chosen targets | deadline-backwards plan per opportunity |

If a user invokes a later stage without the earlier data, do not refuse. Run
with what exists and state plainly what could not be checked. Partial state is
valid state. Getting someone a first useful answer beats blocking them behind
a form.

Separately, `/recon:expand` widens or deepens any saved list on demand -
new countries, new scholarships, new contacts, or more research on ones
already saved. It is not part of this order and this dispatcher never
routes into it.

## Entry point: `/recon:recon`

This dispatcher activates only on the literal `/recon:recon` invocation —
not on freeform messages. A message that describes a specific need (e.g.
"what am I missing", "who should I email") should still match the relevant
stage's own description and enter that stage directly, exactly as today;
state-based resolution below is not a substitute for that.

When `/recon:recon` is invoked, decide which single stage to run next and
enter it, using `stages.*.last_run` in `recon.json` (see
`references/schema.md`). If `recon.json` does not exist yet, create it with
the full schema skeleton — every `stages.*.last_run` is `null` — before
resolving.

Resolve in this fixed order — `profile → scout → target → gaps → outreach →
roadmap` — using these three cases, checked in sequence:

1. **Incomplete first pass.** If any stage has `last_run: null`, enter the
   earliest such stage. A brand-new user (every `last_run` null) and a
   returning user who stopped partway resolve identically — there is no
   separate new-vs-returning branch, just "earliest stage never run."
2. **Stale stage.** Otherwise, if any stage's `last_run` is older than an
   earlier stage's `last_run` (in the fixed order above), enter the earliest
   such stale stage. Say plainly why, e.g. "Your inventory changed after
   `scout` last ran — reflowing scout first." Since every stage stamps its
   own `last_run` in the same write as any data it owns, a stage is never
   stale relative to its own output — staleness only ever points at a stage
   *later* in the order than the one whose data just changed.
3. **Current.** Otherwise, enter `roadmap` — the standing "here's where
   things are" view once nothing is stale.

## Auto-chain

When resolution lands on a stage because of case 1 or case 2 above, do not
stop after that single stage. On its completion, re-run the resolution and
continue into whatever it selects next, within the same conversation — no
re-invocation needed — until resolution reaches case 3 (at which point stop;
do not re-enter `roadmap` if it is the stage that just completed) or a stage
pauses for input it does not have.

Two stages structurally pause the chain rather than completing in one pass:

- `profile` pauses whenever it is waiting on documents or answers it does
  not have yet.
- `outreach` pauses to ask which contact to draft for — it acts on one
  contact at a time and cannot be auto-run in a batch the way `scout` or
  `gaps` can.

Resume the chain from wherever it paused once the user supplies what was
missing.

## State

Everything persists in a single file, `recon.json`. It is the source of truth.
Schema and field definitions: `references/schema.md`.

**Claude Code:** `recon.json` lives in the working directory. Read it at the
start of every stage, write it at the end. It is designed to be git-tracked so
users can diff their own progress.

**Claude.ai:** there is no filesystem between conversations. At the start of a
stage, hydrate from any uploaded `recon.json`, from Project knowledge, or from
the conversation. At the end of a stage, always emit the updated `recon.json`
as a downloadable file and tell the user to keep it - ideally in a Project, so
it is in context automatically next time.

Never store state anywhere platform-specific. No browser storage, no hidden
caches. The user owns the file.

## Doctrine

These apply in every stage. They are not style preferences; each one exists
because violating it causes a specific, hard-to-reverse failure.

### Evidence or silence

Every factual claim recon makes carries a link the user can click. Source
credibility is judged per claim type, not on a flat hierarchy - see
`references/evidence.md`. If a claim cannot be sourced, say so and mark it
unverified. Never invent a citation, never assert a deadline or eligibility
rule from memory, and never let a cached number stand in for a live check.
Rules change between intake years. A user who misses a deadline because recon
trusted last year's page has been actively harmed.

### Nothing unverified reaches a real person

The inventory marks every fact with its provenance. Only facts marked
`document` or `verified_link` may appear as claims in outreach. A `stated`
fact can inform targeting and ranking, but must be confirmed with the user
before it is asserted to a professor. Overstating a workshop poster as a
conference paper ends a candidacy permanently, and it ends it in a way the
user cannot see happening.

### Recon never sends

Recon drafts. A human reads it and sends it. Even where a send capability
exists, do not use it. Faculty are actively filtering for machine-written
mail, and an unattended sender is a machine for burning every contact a user
has. Drafting into a drafts folder is fine. Sending is not.

### Age is computed, never stored

Store date of birth. Compute age **at each opportunity's deadline**, not
today. Age caps and birthdate cutoffs move annually, and someone eligible now
may age out before the intake they are targeting.

### Obey published filters

Many faculty publish instructions for prospective students - a required
subject line, a "do not email me, just apply" policy, a form to use instead.
Hunt for these and follow them exactly. Ignoring an explicit instruction is a
self-inflicted disqualification, and following one is free conversion.

### Report volume honestly

A well-crafted personalised cold email gets a reply roughly 10-20% of the
time. Twenty sends yields two to four replies. Never imply better. Never let a
user believe a non-reply means they did something wrong, and never make
statistical claims about what is working from a sample that cannot support
them.

### Scope of person-search

Work only from what people have made publicly professional: lab pages,
published author lists, staff directories, public alumni profiles. Do not
assemble profiles of private individuals, do not cross-reference pseudonymous
accounts to real identities, and do not compile personal details about anyone.
Finding a name and affiliation to send one relevant email is normal academic
behaviour. Building a dossier on a person is not.

## Reference files

Read these when the relevant stage needs them; do not load all of them up front.

- `references/schema.md` - `recon.json` structure, all field definitions,
  provenance markers, gap taxonomy, signal types
- `references/evidence.md` - source tiering by claim type, citation format,
  what to do when nothing credible exists
- `references/voice.md` - outreach doctrine: voice calibration, the audit
  loop, what makes a draft fail
- `references/systems.md` - how funding actually works in different countries,
  and why the same profile is strong in one system and weak in another
- `references/sources.md` - discovery surfaces organised by funding
  model, from vacancy boards to applicant-side national schemes
