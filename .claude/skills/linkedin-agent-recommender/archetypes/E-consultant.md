---
archetype: E
name: Consultant / Fractional GTM / Sales Coach
agent: expert-research-agent
---

# Archetype E: Consultant / Fractional GTM

## Who they are
Fractional CMO, GTM consultant, RevOps advisor, or sales coach. Has a proprietary methodology or framework. Works with 4-10 clients simultaneously. Revenue capped by hours — the framework is rebuilt or re-applied manually per client engagement.

## The manual loop (what they do today)
1. Client asks for research on a prospect before a call
2. Consultant manually researches: LinkedIn, web, news, tech stack
3. Applies their framework mentally, writes a brief
4. Delivers it hours or a day later
5. Repeat for every client, every request — no leverage on the methodology

## The agent loop

**External trigger**: Client @mentions the agent in their Slack workspace

**What the agent does autonomously** (using the consultant's proprietary methodology, stored in agent context):
1. Receives research request from a client team member
2. Pulls LinkedIn company profile + key contacts
3. Scrapes web for recent news, job postings, tech stack signals, funding events
4. Applies the consultant's scoring rubric and pain segmentation framework
5. Synthesizes a research brief: company overview, pain hypothesis, recommended approach and angle
6. Posts to the client's Slack thread

**Human-as-a-tool moment** (the client, not the consultant):
> "@ResearchBot research Acme Corp for our Q2 outbound"
>
> Bot posts research brief (2 min later)
>
> Client replies: "They also use Salesloft not Outreach. Focus on manufacturing vertical."
>
> Bot updates brief with corrections, re-posts.

Consultant is only looped in if the agent flags low confidence: "Unusual company type — need your judgment on vertical fit."

## Before / After
Before: Consultant charges $200/hr, works 30 billable hours/week, ceiling is $24k/month. Every client gets the same framework rebuilt manually.
After: Agent delivers briefs on-demand, 24/7, across 20+ clients. Consultant spends 1-2 hours/week reviewing quality and iterating the framework. Charges $1,500/month per client for "AI-powered research."

## To build this on DataGen

1. Add from marketplace: `/plugin marketplace add datagendev/datagen-plugin`
2. Install the plugin: `/plugin install datagen --scope project`
3. Restart Claude Code so it can load the plugin
4. Authenticate: `/datagen:setup` — signs in through browser, installs the CLI, creates project context
5. Connect tools: `/datagen:add-mcps` — add Slack, LinkedIn, Firecrawl or Perplexity, HubSpot or Close CRM
6. `/datagen:build-agent expert-research-agent` — agent triggered by Slack @mention in client workspaces; receives company name or research request from any client team member; pulls LinkedIn company data and key contacts, web research, recent news, job postings, and tech stack via Firecrawl/Perplexity; applies your proprietary scoring rubric and pain segmentation framework stored in agent context files; synthesizes research brief with pain hypothesis and recommended outreach angle; posts to client Slack thread; client can reply in-thread to correct or go deeper and agent resumes with full context
7. `/datagen:deploy-agent expert-research-agent` — trigger: Slack @mention, channel: BYOB Slack (agent appears as your branded bot in each client's workspace), subscribers: your Slack for run monitoring + each client Slack workspace
8. Set subscribers: configure one Slack destination per client; add each client's credentials via `datagen secrets set` so the agent uses their CRM and tools per-run with no cross-contamination

**First event**: Once connected to a client's Slack, any team member can type `@[YourBot] research [Company]` and get a brief in 2 minutes — any time of day.
