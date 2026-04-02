# Sprint Log — AdmitGuard

---

## Sprint 0 — Project Setup
**Date:** April 2025
**Status:** ✅ Complete

### What was done
- Created GitHub repository: Admitguard-Fts
- Set up full folder structure (src, prompts, config, docs)
- Wrote research-notes.md covering the admission pipeline problem
- Wrote README.md with project overview
- Initialized Git and connected to remote origin

### Key decisions
- Single file architecture (index.html) — no build tools, no dependencies
- Dark theme chosen for professional operator-facing tool aesthetic
- localStorage for audit persistence — no backend required

### Commits
- sprint-0: project setup and research notes

---

## Sprint 1 — Form Structure & Strict Validation
**Date:** April 2025
**Status:** ✅ Complete

### What was done
- Built complete single-file index.html with dark design system
- Implemented all 11 form fields in 2-column grid layout
- Added fixed navigation bar with 3 panel tabs
- Added eligibility score card with 4 mini metric chips
- Added real-time progress bar tracking form completion
- Implemented all 7 strict validation rules:
  - Name: min 2 chars, letters only
  - Email: valid format + duplicate detection
  - Phone: 10 digits starting with 6–9
  - Aadhaar: exactly 12 digits
  - Qualification: dropdown selection required
  - Interview: Rejected status blocks submission entirely
  - Offer Letter: locked until interview is cleared
- Added inline error/success states on all fields
- Added rejection banner when Interview = Rejected
- Added keyboard shortcut Ctrl+Enter to submit

### Key decisions
- Debounced validation on input (300ms) + immediate on blur
- Score mode toggle (% vs CGPA) on academic score field
- Offer Letter field conditionally locked based on interview status

### Commits
- sprint-1: base form with 11 fields and layout
- sprint-1: strict validation and updated prompt files

---

## Sprint 2 — Soft Rules & Exception System
**Date:** April 2025
**Status:** ✅ Complete

### What was done
- Added 4 soft rule validations:
  - Age: 18–35 (overridable)
  - Graduation Year: 2015–2025 (overridable)
  - Academic Score: ≥60% or ≥6.0 CGPA (overridable)
  - Screening Test Score: ≥40/100 (overridable)
- Built exception toggle UI per soft rule violation
- Built rationale textarea with live character counter
- Implemented keyword detection system:
  - Required phrases: "approved by", "special case",
    "documentation pending", "waiver granted"
  - Keyword chips light up green when phrase detected
- Added manager flag trigger when exceptions > 2
- Eligibility score engine deducts points per violation type
- Submit button enables only when all rules pass or
  valid exceptions are in place

### Key decisions
- Exception is only valid when BOTH conditions met:
  ≥30 characters AND at least one keyword detected
- Flag threshold set at >2 exceptions per submission
- Score deduction: strict violation -12pts, 
  soft violation -4pts, soft with exception -1pt

### Commits
- sprint-2: soft rules with exception system

---

## Sprint 3 — Audit Trail, Storage & Rules Config
**Date:** April 2025
**Status:** ✅ Complete

### What was done
- Built full form submission flow with post-submit overlay
- Implemented localStorage persistence for all records
- Aadhaar masking before storage (shows XXXX-XXXX-XXXX)
- Built complete Audit Log panel:
  - Search and filter records
  - Sort by date, score, status
  - Full record detail modal
  - CSV and JSON export
  - Analytics dashboard (5 metric cards)
- Built Rules Config panel:
  - Editable soft rule thresholds
  - Exception config editor
  - Read-only strict rules display
  - Live rule updates without code changes
- Added draft auto-save every 5 seconds
- Added draft restore banner on page reload
- Added toast notification system
- Handled all edge cases:
  - localStorage unavailable fallback
  - Duplicate email detection
  - Input sanitization (trim, lowercase email)
  - Aadhaar all-same-digit rejection
  - 500+ record warning

### Key decisions
- Audit records tagged with cohort and rules version
- crypto.randomUUID() for unique record IDs
- CSV filename includes date: admitguard-audit-YYYY-MM-DD.csv

### Commits
- sprint-3: configurable rules engine and edge case handling

---

## Sprint 4 — Final Integration & Deployment
**Date:** April 2025
**Status:** ✅ Complete

### What was done
- Ran all 5 integration test flows successfully
- Removed all console.log statements
- Cleaned up duplicate CSS rules
- Added proper meta tags and page title
- Added Help modal with usage guide and field rules reference
- Wrote complete README.md
- Took app screenshot for docs/wireframe.png
- Deployed to GitHub Pages

### Live URL
https://krushnakoushik.github.io/Admitguard-Fts/src/

### Commits
- sprint-4: final integration and README
- sprint-4: deployment prep and documentation