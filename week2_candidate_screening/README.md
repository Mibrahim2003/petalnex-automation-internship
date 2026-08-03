# Automated Lead Management System (n8n)

A complete, scored, and routed lead-management automation built with n8n. This workflow accepts incoming leads, validates the data, scores them based on budget tiers, routes them to appropriate channels, and sends an automated acknowledgment.

## Workflow Architecture
1. **Webhook:** Receives incoming JSON lead data.
2. **Validation:** Ensures `name` and `email` exist, and `budget` is greater than 0.
3. **Processing:** Standardizes the data (e.g., lowercases the email address).
4. **Scoring & Routing:** Evaluates the `budget` field to route leads into three buckets:
   - **High Priority (Budget >= $5,000):** Triggers an immediate internal sales alert via Gmail.
   - **Medium Priority ($1,000 - $4,999):** Appends the lead to a "Lead DB" Google Sheet.
   - **Low Priority (<= $999):** Appends the lead to a "Nurture List" Google Sheet.
5. **Acknowledgment:** Sends a final confirmation email to every valid lead via Gmail.

## Sample API Request
Use the following `cURL` command to send a test request to the Webhook (or paste the JSON body into Postman):

```bash
curl -X POST 'https://[YOUR_N8N_WEBHOOK_URL]' \
-H 'Content-Type: application/json' \
-d '{
  "name": "Muhammad Ibrahim",
  "email": "ibrahimnewacc03@gmail.com",
  "company": "Patelnex",
  "service": "AI automation",
  "budget": 5000
}'
```

## Setup & Execution
1. Import the provided workflow JSON file into your n8n workspace.
2. Open the **Webhook** node and copy your Test/Production URL to use in your API request.
3. Authenticate the **Gmail** nodes and **Google Sheets** nodes with your Google Workspace account.
4. Select your specific Google Sheets document and tab names inside the Google Sheets nodes.
5. Set the workflow to **Active** and fire your sample API request to watch the magic happen!
