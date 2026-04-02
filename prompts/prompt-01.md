# Prompt 01
#Foundation: Layout, Design System & Form Structure
You are a senior frontend developer. Your output must be 
ONE single complete index.html file. Nothing else. No React. 
No Vite. No npm. No components. No separate CSS files. 
No separate JS files. No package.json. No tsconfig. 
Just a single index.html file with all HTML, CSS, and 
JavaScript written inline inside it. The file must open 
by simply double-clicking it in a browser with zero setup.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PROJECT: AdmitGuard
Admission Data Validation & Compliance System
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

VISUAL DESIGN SYSTEM — implement exactly as described

Color palette (define as CSS variables inside :root):
  --bg-primary: #0F1117
  --bg-card: #1A1D27
  --bg-input: #13151F
  --bg-elevated: #20243A
  --accent: #4F6EF7
  --accent-hover: #3B56D4
  --success: #22C97A
  --warning: #F5A623
  --danger: #EF4444
  --text-primary: #F0F2FF
  --text-secondary: #8B91A8
  --text-muted: #4A5068
  --border: #2A2E45
  --border-focus: #4F6EF7

Typography — import at very top of <style> block:
  @import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap');
  --font-heading: 'Syne', sans-serif;
  --font-body: 'DM Sans', sans-serif;

Global rules:
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { background: var(--bg-primary); color: var(--text-primary); 
         font-family: var(--font-body); min-height: 100vh; }
  All cards: border-radius 10px
  All inputs: border-radius 8px
  All badges: border-radius 6px
  All transitions: 0.2s ease
  Custom scrollbar: 6px wide, dark track, accent colored thumb

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LAYOUT STRUCTURE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. FIXED TOP NAVIGATION BAR (height: 60px):

   Left side:
   - Inline SVG shield icon (simple shield shape, accent color fill)
   - Text "AdmitGuard" in font-family Syne, font-weight 700, 18px, 
     color text-primary, margin-left 8px

   Center:
   - Three pill-style tab buttons: "New Entry" | "Audit Log" | "Rules Config"
   - Default tab style: background transparent, color text-secondary, 
     padding 6px 18px, border-radius 20px, border none, cursor pointer,
     font-family Syne, font-size 13px, font-weight 600
   - Active tab style: background accent color, color white
   - Clicking each tab shows/hides the corresponding panel below

   Right side:
   - A small chip: green pulsing dot + text "System Active"
   - Pulsing dot: 8px circle, background success color, 
     CSS animation pulse (opacity 1 to 0.3, 1.5s infinite)
   - Chip font: DM Sans 12px, color text-secondary

   Bar styling:
   - background: var(--bg-card)
   - border-bottom: 1px solid var(--border)
   - position: fixed, top 0, width 100%, z-index 1000
   - display flex, align-items center, justify-content space-between
   - padding: 0 32px

2. MAIN CONTENT AREA:
   - max-width: 900px
   - margin: 0 auto
   - padding: 80px 24px 40px 24px  (80px top clears the fixed nav)

3. THREE PANELS (only one visible at a time, controlled by nav tabs):

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PANEL 1 — NEW ENTRY (default visible)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

A) PANEL HEADER:
   - Small label above title: "ADMISSION FORM" in Syne 600, 
     11px, letter-spacing 0.1em, color accent
   - Main title: "Candidate Entry" in Syne 700, 28px, color text-primary
   - Subtitle: "All fields are validated in real-time. 
     Strict rules cannot be overridden." 
     DM Sans 14px, color text-secondary, margin-top 6px
   
   - PROGRESS BAR below subtitle (margin-top 20px):
     * Outer track: width 100%, height 4px, background bg-elevated, 
       border-radius 2px
     * Inner fill: height 100%, background accent, border-radius 2px,
       width starts at 0%, transitions smoothly with CSS transition
     * Percentage label: right-aligned, 12px, Syne 600, color accent,
       shows "0%" updating to actual percentage as fields are filled
     * Label reads: "Form completion: 0%"

