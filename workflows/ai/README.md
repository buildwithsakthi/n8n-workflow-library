# 🤖 AI Automation Workflows

Production-ready n8n workflows powered by OpenAI and Claude for business automation.

---

## Workflows

### 1. [`ai-email-draft-generator.json`](./ai-email-draft-generator.json)

Watches your Gmail inbox and auto-generates a professional draft reply using GPT-4o-mini. The draft is saved — not sent — so you stay in control.

**Flow:** Gmail Trigger → Build Prompt → OpenAI API → Extract Reply → Save as Gmail Draft

**Credentials needed:**
- Gmail OAuth2
- OpenAI API Key (set as HTTP Header Auth with `Authorization: Bearer sk-...`)

**Key customisation points:**
- Change the `model` in the HTTP Request node (`gpt-4o`, `gpt-4o-mini`, etc.)
- Edit the system prompt in the `Build Prompt` Code node to match your tone
- Set the Gmail trigger filter to only watch specific labels

---

### 2. [`ai-lead-qualification-agent.json`](./ai-lead-qualification-agent.json)

Scores inbound leads from 0–100 using Claude, routes hot leads to a priority Slack channel, and logs all leads to Airtable.

**Flow:** Webhook → Parse Lead → Claude API → Extract Score → IF (hot?) → Airtable + Slack

**Credentials needed:**
- Anthropic API Key (as HTTP Header Auth: `x-api-key: sk-ant-...`)
- Airtable Personal Access Token
- Slack Bot Token

**Airtable setup:**
Create a base called `Leads` with columns: `Name`, `Email`, `Company`, `Role`, `Score`, `Tier`, `Reasoning`, `Suggested Action`, `Status`, `Created At`

**Webhook URL format:**
`https://your-n8n-instance.com/webhook/qualify-lead`

---

### 3. [`ai-content-repurposing.json`](./ai-content-repurposing.json)

Takes a blog post URL via an n8n Form, fetches the content, and generates a tweet, LinkedIn post, and email newsletter section using GPT-4o. All output is logged to Google Sheets.

**Flow:** Form Trigger → Fetch Blog HTML → Clean + Build Prompt → OpenAI → Split Content Types → Google Sheets

**Credentials needed:**
- OpenAI API Key
- Google Sheets OAuth2

**Google Sheets setup:**
Create a sheet called `Content Log` with columns: `Type`, `Content`, `Subject (Email Only)`, `Source URL`, `Generated At`, `Status`

---

## n8n Concepts Used in These Workflows

| Concept | Where Used |
|---------|-----------|
| `gmailTrigger` polling | Email Draft Generator |
| Webhook trigger | Lead Qualification Agent |
| `formTrigger` (n8n built-in form) | Content Repurposing |
| Code node for data transformation | All 3 workflows |
| HTTP Request to external LLM APIs | All 3 workflows |
| IF node for conditional routing | Lead Qualification |
| Returning multiple items from Code node (fan-out) | Content Repurposing |
| `$('Node Name').first().json` cross-node reference | Lead Qualification, Content Repurposing |
