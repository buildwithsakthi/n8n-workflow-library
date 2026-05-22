# ⚡ n8n Workflow Library

> A curated collection of production-ready **n8n workflow templates** for AI automation, CRM sync, invoicing, voice agents, and more. Copy, import, and use instantly.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![n8n](https://img.shields.io/badge/built%20for-n8n-orange)](https://n8n.io)

---

## 🚀 Why This Exists

Every week, businesses pay $50–150/hr to build the same automations over and over. This library gives you **production-tested, import-ready templates** so you can skip the boilerplate and ship faster.

---

## 📂 Workflow Categories

| Category | Description |
|----------|--------------|
| 🤖 AI Automation | LLM-powered email drafts, AI agents, content generation |
| 📋 CRM & Sales | Lead sync, follow-up sequences, pipeline automation |
| 💰 Invoicing & Payments | Stripe triggers, invoice generation, payment reminders |
| 📣 Marketing | Social scheduling, newsletter automation |
| 🔔 Notifications & Alerts | Slack/email alerts, monitoring, status updates |
| 🗂 Data Sync | Airtable, Google Sheets, database sync |

---

## 🤖 AI Automation

### 1. AI Email Draft Generator
Automatically draft professional replies using OpenAI or Claude based on incoming email content.

**Nodes:** Gmail Trigger → HTTP Request (OpenAI/Claude) → Gmail Send  
**File:** `/workflows/ai/ai-email-draft-generator.json`

---

### 2. AI Lead Qualification Agent
Score and qualify inbound leads with an LLM, then route to the right CRM pipeline stage.

**Nodes:** Webhook → HTTP Request (Claude API) → Airtable → Slack  
**File:** `/workflows/ai/ai-lead-qualification-agent.json`

---

### 3. AI Content Repurposing Pipeline
Turn a blog post URL into tweets, LinkedIn posts, and email copy automatically.

**Nodes:** HTTP Request → OpenAI → Google Sheets → Buffer  
**File:** `/workflows/ai/ai-content-repurposing.json`

---

## 📋 CRM & Sales

### 4. New Lead to CRM + Slack Notification
Form submitted? Create a contact in HubSpot/Pipedrive and ping the sales team on Slack instantly.

**Nodes:** Webhook → HubSpot → Slack  
**File:** `/workflows/crm/new-lead-to-crm-slack.json`

---

### 5. Automated Cold Follow-Up Sequence
Send a 3-step email follow-up when a deal has had no activity for X days.

**Nodes:** Schedule Trigger → HubSpot → IF → Gmail  
**File:** `/workflows/crm/automated-follow-up-sequence.json`

---

## 💰 Invoicing & Payments

### 6. Stripe Payment → Invoice + Thank You Email
When a Stripe payment succeeds, auto-generate an invoice and send a branded thank-you email.

**Nodes:** Stripe Trigger → PDF Generator → Gmail  
**File:** `/workflows/invoicing/stripe-payment-invoice-email.json`

---

### 7. Overdue Invoice Reminder
Auto-send payment reminders at 3, 7, and 14 days for unpaid invoices.

**Nodes:** Schedule Trigger → Airtable → IF → Gmail  
**File:** `/workflows/invoicing/overdue-invoice-reminder.json`

---

## 📥 How to Import a Workflow

1. Open your n8n instance
2. Go to **Workflows** → click **Import from file**
3. Select the `.json` file from this repo
4. Update credentials (API keys, OAuth)
5. Activate — you're live! ✅

> Works with both **n8n Cloud** and **self-hosted** instances.

---

## 🛠 Requirements

- [n8n](https://n8n.io) v1.0+
- Relevant API credentials per workflow (listed in each workflow folder)

---

## 🤝 Contributing

Got a workflow that saves you hours? Share it!

1. Fork this repo
2. Add your `.json` to the right category folder
3. Add a short description in the folder README
4. Open a Pull Request

---

## 📜 License

MIT — free to use, modify, and distribute. See [LICENSE](./LICENSE).

---

## ⭐ If this saved you time, please star this repo!

*Built with ❤ by [Sakthivel Kandasamy](https://github.com/buildwithsakthi)*
