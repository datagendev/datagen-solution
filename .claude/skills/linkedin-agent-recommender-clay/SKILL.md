---
name: linkedin-agent-recommender-clay
description: Run the LinkedIn Agent Recommender for a profile, then POST the structured recommendation to a Clay webhook URL so it lands as a row in a Clay table.
---

# LinkedIn Agent Recommender → Clay

Runs the full linkedin-agent-recommender flow, then sends structured results to a Clay table via webhook.

---

## When to invoke

- User provides a LinkedIn URL **and** a Clay webhook URL
- Batch enrichment: pushing agent recommendations into a Clay table for outbound sequences

## Input

- **linkedin_url**: LinkedIn profile URL
- **clay_webhook_url**: Clay webhook URL (e.g. `https://app.clay.com/api/v1/webhooks/...`)

---

## Step 1: Run the recommender

Execute the full linkedin-agent-recommender flow (Steps 1-3 from @../linkedin-agent-recommender/SKILL.md):

1. Fetch the profile via `get_linkedin_person_data`
2. Classify into archetype (A-G)
3. Generate the recommendation using the matching archetype subfile

While generating the recommendation, also capture these structured fields for the Clay payload:

- `linkedin_url` — input URL
- `first_name` — from profile
- `last_name` — from profile
- `headline` — from profile
- `company` — currentPosition.companyName
- `title` — currentPosition.title
- `archetype` — letter + label (e.g. "A - Agency Owner")
- `agent_name` — from archetype subfile (e.g. "call-debrief-agent")
- `recommendation_markdown` — the full rendered recommendation output from Step 3
- `one_liner` — the Role line (1 sentence: who they are + the pain)
- `first_event` — the "First event" sentence from the recommendation

---

## Step 2: Display the recommendation

Show the full recommendation to the user exactly as linkedin-agent-recommender would — same format, same output rules.

---

## Step 3: POST to Clay webhook

Send a POST request to `clay_webhook_url` with the structured payload:

```bash
curl -s -X POST "$clay_webhook_url" \
  -H "Content-Type: application/json" \
  -d '{
    "linkedin_url": "...",
    "first_name": "...",
    "last_name": "...",
    "headline": "...",
    "company": "...",
    "title": "...",
    "archetype": "...",
    "agent_name": "...",
    "recommendation_markdown": "...",
    "one_liner": "...",
    "first_event": "..."
  }'
```

After the POST:
- If success (2xx): confirm "Sent to Clay" with the row data summary.
- If failure: show the status code and error body so the user can debug the webhook.

---

## Notes

- The recommendation output shown to the user is identical to /linkedin-agent-recommender — this skill only adds the Clay POST step.
- The `recommendation_markdown` field contains the full rendered output so Clay columns can reference it or an AI action can parse sections from it.
- If the Clay webhook URL is missing, ask for it before proceeding. If the LinkedIn URL is missing, ask for it.
