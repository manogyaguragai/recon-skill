# Discovery sources

Where `scout` and `target` actually look. Both specify what to look for
in exhaustive detail and never where - this file is the missing layer.
It exists because "discovery is uncapped" and "niche is where the odds
live" (`systems.md`) are unenforceable without concrete surfaces to
check, and the default failure is falling back to the same twenty
famous scholarships that `systems.md` already warns against.

This file is a floor, not the universe. Stopping here is a failure -
every search must reach at least one surface not listed below. Source
class determines citability: see `evidence.md`, especially rules 7 and
8, for what a class can and cannot be cited for. A catalogue hit is a
lead, not a source.

Class slugs, used verbatim in `discovered_via.class` (`schema.md`) and
in `expand`'s coverage-gap diff: `catalogue`, `vacancy_board`,
`capacity_database`, `primary_funder`, `structured_programme`,
`institute_direct`, `bibliographic`, `peer_and_lived`,
`applicant_side_national`. These are the only nine classes -
`Non-English search` and `Rot` below are cross-cutting notes, not
additional classes.

Each class below sketches what question it answers, whether it maps
onto one of the four funding models (`systems.md`), any coverage bias
worth knowing, and whether it is primary or an aggregator - not every
class carries all four.

## Catalogue

What programmes exist. Aggregator, discovery only - never cite a
catalogue for a deadline or eligibility rule; resolve to the primary
source first (`evidence.md`).

- Mastersportal / PhDportal - mastersportal.com, phdportal.com
- FindAMasters / FindAPhD - findamasters.com, findaphd.com
- ScholarshipPortal - scholarshipportal.com
- The DAAD programme database - daad.de
- The Erasmus Mundus joint masters catalogue - eacea.ec.europa.eu
- Official Study-in-X national portals - one per destination country;
  find the specific one at runtime rather than guessing the domain

## Vacancy board

Who is hiring now. The employment model's real surface - in Germany,
the Netherlands, and the Nordics a PhD is a job posting, not an
admissions page, so this class is where the employment model actually
lives.

- EURAXESS - euraxess.ec.europa.eu
- AcademicPositions - academicpositions.com
- jobs.ac.uk
- AcademicTransfer (Netherlands) - academictransfer.com
- Nature Careers - nature.com
- Science Careers - science.org

National vacancy boards exist in most European countries beyond these.
Find the specific one per country at runtime.

## Capacity database

Who has money. Serves the employment and structured-programme models by
surfacing funded groups directly. `evidence.md` already lists NSF Award
Search, CORDIS, GEPRIS, KAKENHI, and UKRI Gateway to Research as the
primary, queryable set - that list is authoritative; it is not repeated
here. Add to it:

- NIH RePORTER - reporter.nih.gov
- SNSF - snf.ch
- NWO - nwo.nl
- FWF - fwf.ac.at
- The ERC funded-project database - erc.europa.eu

## Primary funder

Eligibility, deadlines, financial terms. The only citable source for
any of these - see `evidence.md`'s claim-credibility section. Serves
the central-scholarship model.

- DAAD - daad.de
- Campus France - campusfrance.org
- Swedish Institute - si.se
- MEXT / JASSO (Japan) - mext.go.jp, jasso.go.jp
- NIIED (Korea, GKS) - niied.go.kr
- China Scholarship Council - csc.edu.cn
- Turkiye Burslari - turkiyeburslari.gov.tr
- Chevening - chevening.org
- Commonwealth Scholarship Commission - cscuk.fcdo.gov.uk
- Fulbright - fulbrightonline.org
- Erasmus+ - erasmus-plus.ec.europa.eu

## Structured programme

Cohort intakes that generate no news and are invisible to
freshness-based search - the class `target` already names as the one
freshness search misses entirely.

- The EU Funding & Tenders Portal, for MSCA doctoral networks -
  ec.europa.eu
- DFG research training groups - dfg.de
- The UKRI CDT/DTP directory - ukri.org
- IMPRS and other institute graduate schools - found via the sponsoring
  institute's own page; do not guess a central aggregator domain

## Institute-direct

Non-university research routes.

- CERN - home.cern
- EMBL - embl.org
- ESA - esa.int
- ICTP - ictp.it
- Fraunhofer - fraunhofer.de
- CGIAR centres - cgiar.org

National laboratories are a further exemplar of this class; find the
specific lab rather than guessing a central listing.

## Bibliographic

Who is publishing in this niche, recently.

- OpenAlex - openalex.org
- Semantic Scholar - semanticscholar.org
- ORCID - orcid.org
- Google Scholar - scholar.google.com
- dblp - dblp.org

OpenAlex is API-queryable. Pulling recent papers in a subfield,
extracting corresponding authors and affiliations, then
cross-referencing against a capacity database is a stronger
contact-discovery method than browsing lab pages one at a time.

## Peer and lived

What life there is actually like. `evidence.md` already establishes
anecdote as the correct source class here, labelled as anecdote, never
laundered into fact - that rule applies to everything found through
this class.

- TheGradCafe - thegradcafe.com
- Subject and country subreddits
- Programme alumni
- Diaspora community forums

## Applicant-side national

The applicant's own ministry of education outbound scholarship lists,
their embassy and foreign-ministry pages, and bilateral education
agreements involving their country. Named as a class, not a list of
countries - find the specific pages for the user's own citizenship at
runtime.

This is where small-quota schemes with tiny applicant pools live, and
it is the most under-searched class of all: nobody browses another
country's ministry site, so almost nobody finds these.

## Non-English search

Searching in the destination country's language surfaces listings an
English query never returns. Same doctrine as the under-searched routes
in `systems.md` - the odds are better where fewer people are looking,
and a language barrier is one more reason nobody is looking.

## Rot

`as_of: 2026-07-28`. A surface that cannot be reached is reported as
unreachable, exactly as `evidence.md` requires for any other source -
never worked around by substituting a remembered URL.
