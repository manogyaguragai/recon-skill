# Evidence

Every claim recon makes carries a link the user can click and check. The user
should never have to take recon's word for anything that affects a decision.

## Credibility depends on the claim, not the domain

A flat source hierarchy is wrong here. Credibility is a function of what is
being claimed.

**Eligibility rules, deadlines, financial terms, application procedure.**
Only the official source counts. daad.de, the ministry page, the university's
own admissions page, the funder's published guidelines. A blog restating an
age limit is worthless next to the primary source, and blogs go stale silently.
If only secondary sources exist, say so explicitly and tell the user to confirm
with the funder before acting.

**Live capacity - who has money and is recruiting.** Grant award databases are
primary and queryable:

- NSF Award Search (US) - active awards by topic, PI, or institution
- CORDIS (EU) - Horizon and Marie Sklodowska-Curie projects
- GEPRIS (Germany, DFG)
- KAKENHI (Japan, JSPS)
- UKRI Gateway to Research (UK)
- National equivalents elsewhere; find them rather than guessing

See `sources.md` for the fuller discovery-surface registry beyond
capacity databases.

Then lab pages, group news, staff directories, and posted vacancies.

**What life is actually like there.** Here the official source is the *worst*
one and lived accounts are the best available evidence. Reddit threads,
student forums, diaspora community posts, cost-of-living reports from people on
that exact stipend. Use them. Label them as anecdote, name roughly when and by
whom, and never launder a forum post into a statement of fact. "Several
students on r/X in 2025 reported the stipend does not cover rent in that city"
is honest. "The stipend does not cover rent" is not.

**Competitiveness.** Published admit statistics where they exist. Where they do
not - which is most places - say the number is unpublished rather than
inventing a plausible one. Unpublished is a finding. A fabricated benchmark is
a lie the user will act on.

## Rules

1. **No source, no assertion.** Say "I could not verify this - check with the
   funder" and mark it unverified in state. A visible hole is useful; a
   confident guess is not.
2. **Never fabricate a citation.** Not a URL, not a statistic, not an
   attribution. If a page cannot be reached, say it cannot be reached -
   this applies with special force to person research (LinkedIn, staff
   directories, lab pages): attempt the fetch, cap retries at one, fall
   back to a cheaper public source before trying a heavier fetch again,
   never fill in a role or institution from memory when a fetch fails, and
   tell the user plainly when something could not be verified. Prefer the
   cheapest sufficient source generally; do not run an exhaustive fetch
   pass over every candidate when a lighter check already confirms what is
   needed.
3. **Date everything.** Every requirement and deadline stores `source_url` and
   `as_of`. Rules move between intake years - age cutoffs, deadlines, quotas.
4. **Re-verify before action.** Before a user commits time or money to a
   target, re-check the primary source. Cached numbers are for planning, never
   for acting.
5. **Separate the rule from the interpretation.** Quote the rule's substance
   and link it; keep recon's reading of it clearly distinct from the rule
   itself. Users need to be able to disagree with the interpretation.
6. **Conflicts stay visible.** When sources disagree, show both and say which
   is primary. Do not silently pick a winner.
7. **Aggregators are discovery surfaces, never citations.** A catalogue
   listing a deadline is not a source for that deadline. Resolve every
   aggregator hit to its primary source before writing anything to
   `source_url`. If the primary cannot be found, record the requirement
   as unverified rather than letting the aggregator link stand in.
8. **Paid-service sources are never cited.** Sites whose business model
   is selling application or agent services are never cited, and
   scholarship aggregators carrying figures with no traceable primary
   are treated as unverified. Recon exists so people do not need to pay
   an agent; citing one would undercut the tool's own purpose.

## Presenting sources

Inline, next to the claim, never in a footnote pile at the end. The user is
scanning to verify one specific thing; make that one thing one click away.

For anything with a deadline or an eligibility bar, also surface the `as_of`
date, so a user reading in December knows the page was checked in July.
