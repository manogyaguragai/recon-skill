---
name: profile
description: Build a candidate's evidence-backed inventory for graduate funding applications - CV, publications, transcripts, test scores, awards, coursework, and the eligibility fields that silently disqualify people. Use when someone wants to start recon, asks what documents they need, wants their profile assessed for scholarships or funded PhD/masters applications, says they are preparing to apply abroad, or asks whether their background is good enough. Also use when updating an existing profile after a new test score, publication, or qualification.
---

# recon: profile

Builds the inventory that every other stage diffs against. Read
`../recon/references/schema.md` for field definitions before starting.

**This stage makes no judgements.** It does not score the candidate, assess
competitiveness, or name gaps in their profile. It cannot - a gap only exists
relative to a target, and no target has been chosen yet. Requiring a paper is a
disqualifier in one system and irrelevant in another. Assessment happens in
`gaps`.

The one thing this stage does report is **completeness**: what recon asked for
and did not receive. That is a hole in recon's knowledge, not a flaw in the
person, and the wording should make that obvious.

## Sequence

### 1. Checklist first

Show what recon can use, and let the user select what they have. One line each
on *why it matters*, because for most users this list is the first time anyone
has told them a syllabus or a contact-hour record is worth keeping. This is the
first upskilling moment in the whole system - use it.

Cover at minimum: CV, transcripts, degree certificates, publications or
preprints, course syllabi, language and standardised test reports, award
letters, prior scholarship records, employment and internship letters, teaching
or assistantship records, code repositories and open-source work, volunteering,
competition results, entrepreneurial work, and a sample of their own writing.

Flag the writing sample as required if they intend to use `outreach` later.

### 2. Extract from what they upload

Extract only what is actually in the document. Record `document` provenance
with the file name and location, so any claim can be traced back.

Do not infer, round up, or fill blanks with plausible values. If a transcript
does not state contact hours, that field stays empty. An invented fact here
propagates into an email to a real person.

Verify what is verifiable: DOIs, ORCID, repository URLs, institutional pages.
A confirmed public link upgrades a fact to `verified_link`, which is what
allows it to be claimed in outreach later.

### 3. Ask only for what is missing

Do not re-ask for anything already extracted. Prioritise the eligibility spine,
because those are the fields that decide whether a target is even possible and
users almost never volunteer them:

date of birth (store the date, never an age), citizenships, country of
residence and how long, total years of formal education, duration of each
degree, date each degree was awarded, whether they currently hold any
government scholarship, military service status, prior scholarship history.

Explain briefly why each is being asked. "Some schemes require sixteen total
years of education, which excludes three-year bachelors, so I need the
duration" is a better question than "how long was your degree".

Then constraints: self-contribution ceiling, earliest and latest start,
dependents, mobility limits, and any location preference.

### 4. Human verification

End with a full rundown of everything collected, grouped and readable, each
item showing its provenance. Ask the user to correct anything wrong.

This is not a formality. Every downstream stage trusts this file, and some of
it will end up in front of people who can check. Make corrections easy - the
user should be able to say "that paper was a workshop, not the main conference"
and have it fixed.

Pay particular attention to publication venue types. The difference between a
workshop poster and a main-track paper is invisible in a CV line and enormous
to a reader who knows the field.

### 5. Write state

Write `recon.json`, stamping `stages.profile.last_run` with the current ISO 8601 timestamp (date and time, e.g. `2026-07-20T14:32:00Z`) in the same write. In Claude.ai, emit it as a downloadable file and
tell the user to keep it, ideally in a Project so it loads automatically next
time.

## Partial is fine

Intake is long and nobody finishes in one sitting. Save whatever exists, record
the rest in `completeness`, and let the user leave. If they want to run `scout`
on a half-finished inventory, do it - and state clearly which checks could not
be performed yet.

Never ask about Gmail or tracking here. That question belongs at first use of
`outreach`, when it has a reason to exist.
