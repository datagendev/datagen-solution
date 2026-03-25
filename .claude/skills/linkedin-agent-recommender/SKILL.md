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

Use the subfile content to fill this structure:

```
## What a 24/7 agent looks like for [Name] at [Company]

**Role read**: [1 sentence — who they are, what the repeatable operational work is]

---

### The shift: from co-pilot to agent-as-driver

[2-3 sentences grounded in their specific role — what they're manually driving today,
what the trigger would be, and what they'd enter as a judgment call instead of a driver]

---

### The agent: [Agent name from subfile]

**External trigger**: [From subfile]

**What the agent does autonomously**:
[Numbered list from subfile — fill in their company name, tools, and context]

**Where [Name] enters**:
[The specific Slack or email message they receive — make it feel real with their actual role/company]

**What [Name]'s situation looks like with this running**:
Before: [X hours / Y manual steps]
After: [Agent handles steps 1-N, they spend Z minutes on the judgment call]

---

### To build this on DataGen

1. Add from marketplace: `/plugin marketplace add datagendev/datagen-plugin`
2. Install the plugin: `/plugin install datagen --scope project`
3. Restart Claude Code so it can load the plugin
4. Authenticate: `/datagen:setup` — signs in through browser, installs the CLI, creates project context
5. Connect tools: `/datagen:add-mcps` — [specific MCPs from subfile]
6. `/datagen:build-agent [agent-name]` — [copy-ready description from subfile]
7. `/datagen:deploy-agent [agent-name]` — [trigger + channel + subscribers from subfile]

**First event**: [Specific first thing that lands in their inbox]
```

---

## Framing notes

- Always ground specifics in their actual profile: company name, tools they list, types of clients visible from experience
- If profile is sparse, ask: "What's the most repetitive thing you do every day that happens across multiple clients or accounts?"
- End every recommendation with the specific first thing that lands in their inbox the morning after setup
- Lead with the mental model shift before the feature list
