# Prompt 03
# Exception Flagging + Audit Trail + Persistent Storage
Here is the current index.html code: [PASTE YOUR CURRENT CODE HERE]

Add the full audit trail system and wire up form submission. 
Do not change any visual design or validation logic already built.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART A — FORM SUBMISSION & FLAG LOGIC
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

When the submit button is clicked (and all validations pass):

1. Run a final full-form validation pass across all 11 fields
2. Collect all active exceptions (field name + rationale text)
3. Determine flag status:
   - flagged = true if exceptions.length > 2
   - flagReason = "More than 2 exceptions used — requires manager review"
4. Build this exact audit record object:

   {
     id: crypto.randomUUID(),              // unique record ID
     cohort: RULES_CONFIG.cohort,          // "Cohort-2025"
     rulesVersion: RULES_CONFIG.version,
     timestamp: new Date().toISOString(),
     submittedBy: "Operator",              // static for now
     fields: {
       name:         <value>,
       email:        <value>,
       phone:        <value>,
       aadhaar:      <value>,              // store last 4 digits only: "XXXX-XXXX-1234"
       age:          <value>,
       qualification:<value>,
       grad_year:    <value>,
       score:        <value>,
       score_mode:   "pct" or "cgpa",
       test_score:   <value>,
       interview:    <value>,
       offer_letter: <value>
     },
     exceptions: [
       { field: <fieldName>, rule: <ruleName>, rationale: <text>, keyword_used: <phrase> }
     ],
     eligibilityScore: <final score 0-100>,
     flagged: true/false,
     flagReason: <string or null>,
     strictViolations: [],                 // list of any strict field errors (should be empty on submit)
     softViolations: []                    // list of soft fields that needed exceptions
   }

5. IMPORTANT — Aadhaar masking: before saving to storage, mask aadhaar as 
   "XXXX-XXXX-" + last4digits. Never store the full 12-digit number.

6. Save to localStorage:
   - Key: "admitguard_records"
   - Value: JSON.stringify(array of all records)
   - On load: always read existing records from localStorage first
   - Update the submittedEmails array from localStorage on load (for duplicate check)

7. Post-submission UI:
   - Show a full-screen success overlay (not an alert):
     * If NOT flagged: green checkmark icon + "Record Submitted Successfully" + 
       "Candidate: [Name] · Score: [X]/100 · Record ID: [first 8 chars of UUID]"
     * If flagged: amber warning icon + "Record Submitted — Flagged for Review" + 
       "This record has [X] exceptions and requires manager approval before proceeding."
   - Overlay has two buttons: "Submit Another" (resets form) | "View Audit Log" 
     (switches to Audit Log panel)
   - Overlay auto-dismisses after 6 seconds if no button clicked

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART B — AUDIT LOG PANEL (Panel 2 in nav)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Replace the placeholder with a full audit log view:

HEADER ROW:
  - Title: "Audit Log" in Syne 700, 24px
  - Subtitle: "X records total · X flagged" (live count from localStorage)
  - Right side: Three action buttons:
    * "Export CSV" — downloads all records as a .csv file
    * "Export JSON" — downloads raw JSON
    * "Clear All" — with a confirmation dialog before deleting

FILTER & SEARCH BAR:
  - Text search input: searches across name, email, record ID
  - Filter dropdown: "All Records" | "Flagged Only" | "Clean Records" | "With Exceptions"
  - Sort dropdown: "Newest First" | "Oldest First" | "Score High–Low" | "Score Low–High"
  - Results count: "Showing X of Y records"

RECORDS TABLE:
  Display as a clean dark table with these columns:
  Timestamp | Candidate Name | Email | Score | Exceptions | Status | Actions

  - Timestamp: formatted as "Apr 2, 2025 · 3:47 PM"
  - Score: colored badge (green/amber/red based on score range)
  - Exceptions: shows count, e.g. "2 exceptions" in amber, "0" in muted
  - Status: "Clean" (green badge) or "Flagged" (red badge)
  - Actions: small "View" button that opens a detail modal

RECORD DETAIL MODAL:
  When "View" is clicked, show a modal overlay with:
  - Full record ID
  - All 11 field values in a 2-column grid
  - Exception details (field, rationale, keyword used)
  - Flag reason if flagged
  - Eligibility score with the colored indicator
  - Cohort and rules version it was submitted under
  - A "Close" button

EMPTY STATE:
  When no records exist: show a centered illustration using ASCII/unicode shapes
  and text: "No records yet. Submit your first candidate entry to see it here."

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART C — CSV EXPORT FORMAT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

The CSV download must include these exact headers:
Record ID, Timestamp, Cohort, Candidate Name, Email, Phone, Aadhaar (Masked),
Age, Qualification, Graduation Year, Score, Score Type, Test Score, 
Interview Status, Offer Letter, Eligibility Score, Exceptions Count, 
Exception Details, Flagged, Flag Reason

Filename: admitguard-audit-[YYYY-MM-DD].csv
Trigger as a real browser download (create a blob URL and click it programmatically).

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART D — DASHBOARD ANALYTICS CARD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

At the top of the Audit Log panel, above the table, add a stats row with 5 metric cards:

  1. Total Records (number)
  2. Flagged Records (number + % of total in small text below)
  3. Average Eligibility Score (average of all scores)
  4. Most Common Exception (which field has the most exceptions, or "None")
  5. Clean Submissions (records with 0 exceptions and not flagged)

These should recalculate every time the audit log panel is opened or data changes.

Output the complete updated index.html.