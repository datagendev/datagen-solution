---
archetype: G
name: Engineer / Technical Founder
agent: debug-agent
---

# Archetype G: Engineer / Technical Founder

## Who they are
Engineer, CTO, or technical founder. Solo bottleneck for errors — Sentry fires, they context-switch, pull details, create a ticket, analyze, fix, push PR. 1-2 hours per incident.

## The manual loop
1. Sentry notification arrives (often seen late)
2. Context-switch to terminal, pull error details
3. Create a Linear ticket
4. Analyze root cause, write fix, create PR

## The agent loop

**Trigger**: Sentry webhook on new error

**Autonomous steps**:
1. Receives error details + stack trace
2. Queries Linear for related past tickets
3. Creates a Linear ticket if none exists
4. Inspects source code to trace root cause — the actual code path, not just the error message
5. Creates a GitHub PR with fix + explanation
6. Sends root cause analysis + PR link via Slack and email

**Slack message**:
> *Error: NullPointerException in payment processor, line 247.*
> *Root cause: Token expiry not handled in retry logic.*
> *Related: LIN-234 (same module, fixed Dec — regression).*
> *PR #891 created: [diff link]*
>
> *Does this fix look right, or should I take a different approach?*

Reply example: "Check auth.js line 88 first." → Agent revises the PR.

## Before / After

| | Before | After |
|---|---|---|
| **Response time** | Hours | Minutes |
| **Effort** | 1-2 hours per incident | 2 min reviewing a PR |
| **Bottleneck** | Solo engineer drives everything | Agent drives, human reviews |

**Why this works**: Agent deployed from the codebase with direct file access — root cause analysis is grounded in real code, not surface-level errors.

## Build instructions

**MCPs**: Sentry, Linear, GitHub, Slack

**build-agent description**: agent triggered by Sentry webhook; queries Linear for context; creates ticket; inspects source files to trace root cause; creates GitHub PR with fix; fans out analysis + PR link via Slack and email; subscribers reply to redirect the agent

**deploy-agent config**: trigger: Sentry webhook, channel: Slack + email, subscribers: engineering team

**First event**: Next Sentry error arrives in Slack as a root cause analysis with a PR already created.
