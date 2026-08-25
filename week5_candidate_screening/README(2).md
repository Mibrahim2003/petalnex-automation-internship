# AI-Assisted Recruitment Automation

An n8n workflow for processing Machine Learning Engineer applications. AI extracts job-related evidence and produces an advisory recommendation, while an authorized human makes every final shortlist or rejection decision.

## Workflow

`Application Form → Validation → Google Drive/Sheets → PDF Extraction → AI Evidence Extraction → Deterministic Scoring → Human Review → Candidate Email → Sheet Update`

## Job Criteria

- At least 3 years of relevant ML experience
- Python, NumPy, pandas, scikit-learn, SQL, and Git
- One end-to-end classical machine-learning project
- One substantial deep-learning project
- PyTorch or TensorFlow/Keras
- Production and MLOps skills are preferred

## Scoring

- Core ML skills: 25
- Classical ML project: 20
- Deep-learning project: 20
- Relevant experience: 15
- Production/MLOps: 15
- Education or equivalent learning: 5

AI recommendations are `RECOMMEND_SHORTLIST`, `NEEDS_CLOSER_REVIEW`, or `RECOMMEND_REJECT`. These are advisory only.

## Setup

1. Import `Week_5_AI_Assisted_Recruitment_ML_Engineer_n8n.json` into n8n.
2. Create a Google Sheet with a tab named `Candidates` and copy the header row from the blue workflow note.
3. Open `01 - Configure and Validate` and add the reviewer email and Google Sheet ID.
4. Connect Google Sheets, Google Drive, Gmail, and Gemini credentials.
5. Test the valid, incomplete, shortlist, and rejection paths.
6. Activate the workflow and share the Production Form URL.

## Safety and Error Handling

- Candidate email addresses and phone numbers are redacted before AI analysis.
- Protected personal traits are excluded from assessment.
- Candidate text is treated as untrusted input.
- External service calls use retries.
- PDF, Drive, or AI failures are routed for manual review where possible.
- Incomplete applications receive a correction request instead of rejection.
- The workflow waits for the authorized reviewer before recording a final decision.

## Files

- `Week_5_AI_Assisted_Recruitment_ML_Engineer_n8n.json` — complete connected n8n workflow
- `README.md` — project documentation
