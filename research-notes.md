# Research Notes — AdmitGuard

**Author:** Koushik  
**Date:** April 2025  

---

## The Problem

The current admission pipeline at FTS uses an unstructured Google Sheet 
for candidate data entry. There is zero validation at the point of entry — 
anyone can type anything into any field.

This causes a critical operational failure:

- Ineligible candidates enter the pipeline undetected
- Errors compound silently across 5 stages — Counselor Verification, 
  Screening Test, Interview, Document Verification, Final Enrollment
- Ineligible candidates are only caught at Document Verification — 
  after both the company and candidate have already invested significant time
- This wastes 40+ hours per cohort on candidates who should never 
  have passed the first stage

---

## Business Impact

| Problem | Impact |
|---------|--------|
| No validation at entry | Ineligible candidates enter freely |
| Late-stage rejection | 15% rejection rate at doc verification |
| No exception tracking | Zero accountability for overrides |
| No audit trail | Infinite compliance risk with institutional partners |
| Rules change per cohort | Excel makes updates painful and error-prone |

---

## Root Causes Identified

1. **No enforcement at entry** — Google Sheets accepts any value in any field
2. **Knowledge gap** — Counselors and ops staff don't always know 
   the exact IIT eligibility criteria
3. **Silent compounding** — Invalid data moves through the pipeline 
   without triggering any alert
4. **No exception documentation** — Borderline cases get approved verbally 
   with no written record of who approved what and why
5. **Rigid update process** — Changing eligibility rules requires editing 
   the spreadsheet manually, risking formula breaks

---

## Proposed Solution

Replace the Google Sheet with a form-based web application that:

- Enforces strict eligibility rules at the point of data entry
- Prevents submission until all mandatory conditions are satisfied
- Handles edge cases through a structured exception system
- Stores all rules in a configurable JSON file — no code changes needed
- Logs every submission with full audit trail including exceptions used

---

## Key Insights from Analysis

- The problem is not the people — it is the tool. Excel was never 
  designed for enforced data validation in an operational pipeline.
- Two types of rules are needed: strict (zero tolerance) and soft 
  (overridable with documented rationale)
- The exception system is as important as the validation itself — 
  borderline cases exist in every cohort and need a legitimate path forward
- Audit trail is non-negotiable for institutional compliance

---

## References

- Project brief: AdmitGuard_Project_Walkthrough.pptx
- Admission pipeline stages: Application → Counselor Verification → 
  Screening Test → Interview → Doc Verification → Final Enrollment
- Eligibility criteria: Age 18–35, Grad Year 2015–2025, 
  Score ≥60% or CGPA ≥6.0, Test Score ≥40/100