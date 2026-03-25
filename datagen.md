# What Is DataGen

**The easiest way to deploy your Claude Code agent.**

You build the agent in Claude Code. DataGen handles the deploy, the tools, and the infra.

- No Agent SDK needed
- MCP tools pre-connected
- Webhook + cron built-in

---

## How It Works

Go from Claude Code to a running agent in 4 steps:

1. **Create your agent with Claude Code** -- Build your Claude Code agent the way you already do, in your terminal or IDE. Push it to a GitHub repo. No special format required.

2. **Add the DataGen GitHub App** -- Install the GitHub App on your repo in one click. No config files, no SDK dependencies to add.

3. **DataGen auto-discovers your agent** -- DataGen scans your repo and detects your agent file automatically. Select your API keys and MCP tools, all pre-configured.

4. **One-click deploy with webhook or schedule** -- Click deploy. Get a webhook URL instantly, set a cron schedule with the visual scheduler, or run on-demand from the dashboard.

---

## MCP Tool Management

**Configuring tools for a remote agent is hard. DataGen handles it.**

Connect MCP tools in one click, build custom tools when you need them. Whatever works locally carries over to production -- zero reconfiguration.

### Key capabilities

- **One-click MCP connections** -- Browse 50+ pre-built MCP servers. Click connect, authorize via OAuth, done. No transport config, no API key wiring.
- **Auth management built-in** -- OAuth token refresh, API key storage, credential rotation -- all handled automatically. Your deployed agent never loses access.
- **Use any MCP you want** -- Local stdio servers or remote HTTP endpoints -- bring whatever MCP servers you already use. DataGen routes to both seamlessly.
- **Build custom tools** -- When standard MCPs aren't enough, compose MCP calls with Python in a secure sandbox. Deploy as reusable tools your agent calls automatically.
- **Built-in tool discovery** -- AI-powered search across all your connected tools. Your agent always finds the right tool at the right time.
- **Local-to-production parity** -- Tools you connect and use locally in Claude Code are automatically available when you deploy. Configure once -- no re-wiring, no re-auth.

### Architecture

```
Claude Code
    |
DataGen MCP (Unified Auth, Transport & Discovery)
  - AddMCP
  - SearchTool
  - ExecuteTool
  - ExecuteCode
  - DeployTool
    |
External Tools (GitHub, Slack, Supabase, HubSpot, Snowflake, Custom Tools)
```

---

## Deploy & Build Tools: Composing MCP Beyond Basic

Build custom tools by composing multiple MCPs, databases, and APIs. Deploy to your Custom Tool Library from a prompt, ready for production use.

- **Tool Building** -- Compose MCP tools with SQL, databases, and external services. Build sophisticated workflows that go beyond basic MCP capabilities.
- **DeployTool** -- Deploy to your Custom Tool Library from a prompt. Transform composed workflows into production-ready services.
- **Compose Beyond Basic** -- Create advanced tools by combining multiple MCPs and services. Move from basic MCP to enterprise-grade custom tools.

---

## Communication Channels

**Talk to your agents where you already work.**

Agents don't just run in the background -- they communicate back through Slack and email, and your team can trigger them with a simple @mention or reply.

- **@Datagen-Agent in Slack** -- Trigger any agent from a channel, get results in-thread.
- **Email notifications** -- Rich completion reports with status, output, and log links.
- **Reply-to-resume** -- Reply in Slack or email and the agent picks up where it left off.
- **Granular access control** -- Owners trigger, viewers get notified, no DataGen account required.

---

## Example Use Cases

### Operation: Email Workflow Automation
> "Fetch emails from Google, extract key info, read rate card from Sheets, match quote, generate reply, update Notion tracker"
>
> MCPs used: Gmail, Google Sheets, Notion

### Productivity: Weekly Linear Summary
> "Summarize all Linear tasks I completed this past week and post the summary to Slack"
>
> MCPs used: Linear, Slack

### Sales: Lead Qualification Engine
> "Take an email input, research the prospect's profile, evaluate fit criteria, and save qualified leads to my Neon database"
>
> MCPs used: Neon, Perplexity

---

## When DataGen Is the Right Fit

### Best for you if...
- You already built an agent in Claude Code and now need it running 24/7, not only from your local terminal.
- You want your agent triggered by external systems (webhooks, APIs, cron schedules), not manual prompts.
- You are a lean founder, early-stage team, or ops manager who wants automation without managing complex infra.

### You may prefer generic infra if...
- You only need generic background jobs with no agent workflow requirements.
- You want to own every infrastructure detail end-to-end from day one.
- You are not using Claude Code and do not need MCP tooling.

### What you get with DataGen
- Webhook endpoint and scheduler ready at deploy time.
- Built-in MCP auth, tool discovery, and execution workflow.
- A single dashboard to run, observe, and iterate on agents.

---

## Why DataGen Over Alternatives

- **Early Access: 500 Free Credits** -- Join now and get 500 credits to deploy and run agents. Limited time offer for alpha users.
- **No Complex SDK Setup Required** -- While Modal and Trigger.dev need custom Agent SDK code, DataGen deploys from your existing repo.
- **Webhook URL in One Click** -- Get a production webhook URL the moment you deploy. No infra tickets needed.

---

## Pricing

Pay per execution, not per seat. Only charged when your agents and tools run.

| Plan | Price | Includes |
|------|-------|----------|
| **Free** | $0 | 500 free credits (one-time), Discord community support |
| **Basic** | $30/mo | 5,000 credits/month, 1 credit per MCP tool call, 2 credits per Premium tool call, Discord community support |
| **Pro** | $100/mo | 50,000 credits/month, 1 credit per MCP tool call, 2 credits per Premium tool call, priority customer support |
| **Enterprise** | Contact Sales | Custom credit pricing, custom Claude Code setup, white-glove onboarding, dedicated founder support |

Credits reset monthly. Unused credits expire.

---

## Getting Started

### Native Install
```bash
curl -fsSL https://cli.datagen.dev/install.sh | sh
datagen login
datagen mcp
```

### Add Through Claude Code
```bash
claude mcp add --transport http datagen https://mcp.datagen.dev/mcp --header "x-api-key: YOUR_API_KEY"
```

---

## Links

- Schedule a demo: https://cal.com/yusheng/datagen-on-board-1-1
- Enterprise inquiries: founders@datagen.dev
