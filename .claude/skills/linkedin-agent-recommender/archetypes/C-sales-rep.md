---
archetype: C
name: Sales Rep / AE / BDR
agent: call-prep-agent
---

# Archetype C: Sales Rep / AE

## Who they are
Account Executive, BDR, or Account Manager at a mid-to-large B2B company. Runs 3-8 prospect or client calls per day. Spends 30-60 min prepping for each discovery call manually. CRM updates get delayed or skipped after calls.

## The manual loop (what they do today)
Pre-call: Pull last call notes, check emails, look up open tasks, write an agenda — 30-60 min per call
Post-call: Update CRM with deal stage, commitments, next steps — often delayed 1-2 days or skipped

## The agent loop

**External trigger 1 (pre-call)**: Google Calendar cron — 60 minutes before any meeting tagged as prospect or client

**What the agent does autonomously**:
1. Reads the calendar event: who, what company, what type of call
2. Pulls last 3 Fireflies transcripts for that account
3. Reads open CRM tasks, recent emails, and logged commitments
4. Identifies: what you promised last call and haven't delivered, deal blockers, relationship context
5. Synthesizes prep brief with suggested agenda prioritized by risk

**Human-as-a-tool moment** — Slack message 60 min before the call:
> "Your 10am with Acme Corp is in 1 hour.
> Last call (Feb 28): they flagged integration complexity as a concern. You committed to sending a technical overview — not done yet.
> Open tasks: ACME-14 (SOW, overdue), ACME-15 (schedule tech call, open).
> Suggested agenda: [1] Address SOW status [2] Technical overview [3] Nail next steps.
> [Add context] [Looks good]"

Rep can reply in-thread to add context — agent refines and reposts.

**External trigger 2 (post-call)**: Fireflies webhook fires when transcript is ready

**What the agent does autonomously**:
1. Reads the transcript
2. Extracts: deal stage change, new commitments made, follow-up items, next meeting agreed
3. Updates CRM fields
4. Drafts follow-up email
5. Creates or updates tasks

**Human-as-a-tool moment** — Slack message within 15 min of call ending:
> "Call debrief: Acme Corp. Deal moved to Proposal. You committed to SOW by Friday.
> CRM updated. Follow-up email drafted.
> [Send email now] [Edit first] [Skip]"

## Before / After
Before: 45 min prep per call, 1 hour of delayed CRM work per day, commitments missed because no one tracked them.
After: Prep brief arrives automatically. Post-call update takes 30 seconds to approve.

## To build this on DataGen

1. Add from marketplace: `/plugin marketplace add datagendev/datagen-plugin`
2. Install the plugin: `/plugin install datagen --scope project`
3. Restart Claude Code so it can load the plugin
4. Authenticate: `/datagen:setup` — signs in through browser, installs the CLI, creates project context
5. Connect tools: `/datagen:add-mcps` — add Google Calendar, Fireflies, HubSpot or Salesforce, Slack, Gmail
6. `/datagen:build-agent call-prep-agent` — agent with two triggers: (1) Google Calendar cron 60 minutes before any prospect or client meeting: pulls last 3 Fireflies transcripts for that account, reads open CRM tasks and overdue commitments, synthesizes a prep brief with suggested agenda prioritized by risk, posts to Slack — rep can reply in-thread to add context and agent refines; (2) Fireflies webhook when transcript ready: extracts deal stage change, new commitments, follow-up items, next meeting, updates CRM, drafts follow-up email, posts debrief to Slack with send/edit/skip buttons
7. `/datagen:deploy-agent call-prep-agent` — trigger: Google Calendar cron (T-60min) + Fireflies webhook, channel: Slack + Gmail, subscribers: you
8. Set subscribers: your Slack handle and email; reply in Slack thread to correct the agent before it pushes CRM updates

**First event**: Your next scheduled prospect call will have a prep brief in Slack 60 minutes before it starts.