B) ELIGIBILITY SCORE CARD (margin-top 24px):
   - Card background: linear-gradient(135deg, #1A1D27 0%, #20243A 100%)
   - Border: 1px solid var(--border)
   - Border-radius: 12px
   - Padding: 20px 24px
   - Display: flex, justify-content space-between, align-items center

   Left side of card:
   - Label: "Eligibility Score" Syne 600, 11px, letter-spacing 0.08em,
     color text-secondary, uppercase
   - Large number: "0" in Syne 800, 52px
     * Color: success (#22C97A) if score ≥ 80
     * Color: warning (#F5A623) if score 50-79  
     * Color: danger (#EF4444) if score < 50
     * Starts at 0 (danger color)
   - Small text below number: "out of 100" DM Sans 12px, text-muted

   Right side of card (4 mini metric chips in a 2x2 grid):
   Each chip: background bg-input, border 1px solid border, 
   border-radius 8px, padding 8px 12px
   - Chip 1: label "Strict Rules" 10px text-muted / value "0 / 7 passed" 
     12px text-secondary Syne 600
   - Chip 2: label "Soft Rules" 10px text-muted / value "0 / 4 checked"
   - Chip 3: label "Exceptions" 10px text-muted / value "0 used"
   - Chip 4: label "Status" 10px text-muted / value "Incomplete" 
     with color cycling: Incomplete=muted, Warnings=warning, 
     Flagged=danger, Eligible=success

C) FORM CARD (margin-top 20px):
   - background: var(--bg-card)
   - border: 1px solid var(--border)
   - border-radius: 10px
   - padding: 28px

   ALL FIELD WRAPPER styling (apply to every field):
   - margin-bottom: 20px
   - Label: font-family Syne, font-size 11px, font-weight 600, 
     letter-spacing 0.08em, text-transform uppercase, 
     color text-secondary, display block, margin-bottom 6px
   - Required asterisk: color danger, margin-left 3px
   - Helper text below label: font-size 11px, color text-muted, 
     margin-bottom 6px, font-family DM Sans
   - Input/Select: 
       width: 100%
       background: var(--bg-input)
       border: 1.5px solid var(--border)
       border-radius: 8px
       padding: 12px 14px
       color: var(--text-primary)
       font-family: var(--font-body)
       font-size: 14px
       outline: none
       appearance: none (for selects)
   - Focus state: border-color var(--border-focus), 
     box-shadow: 0 0 0 3px rgba(79,110,247,0.15)
   - Error state (.field-error input): 
     border-color var(--danger), 
     background: rgba(239,68,68,0.05)
   - Success state (.field-success input): 
     border-color var(--success)
   - Error message: display block, font-size 12px, color danger, 
     margin-top 6px, font-family DM Sans
     prefix with "⚠ " before message text
   - Success icon: small "✓" shown inside input right side using 
     position relative + pseudo or a span

   FORM GRID LAYOUT — 2 column CSS grid with gap 20px:
   Use: display grid; grid-template-columns: 1fr 1fr; gap: 20px;
   Some fields span full width using: grid-column: 1 / -1

   EXACT 11 FIELDS in this order:

   Field 1 — Full Name (column 1)
   - Label: "Full Name *"
   - Helper: "Min 2 characters. Letters only, no numbers."
   - Input type: text
   - Placeholder: "Enter candidate's full name"

   Field 2 — Email Address (column 2)
   - Label: "Email Address *"
   - Helper: "Must be valid format. Duplicates not allowed."
   - Input type: email
   - Placeholder: "candidate@example.com"

   Field 3 — Phone Number (column 1)
   - Label: "Phone Number *"
   - Helper: "10 digits. Must start with 6, 7, 8, or 9."
   - Input type: tel
   - Placeholder: "9876543210"

   Field 4 — Aadhaar Number (column 2)
   - Label: "Aadhaar Number *"
   - Helper: "Exactly 12 digits. No spaces or letters."
   - Input type: text
   - Placeholder: "123456789012"
   - maxlength: 12

   Field 5 — Age (column 1)
   - Label: "Age *"
   - Helper: "Must be between 18 and 35 years."
   - Input type: number
   - Placeholder: "25"
   - min: 1, max: 99

   Field 6 — Qualification (column 2)
   - Label: "Qualification *"
   - Helper: "Select from dropdown only."
   - Element type: select (custom styled)
   - Options: 
       <option value="">Select qualification</option>
       <option value="Graduate">Graduate</option>
       <option value="Post-Graduate">Post-Graduate</option>
       <option value="Diploma">Diploma</option>
       <option value="Other">Other</option>

   Field 7 — Graduation Year (column 1)
   - Label: "Graduation Year *"
   - Helper: "Must be between 2015 and 2025."
   - Input type: number
   - Placeholder: "2022"
   - min: 1900, max: 2030

   Field 8 — Academic Score (column 2)
   - Label: "Academic Score *"
   - Helper: "Enter percentage (≥60%) or CGPA (≥6.0)"
   - This field has a TOGGLE between % and CGPA mode:
     * Two small pill buttons side by side above input: "%" and "CGPA"
     * Active mode pill: background accent, color white
     * Inactive mode pill: background bg-elevated, color text-secondary
     * Toggle changes placeholder: "Enter percentage (0-100)" or "Enter CGPA (0-10)"
     * Store mode in a variable: scoreMode = 'pct' or 'cgpa'
   - Input type: number
   - Placeholder changes based on mode

   Field 9 — Screening Test Score (column 1)
   - Label: "Screening Test Score *"
   - Helper: "Score out of 100. Minimum 40 required."
   - Input type: number
   - Placeholder: "65"
   - min: 0, max: 100

   Field 10 — Interview Status (column 2)
   - Label: "Interview Status *"
   - Helper: "Rejected status will block submission entirely."
   - Element type: select
   - Options:
       <option value="">Select status</option>
       <option value="Pending">Pending</option>
       <option value="Cleared">Cleared</option>
       <option value="Rejected">Rejected</option>

   Field 11 — Offer Letter Status (full width, grid-column 1/-1)
   - Label: "Offer Letter Status *"
   - Helper: "Only available after interview is cleared."
   - Element type: select
   - Options:
       <option value="">Select status</option>
       <option value="Not Issued">Not Issued</option>
       <option value="Issued">Issued</option>
       <option value="Pending Signature">Pending Signature</option>

D) REJECTION BANNER (hidden by default, shown when Interview = Rejected):
   - Full width, background rgba(239,68,68,0.1), border 1px solid danger,
     border-radius 8px, padding 14px 18px, margin-top 16px
   - Text: "🚫 Submission Blocked — Candidate has been rejected. 
     This record cannot be submitted."
   - Color: danger, font DM Sans 14px
   - id="rejection-banner", display:none by default

E) SUBMIT BUTTON (margin-top 24px):
   - width: 100%
   - height: 52px
   - background: var(--accent)
   - border: none
   - border-radius: 10px
   - color: white
   - font-family: Syne
   - font-size: 15px
   - font-weight: 600
   - cursor: pointer
   - transition: all 0.2s ease
   - Text: "Submit Candidate Record"
   
   Disabled state:
   - opacity: 0.4
   - cursor: not-allowed
   - text changes to: "Complete all required fields to submit"
   - Start as disabled (disabled attribute on button)
   
   Hover state (when enabled):
   - background: var(--accent-hover)
   - transform: translateY(-1px)

   Below button, centered:
   - Small text: "⌘ Ctrl+Enter to submit" 
   - 11px, color text-muted, margin-top 8px

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PANEL 2 — AUDIT LOG (hidden by default)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Show this placeholder for now:

A centered div with:
- Icon: 📋 at 48px font size
- Title: "Audit Log" Syne 700 24px
- Subtitle: "Coming in Sprint 3 — all submitted records will appear here."
  DM Sans 14px text-secondary
- margin-top: 80px, text-align: center

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PANEL 3 — RULES CONFIG (hidden by default)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Show this placeholder for now:

A centered div with:
- Icon: ⚙️ at 48px font size
- Title: "Rules Configuration" Syne 700 24px
- Subtitle: "Coming in Sprint 3 — edit eligibility thresholds here."
  DM Sans 14px text-secondary
- margin-top: 80px, text-align: center

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FOOTER
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

- text-align: center
- padding: 24px
- font-family: DM Sans, 12px
- color: text-muted
- Text: "AdmitGuard v1.0 · Built with Google AI Studio · Data stored locally"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
JAVASCRIPT — this sprint only
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. TAB SWITCHING:
   Three buttons control three panels.
   On tab click: hide all panels, show clicked panel,
   update active tab styling.
   Default: Panel 1 visible, "New Entry" tab active.

2. PROGRESS BAR:
   Count how many of the 11 input/select fields 
   have a non-empty value.
   Progress = (filledCount / 11) * 100
   Update the bar width and percentage label on 
   every input and change event on any field.

3. SCORE CARD REACTIVE UPDATE:
   On every field input/change event, count filled fields
   and update the eligibility score display as:
   score = Math.round((filledCount / 11) * 100)
   Update the score number and its color class.
   Update mini chip values with current counts.

4. SCORE MODE TOGGLE:
   Two buttons for "%" and "CGPA" above the score field.
   On click: update scoreMode variable,
   update button active styles,
   update input placeholder text.

5. OFFER LETTER LOCK:
   On Interview Status change:
   - If value is NOT "Cleared": disable offer letter select,
     add visual opacity 0.5 to its wrapper
   - If value IS "Cleared": enable offer letter select,
     remove opacity

6. REJECTION BANNER:
   On Interview Status change:
   - If value is "Rejected": show rejection-banner div,
     disable submit button
   - Otherwise: hide rejection-banner div

7. PAGE LOAD ANIMATION:
   On DOMContentLoaded:
   The panel header and score card animate in with:
   opacity: 0 → 1, transform: translateY(10px) → translateY(0)
   Duration: 0.4s ease
   Use CSS classes toggled by JS after a 50ms delay

8. KEYBOARD SHORTCUT:
   Listen for Ctrl+Enter keydown on document.
   If submit button is not disabled, click it.

Output the complete single index.html file.
Start with <!DOCTYPE html> and end with </html>.
Do not add any explanation before or after the code.
Just the raw HTML file content.