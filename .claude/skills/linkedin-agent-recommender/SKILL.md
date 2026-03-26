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

Use the subfile content to fill this structure. The format is designed for scannability — headers break up sections, a table replaces prose for before/after, a blockquote makes the Slack message feel real, and build instructions are a single code block.

```
# [agent-name] for [First Name] [Last Initial].

**Role:** [1 sentence — who they are, what the repeatable operational work is. End with the pain: time cost, bottleneck, or context-switching tax.]

---

## The shift: co-pilot → agent-as-driver

**Today:** [Trigger] → [Name] drives every step: [2-4 concrete manual steps from subfile].

**With the agent:** [Trigger] → agent autonomously [2-3 key autonomous actions] → [Name] gets a Slack message with the work already done. [Name] enters only for judgment: *"[One-line judgment question from subfile]"*

---

## What [agent-name] does

**Trigger:** [External trigger from subfile]

**Autonomous loop:**
[Numbered list from subfile — fill in their company name, tools, and context]

**Where [Name] enters — a Slack message like:**

> *[Line 1: what was detected / what happened]*
> *[Line 2: root cause or key finding]*
> *[Line 3: related context — past ticket, prior data, etc.]*
> *[Line 4: action taken — PR created, draft sent, ticket filed, etc.]*
>
> *[Judgment question — one line]*

[Name] replies in-thread: *"[Example reply that redirects the agent]"* → Agent updates its work accordingly.

---

## Before / After

| | Before | After |
|---|---|---|
| **Response time** | [Hours / delayed until Name has time] | [Minutes — agent runs immediately] |
| **[Name]'s effort** | [X hours per incident/task] | [Y minutes reviewing] |
| **Bottleneck** | [Solo person drives everything] | [Agent drives, human reviews] |

[Optional: **Why this works:** 1 sentence on key architectural reason — e.g., agent deployed from codebase with direct file access, agent uses their proprietary framework, etc.]

---

## Build this on DataGen

\```
/plugin marketplace add datagendev/datagen-plugin
/plugin install datagen --scope project
# restart Claude Code to load plugin
/datagen:setup
/datagen:add-mcps   # [specific MCPs from subfile]
/datagen:build-agent [agent-name]
/datagen:deploy-agent [agent-name]
\```

**First event:** [Specific first thing that lands in their inbox — make it concrete and grounded in the trigger.]
```

---

## Framing notes

- Always ground specifics in their actual profile: company name, tools they list, types of clients visible from experience
- If profile is sparse, ask: "What's the most repetitive thing you do every day that happens across multiple clients or accounts?"
- End every recommendation with the specific first thing that lands in their inbox the morning after setup
- Lead with the mental model shift before the feature list
