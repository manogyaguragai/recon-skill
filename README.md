# recon

**Research + connect.** Find fully funded masters and PhD opportunities
worldwide, work out who to actually contact, and write outreach that does not
read as machine written.

Built for people chasing funded places with a stipend, from anywhere, without
paying an agent.

## Use it in the Claude UI (claude.ai)

No CLI needed. In the Claude web/desktop app:

1. Go to **Settings → Plugins → Add → Add marketplace**.
2. Choose **Add from a repository** and paste: `https://github.com/manogyaguragai/recon-skill`
3. Tick **Sync automatically**, then click **Sync**.
4. Click the **+** icon next to **recon** to enable it.
5. Type `/recon` in a chat to start.

## Why

Most funded places are lost to three things, in this order: bad targeting,
invisible eligibility rules, and outreach that reads mass produced. Recon
attacks them in that order.

- **Targeting** - country funding systems are different games, not variations.
  The same profile is strong in one and structurally weak in another.
- **Eligibility** - a three-year bachelor, a degree awarded seven years ago, or
  an age cutoff can bar someone permanently, and nobody tells them.
- **Outreach** - faculty now filter for machine-written mail. A bad email does
  not fail neutrally; it burns the contact invisibly.

## Stages

Each is independently callable. Enter wherever you are — or run
`/recon:recon` and it will read your saved state and pick up wherever you
left off, including a brand-new run if you have never used it before.

| Command | Does |
|---|---|
| `/recon:profile` | Builds your evidence-backed inventory |
| `/recon:scout` | Which countries and funding systems are worth playing |
| `/recon:target` | Named people and programmes, with proof they are live |
| `/recon:gaps` | What is missing, ranked by how much it unblocks |
| `/recon:outreach` | Drafts that survive a human reading them |
| `/recon:roadmap` | Deadline-backwards plan per opportunity |

Or just describe your situation and the right stage loads itself.

## `/recon:expand`

Not a stage — a standing command for when you already have a scouted list
of countries or a targeted list of scholarships and contacts, and want
more.

- **Breadth** - search for candidates not yet on your list: new countries,
  new scholarships, new professors, or fields and funder types you haven't
  considered.
- **Depth** - do more research on what's already saved: verify a stale
  link, add a specific outreach hook, confirm a signal is still live, or
  move something from the unresearched long tail toward a full dossier.

Ask for either, or both, across any of four things: locations,
scholarships, contacts, or field/funder-type breadth. `/recon:expand`
never removes or overwrites what's already there — it only adds and
strengthens.

## Install

**Claude Code**

```
/plugin marketplace add manogyaguragai/recon-skill
/plugin install recon@recon-plugins
```

To get updates later: `/plugin marketplace update recon-plugins`.

**Claude.ai** - upload the skill files, keep your `recon.json` in a Project so
it loads automatically each session.

## Your data

Everything lives in one file, `recon.json`, in your working directory. Plain
JSON. Git-trackable, diffable, portable, yours. No browser storage, no hidden
caches, nothing platform-specific.

In Claude.ai, recon hands you the updated file at the end of each session.

## Principles

**Evidence or silence.** Every claim carries a link you can click. Nothing is
asserted from memory. If it cannot be sourced, it says so.

**Nothing unverified reaches a real person.** Facts are marked by provenance,
and only document-backed or link-verified facts can be claimed in an email.

**Recon never sends.** It drafts. You read it and send it. The gap between
those two is where you catch the thing that would have embarrassed you.

**Honest volume.** A well-crafted personalised cold email gets a reply roughly
10-20% of the time. Recon will not imply better, and a non-reply does not mean
you did something wrong.

**Discovery is uncapped, engagement is tiered.** The odds are better where
nobody is looking. But a hundred targets is a hundred hours, and recon says so
before you commit.

## Not a spam tool

Recon will refuse to produce a draft it cannot ground in something specific and
true. If there is no real hook, it says to read more or drop the target. That
is the feature.

## License

MIT.
