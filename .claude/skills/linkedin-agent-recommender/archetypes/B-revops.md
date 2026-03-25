---
archetype: B
name: RevOps / GTM Engineer
agent: lead-router
---

# Archetype B: RevOps / GTM Engineer

## Who they are
RevOps lead, Revenue Operations manager, or GTM Engineer at a B2B SaaS company. Owns lead routing, CRM hygiene, pipeline data quality. Tools include Salesforce or HubSpot, Clay, Snowflake. Currently doing most routing and hygiene work manually or via brittle Zapier flows.

## The manual loop (what they do today)
1. New inbound lead arrives — noticed eventually
2. Manually enrich: company size, industry, tech stack
3. Look up routing rules mentally or in a spreadsheet
4. Assign in CRM, notify the rep
5. CRM hygiene: weekly or monthly manual cleanup — duplicates, missing fields, stale deals

## The agent loop

**External trigger 1 (real-time)**: Webhook fires on every new CRM lead creation or form submission

**What the agent does autonomously**:
1. Enriches the lead: company size, tech stack, funding stage, ICP fit score
2. Deduplicates against existing records
3. Applies routing logic: territory, vertical, rep capacity rules
4. Assigns in CRM
5. Drafts rep notification with full context

**Human-as-a-tool moment** — Slack message to RevOps:
> "New inbound: Sarah Chen, Head of RevOps, Ramp (800 employees, Salesforce + Snowflake).
> ICP score: 9/10. Tech stack: data silo pattern — high intent signal.
> Routed to Alex (East Coast, SaaS vertical). CRM updated.
> [Override routing] [Flag for review]"

Most runs need zero human input. The message is a receipt, not a request. Human only enters when confidence is low.

**External trigger 2 (nightly hygiene)**: Cron at 11pm

Agent scans CRM for duplicates, missing required fields, stale deals past expected close date, wrong owners → posts morning report:
> "Hygiene run: 17 issues found (3 duplicates, 8 missing fields, 6 stale deals).
> [Approve all fixes] [Review list]"

## Before / After
Before: Leads sit unrouted for hours. Hygiene runs quarterly if someone has time.
After: Every lead routed within 60 seconds. CRM issues resolved overnight.

## To build this on DataGen

1. Add from marketplace: `/plugin marketplace add datagendev/datagen-plugin`
2. Install the plugin: `/plugin install datagen --scope project`
3. Restart Claude Code so it can load the plugin
4. Authenticate: `/datagen:setup` — signs in through browser, installs the CLI, creates project context
5. Connect tools: `/datagen:add-mcps` — add HubSpot or Salesforce, Slack, Apollo or PDL (enrichment)
6. `/datagen:build-agent lead-router` — agent with two triggers: (1) CRM webhook on every new lead: enriches company size, tech stack, and ICP fit score via Apollo/PDL, deduplicates against existing records, applies territory and vertical routing rules, assigns in CRM, posts routing receipt to Slack with confidence score and override option; (2) nightly cron at 11pm: scans CRM for duplicate records, missing required fields, stale deals, and mismatched ownership, posts hygiene report to Slack with one-click approve-all
7. `/datagen:deploy-agent lead-router` — trigger: CRM webhook (real-time) + nightly cron at 11pm, channel: Slack, subscribers: RevOps lead
8. Set subscribers: RevOps lead gets all routing receipts and hygiene reports; add sales rep Slack handles so they receive a DM when a lead is routed to them

**First event**: The next inbound lead that hits your CRM will be enriched, scored, assigned, and confirmed in Slack within 60 seconds.
