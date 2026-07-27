# Candidate Screening Automation (n8n)

## Problem Statement

Right now, when someone applies for a job, everything is manual. Someone has to open the form responses, read through each one, check the experience and skills, decide if the person should get an interview, and then email them back. When you get a lot of applications, this takes a lot of time and it's easy to miss someone or forget to reply.

## Objective

The goal of this project is to automate the whole process, from the moment someone fills the form to the moment they get an email back, without anyone having to touch it manually. The workflow should:

- Collect the candidate's info through a form
- Save it to a Google Sheet
- Apply screening rules to decide if the candidate should be selected
- Update the sheet with the result
- Send the candidate an email based on that result

## Workflow Architecture

```
Form Submission
      |
Append to Google Sheet (save raw data)
      |
IF: Experience >= 3 years?
      |
   TRUE -------------------------- FALSE
      |                               |
Update Sheet                   IF: Skills contain
(selection = true)              required keywords?
      |                               |
Send Email                    TRUE ---------- FALSE
(Selected)                      |                |
      |                  Update Sheet      Update Sheet
      |                  (selection=true)  (selection=false)
      |                        |                 |
      |                  Send Email         Send Email
      |                  (Selected)         (Rejected)
```

## Technologies Used

- n8n (workflow automation)
- Google Forms / n8n Form Trigger (for candidate applications)
- Google Sheets (as the database for storing and updating candidate records)
- Gmail (for sending result emails)

## Nodes Used

| Node Name | Type | Purpose |
|---|---|---|
| On form submission | Form Trigger | Collects candidate data (Name, Email, Degree, Skills, Experience, Availability) |
| Append row in sheet | Google Sheets (Append) | Saves the raw application as a new row |
| If | IF | Checks if Experience >= 3 years |
| skills check | IF | Checks if Skills field contains relevant keywords |
| Update row in sheet | Google Sheets (Update) | Marks selection = true, matched by Email |
| Update row in sheet1 | Google Sheets (Update) | Marks selection = false, matched by Email |
| Send Email - Selected | Gmail | Sends interview confirmation email |
| Send Email - Rejected | Gmail | Sends rejection email |

## Setup Instructions

1. Import the workflow JSON into n8n (Menu → Import from File).
2. Connect your Google account under the **Google Sheets** nodes (all three of them use the same credential).
3. Connect your Gmail account under both **Send Email** nodes.
4. Open the Google Sheet and make sure the column headers match exactly what the workflow expects (Name, Email, Degree, Skills, Experience, Availability, selection).
5. Activate the form trigger and get the public form URL from the node.
6. Turn the workflow to **Active** once everything is tested.

## Credentials Required

- Google Sheets OAuth2 account
- Gmail OAuth2 account

(No credential values are included in this repo — only the accounts need to be connected inside n8n.)

## Workflow Explanation

When someone submits the form, their answers are first saved as a new row in the Google Sheet so we always have a record, even before any decision is made.

Then the workflow checks the candidate's experience. If they have 3 years or more, they're selected right away, no matter what skills they listed. If they have less than 3 years, the workflow looks at their Skills field and checks if it contains any of the keywords we're looking for. If it does, they still get selected. If not, they're marked as not selected.

After the decision is made, the same row in the sheet gets updated (matched using the candidate's email, since it's unique per person) so the "selection" column shows true or false. Right after that, an email goes out automatically — one message if they're selected, a different one if they're not.

## Test Cases

| Test | Input | Expected Result |
|---|---|---|
| High experience | Experience = 5 years, any skills | Selected |
| Low experience, matching skills | Experience = 1 year, Skills contains "javascript" | Selected |
| Low experience, no matching skills | Experience = 1 year, Skills = "excel, ms word" | Not selected |
| Missing required field | Email left blank | Form blocks submission |
| Invalid email format | Email = "test123" | Form blocks submission |

## Error Handling

- Required fields (Name, Email, Degree) are validated at the form level, so incomplete submissions can't go through.
- The Email field type is set to "Email" so the form itself rejects badly formatted addresses.
- Google Sheets updates are matched using Email, so as long as the email is unique per candidate, the correct row always gets updated.
- If a Google Sheets or Gmail node fails (e.g. token expired), the workflow execution will show as failed in n8n's execution log, which can be checked manually for now.

## Known Limitations

- The skills check only looks for exact keyword matches inside the Skills text field (using "contains"). It doesn't understand skills that are worded differently or spelled differently.
- If two candidates ever use the same email by mistake, the sheet update could affect the wrong row.
- There's no automatic retry if the Gmail or Sheets node fails mid-run.
- The experience value depends on the candidate entering a proper number in the form.

## Future Improvements

-  adding a scoring system instead of true/false, resume parsing, deduplication by email, automatic retries on failure, a dashboard to view results, etc.

---

## My Notes / Struggles While Building This

- I faced a few issues while building this workflow, but I managed to fix most of them by going through the n8n docs and asking LLMs for help. The biggest challenge was the logic itself — figuring out the right order for the IF conditions. I actually sketched the whole flow on paper first and rewrote it a few times until it made sense to me.

- I also got confused about Documents vs Sheets in the Google Sheets node at first. I didn't realize a "Document" is the whole spreadsheet file and a "Sheet" is just a tab inside it, so I looked that up and it clicked pretty quickly after that.

- There were a couple of smaller issues too, like an email showing {{ }} instead of the actual candidate name — turned out I forgot to set that field to Expression mode. Small thing, but it took me a bit to spot.