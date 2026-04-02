# AdmitGuard — Admission Data Validation & Compliance System

## Problem Statement
The admission process for high-volume education programs often suffers from data integrity issues and manual compliance bottlenecks. Operators frequently enter incorrect candidate details (e.g., invalid Aadhaar formats, out-of-range ages), leading to downstream processing failures and legal risks.

Furthermore, the lack of a standardized exception handling mechanism means that "special cases" are often approved without proper documentation or audit trails. This results in a "black box" pipeline where business impact is hard to measure and compliance is nearly impossible to verify in real-time.

## Solution
AdmitGuard provides a professional, real-time validation and compliance layer for admission operators. By enforcing 7 strict hardcoded rules and 4 configurable soft rules, it catches errors at the source.

The system features a robust exception engine that requires operators to provide rationales with specific keywords for any rule overrides. All actions are logged in a persistent audit trail, providing full transparency and a manager-flagging system for high-risk submissions.

## Features
- **Real-time form validation:** 7 strict rules (blocked) and 4 soft rules (overridable).
- **Exception handling system:** Keyword-enforced rationales for all rule overrides.
- **Configurable rules engine:** Update thresholds for age, scores, and years without code changes.
- **Full audit trail:** Persistent storage via `localStorage` with search, filter, and sort capabilities.
- **Export capabilities:** One-click CSV and JSON export for reporting.
- **Manager flag system:** Automatic flagging for submissions with high exception counts.
- **Mobile responsive:** Fully optimized for desktop and mobile devices.

## How to Run
1. Clone this repository.
2. Open `index.html` in any modern web browser (Chrome, Firefox, Safari, Edge).
3. No server or build step required — the application is fully client-side.

## How to Deploy (GitHub Pages)
1. Push the repository to GitHub.
2. Go to **Settings** → **Pages**.
3. Under **Build and deployment**, set **Source** to "Deploy from a branch".
4. Select the `main` branch and `/ (root)` folder.
5. Your app will be live at: `https://[username].github.io/[repo-name]`

## Rules Configuration
Use the built-in **Rules Config** panel to modify:
- **Age Range:** Minimum and maximum eligibility ages.
- **Graduation Year:** Valid range for passing years.
- **Academic Score:** Minimum Percentage or CGPA thresholds.
- **Screening Test:** Minimum assessment score.
- **Exception Keywords:** Manage the list of required keywords for rationales.

## Validation Rules Reference
| Field Name | Rule Type | Requirement | Override? |
| :--- | :--- | :--- | :--- |
| Full Name | Strict | Min 2 chars, No numbers | No |
| Email | Strict | Valid format, Unique | No |
| Phone | Strict | 10 digits, starts 6-9 | No |
| Aadhaar | Strict | 12 digits, No same digits | No |
| Qualification | Strict | Required selection | No |
| Age | Soft | 18–35 years | Yes |
| Grad Year | Soft | 2015–2025 | Yes |
| Academic Score | Soft | ≥60% or ≥6.0 CGPA | Yes |
| Screening Test | Soft | ≥40 out of 100 | Yes |
| Interview Status | Strict | Not "Rejected" | No |
| Offer Letter | Strict | Only if Interview Cleared | No |

## Tech Stack
- **Pure HTML/CSS/JavaScript:** Zero external dependencies or frameworks.
- **Google Fonts:** Syne (Headings) and DM Sans (Body).
- **Persistence:** Browser `localStorage` for records and configuration.
- **Built using Google AI Studio:** Powered by Gemini.

## Sprint Log
- **Sprint 1:** Core UI layout, design system, and basic form structure.
- **Sprint 2:** Real-time validation engine and strict/soft rule logic.
- **Sprint 3:** Exception handling system and rationale validation.
- **Sprint 4:** Audit log implementation with search, filter, and export.
- **Sprint 5:** Rules Configuration panel and live threshold updates.
- **Sprint 6 (Final):** UX polish, help system, draft persistence, and deployment prep.
