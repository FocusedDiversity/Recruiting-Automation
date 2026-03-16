# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **no-code/low-code** recruiting automation project for Synaptiq (synaptiq.ai). There is no application code to build or test — the system is built on connected SaaS tools. Work in this repo consists of updating documentation, prompt templates, and configuration guides.

## How to Resume With a Returning User

When Rich starts a new session, read `docs/project-plan.md` first. It contains the full project context, all key decisions, resource IDs, and a session log showing exactly where work left off.

The standard resume prompt is:
> "I'm continuing work on my Recruiting Automation project. Here is my project plan: https://github.com/FocusedDiversity/Recruiting-Automation/blob/main/docs/project-plan.md — please review it and help me pick up where I left off."

## Architecture

The system connects four external services via Make.com automation:

```
Google Form → Make.com → Google Drive + Google Sheets + Claude API
```

1. **Google Form** — candidate intake (file uploads for resume + cover letter)
2. **Make.com** — automation glue; triggers on form submission, orchestrates all downstream steps
3. **Google Drive** — stores candidate documents (PDF originals + Google Docs versions)
4. **Google Sheets** — master candidate tracker with LLM scores and rank ordering
5. **Claude API** — evaluates each candidate, returns structured JSON scores

## Key Resource IDs

These are needed when configuring Make.com scenarios:

| Resource | ID / URL |
|---|---|
| Google Form | https://forms.gle/inZtmdSQJRjbih2b7 |
| Master Tracker (Google Sheet) | `1HqsXvRox7SElPDbo658sgZO_y9981-OaWVpSqUAnm_4` |
| GDrive `_Automation` folder | `112ubR99hjiofzA0UzrQe9AXjywV0_P4g` |
| GDrive `Candidates` folder (test role) | `1Qh1yRfaX6L1XHJAn72Hqb0BQ3y-i2Hv7` |

## Google Drive Structure

```
07 - Recruiting (Shared Drive)
  _Automation/
    _Templates/          ← Synaptitudes, company brief, tracker template
    Active Roles/
      [Role Title] - [YYYY-MM-DD]/
        _Job Description/
        Candidates/
          [LastName, FirstName]/
            Resume (PDF + Google Docs version)
            Cover Letter (PDF + Google Docs version)
            LLM Evaluation (Google Doc)
```

## LLM Evaluation Scoring

Candidates are scored 1–5 on each dimension where **1 = best fit, 5 = worst fit**:
- **Job Match** — alignment with role requirements
- **Cultural Fit** — alignment with Synaptitudes
- **Career Trajectory** — is this role appropriate for where they are in their career?
- **Overall Rating** — composite score

Claude returns a structured JSON object. See `prompts/candidate-evaluation.md` for the full prompt template and expected JSON schema.

## Google Sheets Tracker Column Order

A (Candidate ID) | B (Rank) | C (Timestamp) | D (Role) | E (First Name) | F (Last Name) | G (Email) | H (Phone) | I (Location) | J (LinkedIn URL) | K (GitHub URL) | L (Resume PDF) | M (Cover Letter PDF) | N (Candidate Folder) | O (Job Match 1-5) | P (Cultural Fit 1-5) | Q (Career Trajectory 1-5) | R (Overall Rating 1-5) | S (Strengths) | T (Risks) | U (Fit Summary) | V (Interview Questions) | W (Decision) | X (Stage) | Y (Hiring Manager Notes)

## Pending Items (as of last session)

- Anthropic API key: not yet obtained (needed for Make.com → Claude API connection)
- Make.com scenario: not yet built (Step 6)
- Claude evaluation prompt: not yet finalized (Step 5)
- Application form: additional fields noted in `_Scratch/Job Application Form Notes.md` not yet added (work authorization, visa sponsorship, start date, expected salary)

## Repository Structure

- `docs/project-plan.md` — master project plan and session log (primary reference)
- `docs/setup.md` — step-by-step MVP setup guide
- `prompts/candidate-evaluation.md` — Claude API prompt template
- `make-scenarios/README.md` — Make.com scenario build guide
- `sheets/candidate-tracker-columns.md` — tracker column spec
- `_Scratch/` — Rich's working notes (not committed to main project docs yet)
