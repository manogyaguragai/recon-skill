---
name: outreach
description: Write tailored cold emails to professors, admissions staff, recruiters, and programme coordinators for funded masters and PhD positions - and manage follow-ups and replies. Use when someone wants to email a professor, contact a supervisor, reach out about a research position or scholarship, needs help wording an academic cold email, wants their draft reviewed, asks how to follow up after no reply, or has received a reply and needs to respond well. Never sends anything; always hands the draft to the human.
---

# recon: outreach

Read `../recon/references/voice.md` before drafting. It is the operative file
for this stage.

## Preconditions

Do not draft without a `writing_sample` in the inventory. Ask for one - any 200
words the user actually wrote, an old statement of purpose, a project README, a
long message they sent someone. Two minutes of their time, and it is the single
highest-leverage input to this stage. Drafting in a generic house voice is how
this fails.

Do not draft without a verified hook. If `target` could not produce one that
passes the find-and-replace test, say so and recommend either deeper reading or
dropping the contact. **A weak email is worse than no email** - it does not
fail neutrally, it burns the contact permanently and invisibly.

Check `published_filters` and obey them exactly.

## Tracking setup - ask here, not at intake

The first time a user reaches this stage, offer the choice once:

> Want me to put drafts straight into your Gmail drafts folder and watch for
> replies? Or I can hand you the text and you tell me when it goes out.

Record the answer in `settings` and never ask again. Both paths are fully
supported and the manual one is the default that always works. Do not nag.

- **Manual** - recon drafts, user sends from anywhere, user reports sends and
  replies. Zero dependencies
- **Gmail connected** - drafts land in their drafts folder; thread search
  detects replies and computes follow-up windows
- **MCP** - same, wired through a configured server in Claude Code

Recon must never require a connector. Most installs will not have one.

## Recon never sends

Draft, hand over, human sends. This holds even where a send capability exists.
The gap between drafting and sending is where a person reads their own email
and catches the thing that would have embarrassed them. Removing that gap is
how a tool becomes a spam machine.

## Format by contact type

**Academics** - plain text, short, CV attached. No infographics, no video
links, no branding. Visual production reads as marketing and marketing is
precisely the smell being filtered for.

**Industry, recruiters, employer-funders** - different register, more warmth,
and visual assets are normal and welcome. If the user wants a video, recon can
write the script, shot list, and storyboard for them to record; recon cannot
record it.

## Draft, audit, revise

Never ship a first draft. Run the loop in `voice.md`:

1. Draft using the four moves - hook, bridge, the user, the ask
2. Ask explicitly what makes it read as machine written, and answer honestly
3. Revise against that answer
4. Find-and-replace test - swap in another name and institution. If it still
   works, the hook is generic. Go back
5. Provenance test - every claim about the user traces to `document` or
   `verified_link`. Anything `stated` gets confirmed with the user or cut

Then show the user the draft and say what was verified and what was not.

## The ask

Small, clear, easy to say yes to. Never open by asking for money.

Where the user has their own portable funding, or has applied for it, say so
early and plainly - in systems where the supervisor pays from a project budget,
it removes the single biggest barrier to hiring them. Where they do not have
it, do not lead with the request.

## Follow-ups

One short polite reminder after seven to ten days. One or two maximum. Never
more. Recon computes when the window opens and tells the user; the user
decides. A third follow-up converts nobody and costs the contact.

## Replies

When a reply arrives - detected or reported - flag it immediately, update
`outcome` and status, and offer to draft the response. Keep the loop open for
that contact until it resolves into an outcome.

Match their length and register. A two-line question gets a short answer, not
an essay. Answer what was asked before adding anything. This moment is the
highest-leverage point in the whole process and the most commonly fumbled -
usually by being too eager, too long, or too slow.

Record everything in `outreach`, stamping `stages.outreach.last_run` with the
current ISO 8601 timestamp (date and time, e.g. `2026-07-20T14:32:00Z`) in the same write. It is what makes later analysis
possible, with the honesty caveat that small samples do not support strong
conclusions.
