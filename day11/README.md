# Day 11: Multi-System Integration - Lead Processing Workflow 

## Pipeline Overview
This workflow automates the intake, validation, enrichment, and notification of new business leads. The data flows sequentially through the following systems: 

`New Lead (Webhook) -> Validation (If Node) -> Data Enrichment (HTTP Request) -> Storage (Edit Fields) -> Notification (Gmail) -> Confirmation (Webhook Response)`

## System Components
*   **Trigger:** An n8n Webhook listening for `POST` requests containing a lead's basic information (name and email).
*   **Data Enrichment:** The workflow uses the free, external [Agify.io API](https://api.agify.io/) via an HTTP Request to estimate the lead's age based on their provided name, enriching the incoming profile.
*   **Data Storage:** An `Edit Fields` (Set) node is utilized to merge the original lead inputs and the enriched API demographic data into a single, structured record.
*   **Notification Channel:** A properly authenticated Gmail node automatically sends a formatted email alert to the team containing the newly enriched lead details.
*   **Confirmation:** A final `Respond to Webhook` node returns a `200 OK` JSON success message back to the original sender to close the loop.