# 💰 Invoicing & Payments Workflows

Automate your billing cycle — from Stripe payments to overdue reminders — with zero manual work.

---

## Workflows

### 1. [`stripe-payment-invoice-email.json`](./stripe-payment-invoice-email.json)

Triggered the instant a Stripe payment succeeds. Extracts payment details, generates a PDF invoice via Documint, and sends a branded HTML thank-you email with the invoice attached.

**Flow:** Stripe Trigger → Extract Payment Data → Documint PDF API → Gmail (HTML + PDF attachment)

**Credentials needed:**
- Stripe API Key (Webhook signing secret configured in Stripe Dashboard)
- Documint API Key (free tier available at [documint.me](https://documint.me))
- Gmail OAuth2

**Stripe setup:**
1. Go to Stripe Dashboard → Developers → Webhooks
2. Add endpoint: `https://your-n8n.com/webhook/stripe-payment-webhook`
3. Select event: `payment_intent.succeeded`
4. Add `receipt_email` and `customer_name` to your Stripe PaymentIntent metadata

**Documint setup:**
1. Create a free account at documint.me
2. Design your invoice template using their visual editor
3. Copy the Template ID into the HTTP Request node URL
4. Match your template variables to the field names in the workflow

**Without Documint:** Replace the PDF step with an HTTP Request to any PDF API (DocRaptor, Carbone, or your own endpoint). The Gmail node attaches `data` (binary property) — just make sure your PDF API returns a binary response.

---

### 2. [`overdue-invoice-reminder.json`](./overdue-invoice-reminder.json)

Runs daily at 10 AM. Checks Airtable for all unpaid invoices past their due date, calculates days overdue, and sends the right reminder email. Automatically increments the reminder counter and marks Day-14 invoices as "Escalated" so your team can take over.

**Reminder schedule:**
| Reminder | Days Overdue | Tone |
|----------|-------------|------|
| 1st | 3 days | Friendly, assumes oversight |
| 2nd | 7 days | Firm, asks for response |
| 3rd | 14 days | Final notice, mentions escalation |

**Flow:** Schedule (10AM daily) → Airtable Get Unpaid → Classify Reminders → IF (skip?) → Switch (step 1/2/3) → Gmail → Airtable Update

**Credentials needed:**
- Airtable Personal Access Token (with `data.records:read` and `data.records:write` scopes)
- Gmail OAuth2

**Airtable setup:**
Create a base with a table called `Invoices` containing these fields:
| Field | Type |
|-------|------|
| Invoice Number | Single line text |
| Customer Name | Single line text |
| Customer Email | Email |
| Amount | Number (decimal) |
| Currency | Single line text |
| Due Date | Date |
| Invoice Date | Date |
| Status | Single select: `Unpaid`, `Paid`, `Escalated` |
| Reminders Sent | Number (integer, default 0) |
| Last Reminder Date | Date |

**Customisation:**
- Update `[Payment Link]` in each Gmail template with your actual payment URL
- Update `[Your Company Name]` with your business name
- Add a Slack notification after the Day-14 email to alert your accounts team

---

## n8n Concepts Used

| Concept | Where Used |
|---------|-----------|
| `stripeTrigger` (event-based webhook) | Stripe → Invoice |
| Binary file handling (PDF attachment in Gmail) | Stripe → Invoice |
| Airtable formula filter (`filterByFormula`) | Overdue Reminders |
| `$input.all()` loop + conditional filtering | Overdue Reminders |
| Airtable `update` operation with record ID | Overdue Reminders |
| Cron schedule trigger | Both workflows |
| Cross-node data reference (`$('Node Name').first().json`) | Stripe → Invoice |
