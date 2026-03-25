---
archetype: A
name: Agency Owner / GTM Agency Founder
agent: call-debrief-agent
---

# Archetype A: Agency Owner

## Who they are
Founder or owner of a GTM, sales, outbound, or marketing agency. Manages 5-10+ active client relationships simultaneously. Uses tools like Clay, HeyReach, Instantly, Fireflies. Spends 2-3 hours/day on manual call prep and debrief across all clients.

## The manual loop (what they do today)
1. Call ends with a client
2. Hours later: pull the Fireflies transcript
3. Manually update CRM with follow-up actions, deal stage, notes
4. Manually create ClickUp tasks from commitments made on the call
5. Manually draft and send follow-up email to client
6. Morning of next call: 30-45 min pulling notes, past transcripts, open tasks to prep

## The agent loop

**External trigger**: Fireflies webhook fires when a call transcript is ready

**What the agent does autonomously**:
1. Reads the full transcript
2. Cross-references open CRM tasks and commitments made on prior calls — flags anything overdue
3. Extracts: deal stage change, new commitments, follow-up actions, open questions
4. Drafts the follow-up email to the client
5. Proposes specific ClickUp task updates (create / close / modify)
6. Packages debrief message with all proposed actions attached — nothing executes until approved

**Human-as-a-tool moment** — Slack message arrives within 15 minutes of call ending:
> "Debrief: Acme Corp call (42 min). Deal stage: Negotiation → Proposal sent.
> You committed to sending a revised SOW by Friday.
> 3 ClickUp tasks proposed (2 new, 1 closed).
> Follow-up email drafted — ready to send.
> [Approve all] [Review each] [Edit email draft]"

Owner clicks approve. Agent pushes CRM update, creates tasks, sends email.

**Pre-call trigger** (Google Calendar cron, T-60min before any client meeting):
Agent pulls last 3 transcripts for that client, open CRM tasks, recent emails → posts prep brief to Slack:
> "Your 10am with Acme is in 1 hour. You committed to a revised SOW — it's overdue.
> Open tasks: ACME-14 (SOW), ACME-15 (tech call scheduling).
> Suggested agenda: [1] SOW status [2] Technical walkthrough scheduling [3] Nail next steps.
> [Add context] [Looks good]"
Owner can reply in-thread to refine — agent resumes with corrections.

## Before / After
Before: 2-3 hours/day across 5+ clients on manual prep, debrief, CRM updates, task creation, email drafts.
After: Agent handles all of it. Owner spends ~10 minutes reviewing and approving actions across all clients.

## To build this on DataGen

1. Add from marketplace: `/plugin marketplace add datagendev/datagen-plugin`
2. Install the plugin: `/plugin install datagen --scope project`
3. Restart Claude Code so it can load the plugin
4. Authenticate: `/datagen:setup` — signs in through browser, installs the CLI, creates project context
5. Connect tools: `/datagen:add-mcps` — add Fireflies, Google Calendar, HubSpot or Close CRM, ClickUp or Linear, Slack, Gmail
6. `/datagen:build-agent call-debrief-agent` — agent with two triggers: (1) Fireflies webhook on transcript ready: reads transcript, cross-references open CRM tasks and prior commitments, extracts deal stage changes and new follow-up actions, drafts client follow-up email, proposes ClickUp task updates, posts full debrief to Slack with approve/reject buttons before executing anything; (2) Google Calendar cron 60 minutes before any client meeting: pulls last 3 Fireflies transcripts for that client, reads open tasks and overdue commitments, synthesizes prep brief with suggested agenda, posts to Slack for review
7. `/datagen:deploy-agent call-debrief-agent` — trigger: Fireflies webhook + Google Calendar cron (T-60min), channel: Slack + Gmail, subscribers: you

**First event**: The next call that ends will have a Slack debrief waiting within 15 minutes — deal stage, proposed tasks, and follow-up email draft ready to send in one click.
