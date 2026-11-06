# SYMBIOSSANCE — Luxury Brand Outreach Agent

**An AI agent that replaces a wholesale broker _and_ a PR firm for a one-person brand.**
It researches each boutique buyer, gift broker, and celebrity stylist, drafts outreach in
the founder's own voice, and stops at the human before anything sends.

Built with Claude Code. Piloted on a real brand — [AirPlantNina](https://airplantnina.com),
living air-plant wearables — by founder **Nina Bazan-Sakamoto**.

> **Case study:** [portfolio write-up](https://claude.ai/artifact/ULmajWXqpUzC8aZxTTDqKF) ·
> **Demo:** [3-min walkthrough](https://www.loom.com/share/4ece526285224fee945eb53ed37ae5fd)

---

## The problem

A solo luxury founder who wants stocked boutiques and celebrity-gifting placements faces two walls:

- **Wholesale brokers** charge a **~$4,000/month** retainer before a single buyer is contacted.
- **Celebrity gifting** needs **100+** cold contacts before one placement sticks.

Done by hand, outreach dies after one or two touches and reads like a template — the fastest
way to be ignored by the buyers and stylists who curate for a living.

## The design

One engine, three parts:

1. **Brand knowledge base** (`brand/`) — the agent reads who the brand is and how the founder
   sounds *before it writes a word*. `voice.md` is the single source of truth for tone.
2. **A research-first skill** (`.claude/skills/outreach-composer/`) — its contract blocks
   drafting until it has 2–3 verified specifics about the lead. If a lead can't be verified,
   confidence drops. **Nothing is invented.**
3. **A human gate** — every draft is written to `data/outreach_drafts/` marked
   `STATUS: AWAITING NINA`, with the connection point named. **The agent never sends.**

```
target market ──► lead-researcher ──► research each lead ──► outreach-composer ──► AWAITING NINA
   (input)         scores + leads.csv    site + socials        draft from overlap      (human gate)
```

### Why this is orchestration, not a mail merge
- **Research before writing** — personalization fails the moment it invents a detail, so invention is designed out.
- **Voice as one source of truth** — change `voice.md`, and every future message changes.
- **A deliberate human gate** — in luxury, the relationship *is* the product; automating the send would break it.
- **One engine, any brand** — swap the `brand/` files and the same agent runs for any independent designer.

## Repo layout

```
symbiossance-agent/
├── README.md
├── .claude/
│   └── skills/
│       └── outreach-composer/
│           └── SKILL.md            # the agent's contract: research → connect → draft → stop
├── brand/                          # the knowledge base (the agent's memory)
│   ├── voice.md                    # how Nina sounds — single source of truth for tone
│   ├── positioning.md              # where the brand sits in the market
│   ├── product.md                  # what the pieces are + plant-care facts
│   ├── industry_agreements_research.md
│   ├── sample_outreach_wholesale.md
│   ├── sample_outreach_gifting.md
│   ├── sample_wholesale_agreement.md
│   └── sample_gifting_agreement.md
└── data/
    ├── leads.csv                   # example input: scored, evidence-backed leads
    └── outreach_drafts/            # example output: drafts held for review
        ├── L001_maison-celeste_wholesale.md
        └── L002_beaumont-creative_gifting.md
```

## Run it

This is a Claude Code agent, not a hosted web app.

1. Install [Claude Code](https://claude.com/claude-code).
2. Clone this repo and open it: `claude` from the repo root.
3. Add your own leads to `data/leads.csv`.
4. Ask the agent to run the `outreach-composer` skill over your leads.
5. Review the drafts in `data/outreach_drafts/` — each is marked `AWAITING NINA`. Approve and send yourself.

To point the agent at a different brand, replace the files in `brand/` — especially `voice.md`.

## Status

- **Built:** working agent — knowledge base, research-first skill, channel-specific drafting, human gate; auto-drafted wholesale & gifting agreements.
- **Piloted:** on AirPlantNina — 8 boutique leads, 2 A-list stylist leads sourced; 10+ drafts held for review.
- **Next:** reply-rate and placement results across a full batch; autonomous follow-up cadence (needs validation).

---

_Built by Nina Bazan-Sakamoto · airplantnina@gmail.com · [@airplantnina](https://airplantnina.com)_
