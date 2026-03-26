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

**Human-as-a-tool moment** — Slack message example:
> *Error: NullPointerException in payment processor, line 247.*
> *Root cause: Token expiry not handled in retry logic.*
> *Related: LIN-234 (same module, fixed in Dec — regression).*
> *PR #891 created: [diff link]*
>
> *Does this fix look right, or should I take a different approach?*

Any subscriber replies: "The token refresh is actually handled in auth.js line 88, check that first." Agent reads the reply, updates its analysis, revises the PR.

## Before / After

| | Before | After |
|---|---|---|
| **Response time** | Hours (whenever someone can context-switch) | Minutes (agent runs immediately) |
| **Engineer's effort** | 1-2 hours per incident | 2 minutes reviewing a PR |
| **Bottleneck** | Solo engineer drives everything | Agent drives, human reviews |

**Why this works**: The agent is deployed *from* the codebase. It has direct access to the source files — not an API call to a black box. Root cause analysis is grounded in real code paths, not surface-level error messages.

## To build this on DataGen

```
/plugin marketplace add datagendev/datagen-plugin
/plugin install datagen --scope project
# restart Claude Code to load plugin
/datagen:setup
/datagen:add-mcps   # Sentry, Linear, GitHub, Slack
/datagen:build-agent debug-agent
/datagen:deploy-agent debug-agent
```

**Subscribers**: Add every engineer's Slack handle and email — any team member can reply to unblock the agent. Use `datagen agents config debug-agent --add-recipient` to add each person.

**First event**: The next Sentry error that fires will arrive in Slack as a root cause analysis with a PR already created — not just a mystery notification requiring hours to resolve.
