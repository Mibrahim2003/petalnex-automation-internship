# AI Customer Support Triage System

An n8n-based AI customer support workflow for an earbuds business. The system collects customer complaints, analyzes them with an LLM, prioritizes and categorizes them, stores the ticket, sends an acknowledgement, and alerts support when a case is urgent.

## Workflow

```text
Customer Form
     ↓
Gemini AI Analysis
     ↓
Structured Output
     ↓
Category Switch
     ↓
Google Sheets
     ↓
Customer Acknowledgement

Gemini AI
     ↓
Urgent Alert?
   ↙       ↘
 No         Yes
             ↓
       Urgent Alert
```

## How It Works

1. The customer submits the support form.
2. The form collects:
   - Name
   - Email
   - Problem(s)
   - Problem description
3. Gemini analyzes the problem and description.
4. The Structured Output Parser produces a consistent result.
5. The system determines:
   - Category
   - Priority
   - Sentiment
   - Department
   - Summary
   - Suggested response
   - Urgent alert status
   - Confidence
6. The Switch node routes the ticket based on its category.
7. The ticket is stored in a Google Sheet.
8. The customer receives an acknowledgement.
9. If the AI marks the ticket as urgent, an urgent alert is sent to support.

## Triage

Triage means determining how a customer support request should be handled and how important it is.

The workflow uses four priority levels:

- `urgent` — immediate attention is required
- `high` — serious customer/product issue
- `medium` — normal support issue requiring follow-up
- `low` — minor issue or general question

Urgent cases can include overheating, smoke, exposed electrical components, battery swelling, or other potentially unsafe situations.

## Categories

- `charging_issue`
- `battery_issue`
- `connectivity_issue`
- `sound_issue`
- `microphone_issue`
- `physical_damage`
- `missing_item`
- `order_delivery`
- `refund_return`
- `warranty_support`
- `other`

## Technologies

- n8n
- Google Gemini
- Structured Output Parser
- Google Sheets
- Gmail

## Setup

1. Import the n8n workflow JSON.
2. Connect your Google Gemini credentials.
3. Connect your Google Sheets credentials.
4. Connect your Gmail credentials.
5. Change the support alert email to your actual support address.
6. Submit test forms and check the workflow execution.

## Testing

Test both normal and urgent requests.

Example urgent request:

> My earbuds became extremely hot while charging and I noticed smoke coming from the charging case. I unplugged them immediately.

Expected result:

```json
{
  "priority": "urgent",
  "urgent_alert": true
}
```

Normal issues should not automatically trigger an urgent alert.

## Result

The final system provides an automated customer support triage workflow that can classify incoming requests, determine their priority, route them by category, store them, acknowledge customers, and escalate urgent cases.
