# Prompt 02 
# Validation Engine: Strict Rules + Soft Rules + Real-time Feedback

Here is the current index.html code: [PASTE YOUR CURRENT CODE HERE]
Now add the complete validation engine. Do not change any visual design or layout.
Only add JavaScript logic and update field states (error/success CSS classes).
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART A — RULES CONFIGURATION OBJECT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
At the top of your JS, define this exact config object (this simulates rules.json):
const RULES_CONFIG = {
cohort: "Cohort-2025",
version: "1.0",
strict: [
{ field: "name", type: "regex", pattern: "^[a-zA-Z\s]{2,}
",
message: "Must be exactly 10 digits and start with 6, 7, 8, or 9." },
{ field: "aadhaar", type: "regex", pattern: "^\d{12}$",
message: "Must be exactly 12 digits. No spaces or letters." },
{ field: "qualification",type: "required",
message: "Please select a qualification from the dropdown." },
{ field: "interview", type: "not_value", blocked_value: "Rejected",
message: "Candidate status is Rejected. Submission is blocked." },
{ field: "offer_letter", type: "conditional", depends_on: "interview",
required_value: "Cleared",
message: "Offer Letter can only be issued after interview is cleared." }
],
soft: [
{ field: "age", type: "range", min: 18, max: 35,
message: "Age should be between 18–35 years." },
{ field: "grad_year", type: "range", min: 2015, max: 2025,
message: "Graduation year should be between 2015–2025." },
{ field: "score", type: "threshold",min_pct: 60, min_cgpa: 6.0,
message: "Score should be ≥60% or ≥6.0 CGPA." },
{ field: "test_score", type: "threshold",min: 40, max: 100,
message: "Screening test score should be ≥40 out of 100." }
],
exception: {
min_rationale_length: 30,
required_keywords: ["approved by", "special case", "documentation pending", "waiver granted"],
max_exceptions_before_flag: 2
}
};
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART B — STRICT RULE VALIDATION FUNCTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Build a validateField(fieldName, value) function that runs on:
Every field: on blur event (when user leaves the field)
Every field: on input event with 300ms debounce (while typing)
All fields: on submit attempt
For each strict rule violation:
Add CSS class "field-error" to the input wrapper div
Show the error message from RULES_CONFIG below the field
with a ⚠ icon prefix, in danger color, 12px font
Remove any "field-success" class
For each strict rule that passes:
Add CSS class "field-success" to the input wrapper div
Show a small green ✓ icon inside the input on the right side
Remove any "field-error" class
SPECIAL STRICT BEHAVIORS:
A) Email uniqueness check:
- Maintain an array called submittedEmails in memory
- On email field blur, check if value exists in submittedEmails array
- If duplicate: show error "⚠ This email was already submitted on [date].
Duplicate entries are not allowed."
- If not duplicate: normal success state
B) Interview Status = "Rejected":
- When this dropdown is selected, immediately:
* Show a full-width red banner above the submit button:
"🚫 Submission Blocked — Candidate has been rejected.
This record cannot be submitted."
* Disable the submit button completely
* Add a red border to the entire form card
- When changed away from "Rejected": remove all above
C) Offer Letter conditional lock:
- If Interview Status is NOT "Cleared":
* Disable the Offer Letter dropdown
* Show helper text: "🔒 Locked — available only after interview is cleared"
* Visually grey out the field
- If Interview Status IS "Cleared":
* Enable the dropdown, remove lock styling
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART C — SOFT RULE VALIDATION WITH EXCEPTION SYSTEM
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
When a soft rule is violated (after blur):
Show the field in WARNING state — amber border, amber warning message
Below the warning message, render this exception UI:
┌─────────────────────────────────────────────────────────┐
│ ⚡ Rule Exception │
│ Toggle to request an override for this field │
│ [ Request Exception ○ ] ← toggle switch │
└─────────────────────────────────────────────────────────┘
When toggle is switched ON, expand a section below it:
┌─────────────────────────────────────────────────────────┐
│ Exception Rationale * │
│ ┌───────────────────────────────────────────────────┐ │
│ │ Textarea: "Describe the reason for this exception"│ │
│ │ (min-height: 80px, same dark styling as inputs) │ │
│ └───────────────────────────────────────────────────┘ │
│ Characters: 0/30 minimum [keyword chips below] │
│ Required phrase: ○ approved by ○ special case │
│ ○ documentation pending ○ waiver │
│ (chips turn green ✓ when phrase detected in textarea) │
└─────────────────────────────────────────────────────────┘
The rationale textarea validates in real-time:
Character counter: "12 / 30 minimum" — turns green when ≥30
Keyword detection: scan textarea content for each required phrase,
light up the matching chip in green when found
Field is only considered "exception-cleared" when BOTH conditions met:
≥30 characters AND at least one keyword phrase detected
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART D — ELIGIBILITY SCORE ENGINE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Build a calculateEligibilityScore() function that runs after every field change:
Score starts at 100. Deductions:
Each strict rule violated: -12 points (7 strict rules × 12 = 84 max deduction)
Each soft rule violated AND no exception: -4 points
Each soft rule violated WITH valid exception: -1 point (small penalty for exceptions)
More than 2 exceptions active: additional -10 points
Update the eligibility score card:
Animate the number change (count up/down smoothly over 400ms)
Color: ≥80 = success green, 50–79 = warning amber, <50 = danger red
Update the 4 mini-metric chips: "Strict Rules: X/7 passed",
"Soft Rules: X/4 checked", "Exceptions: X used", and Status chip
Update the submit button state:
ENABLED only when: all strict rules pass AND
(all soft rules pass OR each soft violation has a valid exception)
DISABLED with appropriate message otherwise
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART E — SCORE FIELD SPECIAL HANDLING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
The Academic Score field has a % / CGPA toggle:
When % mode: validate value ≥ 60 (and ≤ 100)
When CGPA mode: validate value ≥ 6.0 (and ≤ 10)
Toggle switches the placeholder text and validation rule live
Store both the value and the mode ("pct" or "cgpa") for the audit record
Output the complete updated index.html with all validation logic added.