---
archetype: F
name: Founder / Head of Growth
agent: morning-signal-digest
---

# Archetype F: Founder / Head of Growth

## Who they are
Founder, CEO, or Head of Growth at an early-stage company (10-100 people). Small team wearing multiple hats. Responsible for pipeline, outbound, and growth. No dedicated ops person to watch accounts between calls.

## The manual loop (what they do today)
1. Check campaigns manually each morning — or don't
2. Miss buying signals on pipeline accounts because no one is watching
3. Unread replies sit in the sequencing tool inbox for hours
4. Weekly team update assembled manually from multiple tools

## The agent loop

**External trigger**: Daily cron at 7am

**What the agent does autonomously**:
1. Scans all open pipeline accounts for overnight signals: LinkedIn activity from key contacts, funding news, job postings, tech stack changes
2. Checks campaign inboxes for new replies, classifies each one (Interested / Objection / Not Now)
3. Flags accounts with significant signal changes
4. Pulls unactioned inbound leads from prior day
5. Synthesizes into a prioritized digest: what changed, who to reach out to today, what needs a reply — with drafted actions attached

**Human-as-a-tool moment** — Slack message at 7am:
> "Morning. 3 things need you today:
> 1. [Name] at [Company] posted about switching their data stack — fits your pitch. Suggested opener: [draft]. [Send] [Edit]
> 2. [Company] raised $18M Series B overnight. 2 contacts in your pipeline. [Alert reps] [Skip]
> 3. [Name] replied to campaign — classified Interested. Draft response ready. [Send] [Edit]
> Nothing urgent on the rest."

Founder spends 3 minutes approving actions. Agent executes.

## Before / After
Before: Pipeline signals get missed between calls. Replies sit unread. Team updates take 30 min to assemble.
After: Everything that happened overnight across the pipeline is prioritized and actionable in one Slack thread. 3 minutes to clear it.

## To build this on DataGen

1. Add from marketplace: `/plugin marketplace add datagendev/datagen-plugin`
2. Install the plugin: `/plugin install datagen --scope project`
3. Restart Claude Code so it can load the plugin
4. Authenticate: `/datagen:setup` — signs in through browser, installs the CLI, creates project context
5. Connect tools: `/datagen:add-mcps` — add LinkedIn, Firecrawl or Perplexity, HubSpot or Salesforce, Instantly or HeyReach, Slack
6. `/datagen:build-agent morning-signal-digest` — agent: daily cron at 7am; scans all open pipeline accounts for overnight signals (LinkedIn activity from key contacts, funding news, job postings, tech stack changes via Firecrawl/Perplexity); checks Instantly/HeyReach campaign inboxes for new replies and classifies each as Interested/Objection/Not Now; flags accounts with significant signal changes; pulls unactioned inbound leads from prior day; synthesizes into a prioritized digest with drafted actions per item (reply draft, rep alert, follow-up booking); posts to Slack with per-item approve/skip buttons — approved actions execute immediately
7. `/datagen:deploy-agent morning-signal-digest` — trigger: daily cron at 7am, channel: Slack + email, subscribers: you
8. Set subscribers: your Slack handle and email; each approved action executes immediately — reply sent, rep alerted, follow-up booked

**First event**: Tomorrow morning at 7am you'll get a digest of everything that happened overnight across your pipeline — signals, replies, and actions ready to approve in one Slack thread.
