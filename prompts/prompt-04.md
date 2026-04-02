# Prompt 04
# Rules Config Panel + Form Reset + Edge Cases + Polish
Here is the current index.html code: [PASTE YOUR CURRENT CODE HERE]

This is the polish and hardening sprint. Add the Rules Config panel, 
handle all edge cases, and refine the UX throughout.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART A — RULES CONFIG PANEL (Panel 3 in nav)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Replace the placeholder with a live rules editor:

HEADER:
  - Title: "Rules Configuration" in Syne 700
  - Subtitle: "Modify eligibility thresholds without touching code. 
    Changes apply immediately to new submissions."
  - A "Cohort" field at the top — text input showing current cohort name 
    (default "Cohort-2025"), editable. Any change updates RULES_CONFIG.cohort live.

SOFT RULES EDITOR (editable cards):
  Create 4 editable rule cards, one per soft rule:

  Card 1 — Age Range:
    [Min Age: ___] [Max Age: ___] [Save]
    Current: 18 – 35 years
    
  Card 2 — Graduation Year Range:
    [Min Year: ____] [Max Year: ____] [Save]
    Current: 2015 – 2025
    
  Card 3 — Academic Score Threshold:
    [Min Percentage: ___%] [Min CGPA: ___] [Save]
    Current: ≥60% or ≥6.0 CGPA
    
  Card 4 — Screening Test Threshold:
    [Min Score: ___] out of 100 [Save]
    Current: ≥40 / 100

  Each card:
  - Has input fields pre-filled with current RULES_CONFIG values
  - "Save" button updates the in-memory RULES_CONFIG object immediately
  - Shows a green toast notification: "✓ Rule updated — applies to new submissions"
  - Shows "Last updated: just now" timestamp below
  - Input validation on the config inputs themselves 
    (e.g., min age cannot exceed max age)

EXCEPTION CONFIG EDITOR:
  Card for exception settings:
  - Min rationale length: [___] characters (default 30)
  - Max exceptions before flag: [___] (default 2)
  - Required keywords list — shown as editable chips:
    [approved by ×] [special case ×] [documentation pending ×] [waiver granted ×]
    + [+ Add keyword] button that adds a new text input
  - [Save Exception Rules] button

READ-ONLY STRICT RULES DISPLAY:
  Below the editable section, show a read-only list of all 7 strict rules:
  - Each shown as a card with a red "STRICT" badge
  - Field name, rule description, example of valid/invalid input
  - A note: "Strict rules are hardcoded and cannot be modified from this panel."

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART B — FORM RESET & STATE MANAGEMENT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. After successful submission, "Submit Another" button must:
   - Reset ALL 11 fields to empty
   - Remove all error/success/warning CSS classes
   - Reset all exception toggles to OFF and collapse rationale areas
   - Reset eligibility score to 0
   - Reset progress bar to 0%
   - Reset all mini-metric chips to default
   - Re-enable offer letter field if it was locked, then re-lock it 
     (because interview is now blank again)
   - Scroll to top of form smoothly

2. Add a "Clear Form" button (small, text-secondary color, top right of form card):
   - Same reset behavior as above
   - Show a confirmation: "Are you sure you want to clear all fields? 
     This cannot be undone." (use a small inline confirmation row, not browser alert)

3. Browser refresh persistence:
   - On page load, check localStorage for "admitguard_draft"
   - If exists, show a banner: "↩ Unsaved draft found from [time]. 
     [Restore Draft] [Discard]"
   - Save form state to localStorage key "admitguard_draft" every 5 seconds 
     (only if at least one field has a value)
   - Clear the draft key on successful submission or "Clear Form"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART C — EDGE CASES TO HANDLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Handle all of these explicitly:

1. Phone number: strip spaces/dashes before validation (so "98765 43210" still validates)
2. Email: trim whitespace, convert to lowercase before storing
3. Name: trim leading/trailing whitespace, reject if only spaces
4. Age: must be a whole number — reject decimals like 25.5
5. Graduation year: must be a 4-digit number — reject "25" or "20250"
6. Score: if CGPA mode, max is 10.0; if % mode, max is 100. Reject out-of-range values.
7. Test score: must be between 0 and 100 inclusive
8. Aadhaar: strip spaces before validation. Validate it's not all same digit (e.g., 111111111111)
9. If localStorage is unavailable (private browsing): 
   - Fall back to in-memory array
   - Show a persistent info banner: "ℹ Storage unavailable in private mode. 
     Records will be lost on page close. Export CSV to save."
10. If more than 500 records in localStorage (size limit concern):
    - Show warning: "⚠ Audit log has 500+ records. Consider exporting and clearing 
      old records to maintain performance."

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PART D — UX POLISH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. TOAST NOTIFICATION SYSTEM:
   Build a global showToast(message, type) function where type is 
   "success" | "warning" | "error" | "info".
   Toast appears bottom-right, slides in from right, auto-dismisses after 3 seconds.
   Max 3 toasts visible at once (queue older ones out).
   Use this for: rule saves, form clear confirmations, export success messages.

2. KEYBOARD SHORTCUTS:
   - Ctrl/Cmd + Enter: submit form (if enabled)
   - Escape: close any open modal
   - Show a small keyboard hint near the submit button: "⌘ Enter to submit"

3. FIELD FOCUS FLOW:
   - Tab order must follow visual field order (name → email → phone → aadhaar → ...)
   - After fixing an error and tabbing away, the error should clear immediately
     if the new value is now valid

4. MOBILE RESPONSIVENESS:
   - Below 768px: switch 2-column grid to single column
   - Nav tabs scroll horizontally on small screens
   - Audit log table becomes a card-stack layout on mobile 
     (each row becomes a card with label: value pairs)
   - Touch-friendly: all tap targets minimum 44px height

5. LOADING STATE:
   When the submit button is clicked:
   - Button shows spinner (CSS animation, no library needed) + "Validating..."
   - Simulate 800ms processing delay before showing success overlay
   - This makes the app feel professional, not instant-jank

Output the complete updated index.html.