# 📋 CRM & Sales Workflows

Automate your sales pipeline with these n8n workflows for lead capture, follow-ups, and CRM management.

---

## Workflows

### 1. [`new-lead-to-crm-slack.json`](./new-lead-to-crm-slack.json)

Receives a form submission via Webhook, creates a contact in HubSpot, and sends an instant Slack notification to the sales team. Works with Typeform, Tally, custom forms — anything that can POST JSON.

**Flow:** Webhook → Normalise Data → HubSpot Create Contact → Slack Alert → Respond to Webhook

**Credentials needed:**
- HubSpot App Token (create a Private App with `crm.objects.contacts.write` scope)
- Slack Bot Token (with `chat:write` scope, bot must be added to the channel)

**Webhook payload format:**
```json
{
  "first_name": "Jane",
  "last_name": "Doe",
  "email": "jane@example.com",
  "company": "Acme Corp",
  "phone": "+1-555-0100",
  "message": "Interested in the enterprise plan",
  "utm_source": "linkedin"
}
```

**Customisation:**
- Edit `lifecycleStage` and `leadStatus` in the HubSpot node to match your pipeline
- Update the Slack message template to include a direct HubSpot link (replace the placeholder URL with your portal ID)

---

### 2. [`automated-follow-up-sequence.json`](./automated-follow-up-sequence.json)

Runs every weekday morning at 9 AM (configurable). Fetches open HubSpot deals with no recent activity, calculates days since last contact, and sends the right email for the right follow-up step — automatically.

**Follow-up schedule:**
| Step | Trigger | Email Type |
|------|---------|------------|
| 1 | Day 3 of silence | Gentle check-in |
| 2 | Day 7 of silence | Value reminder |
| 3 | Day 14 of silence | Break-up email |

**Flow:** Schedule (9AM weekdays) → HubSpot Get Deals → Filter & Classify → IF (skip?) → Switch (step 1/2/3) → Gmail

**Credentials needed:**
- HubSpot App Token (with `crm.objects.deals.read` scope)
- Gmail OAuth2

**Customisation:**
- Change the cron expression in the Schedule node: `0 9 * * 1-5` = 9AM Mon–Fri
- Update `dealStage` filter in the HubSpot node to match your pipeline stage IDs
- Personalise the email templates in each Gmail node — keep them short and human
- Change `sendTo` to the actual rep's email (ideally pulled from HubSpot deal owner)

---

## n8n Concepts Used

| Concept | Where Used |
|---------|-----------|
| `respondToWebhook` node (returns HTTP response to caller) | New Lead to CRM |
| Data normalisation / field aliasing in Code node | Both workflows |
| Schedule Trigger with cron syntax | Follow-Up Sequence |
| `$input.all()` loop to process multiple records | Follow-Up Sequence |
| Switch node for multi-branch routing | Follow-Up Sequence |
| Error throwing inside Code node for validation | Both workflows |
