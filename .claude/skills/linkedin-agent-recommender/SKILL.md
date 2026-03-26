---
name: linkedin-agent-recommender
description: Given a LinkedIn profile URL, fetch the person's role and context, classify them into an archetype, and output a copy-ready DataGen agent recommendation — a 24/7 deployed agent that communicates via Slack and email, grounded in the human-as-a-tool paradigm.
---

# LinkedIn Agent Recommender

Parses a LinkedIn profile and recommends one specific DataGen deployed agent tailored to that person's daily operational work — with copy-ready build and deploy instructions.

**Core framing**: Most people still use AI as a co-pilot. They open a terminal, start the task, AI helps finish it. Efficiency gain, but they're still driving. The real shift is **human-as-a-tool**: an external event triggers the agent, it does the intelligent work autonomously, and reaches the human only for a judgment call — the same way it calls any API. DataGen makes this possible with three primitives: (1) one-click deployment to a live webhook or cron, (2) MCP gateway for tool portability across local and cloud, (3) built-in Slack and email channels with granular subscriber control.

---

## When to invoke

- User shares a LinkedIn URL and asks what DataGen agent would help them
- Used during outbound or onboarding to personalize the DataGen pitch to someone's specific role

## Input

- **linkedin_url**: LinkedIn profile URL

---

## Step 1: Fetch the profile

Call `get_linkedin_person_data` with `linkedin_url`.

Extract: `headline`, `currentPosition.title`, `currentPosition.companyName`, `summary`, `positions` (recent 3), `skills`.

If unavailable, ask: "What's your role and what's the most repetitive part of your day?"

---

## Step 2: Classify archetype

| # | Archetype | Key signals | Subfile |
|---|-----------|-------------|---------|
| A | Agency Owner | "founder/owner" at agency; bio mentions multiple clients, Clay, HeyReach, Instantly | @archetypes/A-agency-owner.md |
| B | RevOps / GTM Engineer | "RevOps", "Revenue Operations", "GTM Engineer"; Salesforce, HubSpot, Clay, Snowflake | @archetypes/B-revops.md |
| C | Sales Rep / AE | "Account Executive", "BDR", "Account Manager"; mid-to-large company | @archetypes/C-sales-rep.md |
| D | Outbound Specialist | "growth", "outbound", "lead gen"; Clay, Apollo, Instantly, HeyReach | @archetypes/D-outbound-specialist.md |
| E | Consultant / Fractional | "fractional", "consultant", "advisor"; works with multiple clients | @archetypes/E-consultant.md |
| F | Founder / Head of Growth | "Founder", "CEO", "Head of Growth"; early-stage, small team | @archetypes/F-founder.md |
| G | Engineer / Technical Founder | "Engineer", "CTO", "Developer"; Sentry, Linear, GitHub | @archetypes/G-engineer.md |

Read the matching subfile for the full agent details and copy-ready output.

If the person fits multiple archetypes, recommend the primary one and mention the secondary as an add-on.

---

## Step 3: Generate the recommendation

Use the subfile content to fill this structure. **The final output must be under 35 lines of rendered text.** Every sentence must add new information — cut anything redundant. No filler, no restating what was already said.

```
# [agent-name] for [First Name] [Last Initial].

**Role:** [1 sentence: who they are + the pain.]

---

## The shift: co-pilot → agent-as-driver

**Today:** [Trigger] → [Name] drives every step: [list manual steps].

**With the agent:** [Trigger] → agent [key actions] → [Name] gets a Slack message with work done. Enters only for judgment: *"[question]"*

---

## What [agent-name] does

**Trigger:** [from subfile]

**Autonomous loop:**
[Numbered list — use their company/tools/context. Max 6 steps.]

**Where [Name] enters — a Slack message like:**

> *[4 lines max: what happened, finding, context, action taken]*
>
> *[Judgment question]*

[Name] replies in-thread → agent updates accordingly.

---

## Before / After

| | Before | After |
|---|---|---|
| **Response time** | [Hours] | [Minutes] |
| **Effort** | [X hours] | [Y min reviewing] |
| **Bottleneck** | [Solo driver] | [Agent drives, human reviews] |

**Why this works:** [1 sentence.]

---

## Build this on DataGen

\```
/plugin marketplace add datagendev/datagen-plugin
/plugin install datagen --scope project
# restart Claude Code to load plugin
/datagen:setup
/datagen:add-mcps   # [MCPs]
/datagen:build-agent [agent-name]
/datagen:deploy-agent [agent-name]
\```

**First event:** [1 sentence.]
```

---

## Framing notes

- Ground in their actual profile: company, tools, client types
- If profile is sparse, ask: "What's the most repetitive thing you do every day?"
- End with the concrete first event
- Lead with mental model shift, not features
- **Under 35 lines rendered. Every sentence must add new information. No filler.**
