---
archetype: D
name: Outbound Specialist / Clay User / Lead Gen
agent: campaign-monitor
---

# Archetype D: Outbound Specialist

## Who they are
Growth, outbound, or lead generation specialist at a startup or agency. Runs multiple campaigns simultaneously via Instantly, HeyReach, or Clay. Monitors campaign performance manually. Classifies and responds to replies by hand.

## The manual loop (what they do today)
1. Log into Instantly or HeyReach to manually check campaign metrics
2. Notice a performance drop days after it started
3. Open each positive reply, manually decide on a response, write it
4. Miss patterns across campaigns because there's no cross-campaign view

## The agent loop

**External trigger 1 (daily monitoring)**: Cron at 8am

**What the agent does autonomously**:
1. Polls all active campaigns in Instantly or HeyReach
2. Compares reply rate, open rate, bounce rate vs prior 7-day baseline per campaign
3. Flags any campaign with >20% drop in reply rate
4. Reads last 20 negative replies from flagged campaigns to identify the pattern
5. Generates 3 alternative angle suggestions grounded in what the negatives actually say

**Human-as-a-tool moment** — Slack message at 8am:
> "Campaign health: 2 flagged.
> [Ramp list]: reply rate 4.2% → 1.8% this week. Top negative pattern (12/18): 'already have a solution for this.'
> 3 angle alternatives: [1] Lead with migration pain instead of net-new [2] Reference the Snowflake+Salesforce gap specifically [3] Target recent RevOps job posts as trigger
> [Use angle 1] [See full analysis] [Pause campaign]"

**External trigger 2 (reply classification)**: Webhook fires when new reply arrives in Instantly or HeyReach

**What the agent does autonomously**:
1. Reads full reply + thread history
2. Classifies: Interested / Objection / Not Now / Wrong Person / Unsubscribe
3. For Interested: drafts a response and proposed next step
4. For Objection: drafts a reframe with supporting evidence

**Human-as-a-tool moment** — Slack message:
> "New reply from Sarah Chen, Ramp: Interested.
> 'This actually sounds relevant — can you send more info?'
> Suggested response: [draft]. Suggested next step: Book a 15-min walkthrough.
> [Send response] [Edit first] [Handle manually]"

## Before / After
Before: Campaign drops go unnoticed for days. Every reply handled manually with no context on the full thread.
After: Performance issues surface the same morning they start. Interested replies have a drafted response ready before you open the inbox.

## To build this on DataGen

1. Add from marketplace: `/plugin marketplace add datagendev/datagen-plugin`
2. Install the plugin: `/plugin install datagen --scope project`
3. Restart Claude Code so it can load the plugin
4. Authenticate: `/datagen:setup` — signs in through browser, installs the CLI, creates project context
5. Connect tools: `/datagen:add-mcps` — add Instantly or HeyReach, Slack, HubSpot or Salesforce (optional)
6. `/datagen:build-agent campaign-monitor` — agent with two triggers: (1) daily cron at 8am: polls all active Instantly/HeyReach campaigns, compares reply/open/bounce rates vs prior 7-day baseline, flags any campaign with >20% performance drop, reads last 20 negative replies to identify the pattern, generates 3 alternative angle suggestions, posts flagged campaigns + angle options to Slack; (2) Instantly or HeyReach reply webhook: reads full reply and thread history, classifies as Interested/Objection/Not Now/Wrong Person/Unsubscribe, drafts suggested response for Interested and Objection, posts classification + draft to Slack with send/edit/handle-manually buttons
7. `/datagen:deploy-agent campaign-monitor` — trigger: daily cron at 8am + Instantly/HeyReach reply webhook, channel: Slack, subscribers: you
8. Set subscribers: your Slack handle; reply in thread to send the drafted response or trigger a campaign angle swap

**First event**: Tomorrow morning at 8am you'll get a campaign health check in Slack — any underperforming campaigns flagged with angle alternatives ready.
