---
archetype: G
name: Engineer / Technical Founder
agent: debug-agent
---

# Archetype G: Engineer / Technical Founder

## Who they are
Software engineer, CTO, or technical founder. Uses Sentry or PagerDuty for error tracking, Linear or Jira for tickets, GitHub for code. Currently the bottleneck: errors require one specific person to notice them, context-switch to a terminal, pull Sentry info, create a ticket, and run analysis.

## The manual loop (what they do today)
1. Sentry notification arrives (often ignored or seen late)
2. Find time to get back to a terminal
3. Pull full Sentry error context manually
4. Create a Linear ticket
5. Open Claude Code, load context, analyze the error, propose a fix, create a PR
Total: 1-2 hours per incident. One person bottleneck. Only happens when someone has time.

## The agent loop

**External trigger**: Sentry webhook fires on every new error

**What the agent does autonomously** (the agent lives inside the codebase — it reads files directly):
1. Receives webhook with full error details and stack trace
2. Queries Linear for related past tickets and prior context on the affected module
3. Creates a new Linear ticket if one doesn't exist
4. Inspects the relevant source code files to trace root cause — not just the error message, the actual code path
5. Proposes a specific fix
6. Creates a GitHub PR with the fix and explanation
7. Fans out root cause analysis + PR link to all subscribers via Slack and email

**Human-as-a-tool moment** — Slack message + email to all subscribers:
> "Error detected: NullPointerException in payment processor, line 247.
> Root cause: Token expiry not handled in retry logic.
> Related ticket: LIN-234 (same module, fixed in Dec — regression).
> PR created: #891 — diff: [link]
> Does this fix look right, or should I take a different approach?
> Any team member can reply."

Any subscriber replies: "The token refresh is actually handled in auth.js line 88, check that first." Agent reads the reply, updates its analysis, revises the PR.

## Before / After
Before: 1-2 hours per incident. One person bottleneck. Errors sit unresolved until someone has time.
After: Within minutes of the error firing, a Slack message arrives with root cause analysis and a PR already created. Any team member can unblock the agent with a reply — not just the one who first noticed it.

**Key reason this works**: The agent is deployed *from* the codebase. It has direct access to the source files — not an API call to a black box. That's why the root cause analysis is meaningful, not surface-level.

## To build this on DataGen

1. Add from marketplace: `/plugin marketplace add datagendev/datagen-plugin`
2. Install the plugin: `/plugin install datagen --scope project`
3. Restart Claude Code so it can load the plugin
4. Authenticate: `/datagen:setup` — signs in through browser, installs the CLI, creates project context
5. Connect tools: `/datagen:add-mcps` — add Sentry, Linear, GitHub, Slack
6. `/datagen:build-agent debug-agent` — agent triggered by Sentry webhook on new error; queries Linear for related past tickets and prior context on the affected module; creates a new Linear ticket if one doesn't exist; inspects the relevant source code files directly to trace the root cause through the actual code path (not just the error message); proposes a specific fix; creates a GitHub PR with the fix and a plain-language explanation; fans out the root cause analysis, PR link, and a single question to all subscribers via Slack and email; any subscriber can reply in the Slack thread or email to suggest a better approach and the agent updates the PR
7. `/datagen:deploy-agent debug-agent` — trigger: Sentry webhook, channel: Slack + email, subscribers: full engineering team
8. Set subscribers: add every engineer's Slack handle and email — any team member can reply to unblock the agent, not just the person who first saw the error; use `datagen agents config debug-agent --add-recipient` to add each person

**First event**: The next Sentry error that fires will arrive in your Slack as a root cause analysis with a PR already created — not just a mystery notification.
