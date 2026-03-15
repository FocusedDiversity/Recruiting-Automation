# Recruiting Automation — Project Plan

**Last Updated:** 2026-03-15
**Status:** MVP In Progress

---

## Goal

Build an AI-powered recruiting system that automates candidate intake, file organization, and LLM-based screening — enabling hiring managers to quickly identify and prioritize the best candidates across open roles.

---

## Background

Synaptides is standing up a structured, repeatable recruiting process powered by LLMs. The system should:
- Accept applications via a clean web form
- Automatically organize candidate materials in Google Drive
- Use Claude to evaluate each applicant against role requirements and cultural fit (Synaptitudes)
- Surface ranked results to the hiring manager in a single tracking view

---

## MVP Scope

**In scope:**
- 2 open roles
- Application form per role (Tally.so)
- Automated GDrive folder creation per candidate
- Candidate tracking via Google Sheets
- LLM evaluation (Claude API) triggered on each new application
- LLM output written to Sheet + candidate GDrive folder

**Out of scope (post-MVP):**
- Candidate-facing email acknowledgements
- Google Scheduler integration for self-serve interview booking
- Multi-stage interview tracking
- Custom-built application form (website-embedded)
- Automated reference checks or assignment workflows

---

## Tech Stack

| Component | Tool | Notes |
|---|---|---|
| Application form | Tally.so | Free tier; supports file uploads |
| Automation | Make.com | More powerful than Zapier for multi-step flows |
| File storage | Google Drive | Already in use (Google Workspace) |
| Candidate DB | Google Sheets | One sheet per role |
| LLM Evaluation | Claude API (Sonnet) | `claude-sonnet-4-6`; long context, cost-effective |

---

## Full Workflow (Target State)

```
Planning & Inputs
  → Configure LLM & Post Job (company website, LinkedIn, Indeed)
  → Candidate Submits Application (Tally form)
  → Send Acknowledgement Email
  → Store Applicant in Tracking Sheet
  → Store Application Documents in GDrive
  → Perform LLM Evaluation
  → Update Tracking Sheet with Results
  → Hiring Manager Reviews & Prioritizes
  → Contact Candidates to Schedule Interviews (Google Scheduler)
```

### Interview Stages
1. LLM Screening (automated)
2. AI Screening Interview (optional)
3. Interview #1 — Synaptitudes / General Fit
4. Interview #2 — Technical / Functional Fit
5. Assignment
6. Final Decision: Hire / Save for Future / Not Interested

---

## Google Drive Folder Structure

```
Recruiting/
  [Role Title] - [Start Date]/
    _Job Description.pdf
    _Synaptitudes.pdf
    _Candidate Tracker (link to Sheet)
    Candidates/
      [LastName, FirstName]/
        Resume - [LastName, FirstName].pdf
        Cover Letter - [LastName, FirstName].pdf
        LinkedIn - [LastName, FirstName].pdf (optional)
        LLM Evaluation - [LastName, FirstName].md
```

---

## Google Sheets Tracker Columns

| Column | Source |
|---|---|
| Timestamp | Auto (Make) |
| Full Name | Form |
| Email | Form |
| Phone | Form |
| LinkedIn URL | Form |
| Resume Link | GDrive (Make) |
| Cover Letter Link | GDrive (Make) |
| Candidate Folder Link | GDrive (Make) |
| LLM Score (1-10) | Claude API |
| LLM Summary | Claude API |
| Strengths | Claude API |
| Concerns | Claude API |
| Synaptitudes Alignment | Claude API |
| Recommended Questions | Claude API |
| LLM Decision | Claude API |
| Stage | Manual |
| Hiring Manager Notes | Manual |

---

## LLM Evaluation Output Structure

Claude returns a structured JSON object per candidate:

```json
{
  "overall_score": 8,
  "summary": "...",
  "strengths": ["...", "..."],
  "concerns": ["...", "..."],
  "synaptitudes_alignment": { "score": 7, "notes": "..." },
  "technical_fit": { "score": 8, "notes": "..." },
  "recommended_questions": ["...", "...", "..."],
  "decision": "Advance | Hold | Pass",
  "decision_rationale": "..."
}
```

---

## Build Order (MVP)

### Step 1 — Google Drive Structure ✅ Designed / ⬜ Implemented
- [ ] Create `Recruiting/` top-level folder
- [ ] Create Role A subfolder with `_Job Description`, `_Synaptitudes`, `Candidates/`
- [ ] Create Role B subfolder (same structure)

### Step 2 — Google Sheets Tracker ⬜
- [ ] Create tracker sheet for Role A
- [ ] Create tracker sheet for Role B
- [ ] Add all columns per spec (see above)

### Step 3 — Tally Application Form ⬜
- [ ] Build form for Role A
- [ ] Build form for Role B
- [ ] Fields: Name, Email, Phone, LinkedIn, Resume upload, Cover Letter upload

### Step 4 — Make.com Automation Scenario ⬜
- [ ] Connect Tally webhook trigger
- [ ] Create GDrive candidate folder
- [ ] Upload resume + cover letter to folder
- [ ] Add row to Google Sheet
- [ ] Extract file text content
- [ ] Call Claude API with evaluation prompt
- [ ] Parse JSON response
- [ ] Update Sheet with LLM results
- [ ] Create LLM Evaluation doc in GDrive

### Step 5 — Claude Evaluation Prompt ⬜
- [ ] Finalize system prompt with company brief + Synaptitudes
- [ ] Finalize user prompt template with variable placeholders
- [ ] Test manually with sample resumes before wiring into Make
- [ ] Iterate until output quality is satisfactory

---

## Context Documents Needed

These will be loaded into the Claude system prompt to ground evaluations:

- [ ] Job Description — Role A
- [ ] Job Description — Role B
- [ ] Synaptitudes (cultural tenets with descriptions)
- [ ] Company Brief (distilled from website: what Synaptides does, project types, team culture)
- [ ] Evaluation Rubric per role (what does a great candidate look like?)

---

## Open Questions / Decisions

| Question | Status |
|---|---|
| Which 2 roles are in MVP scope? | ⬜ To decide |
| Low-code (Make) vs. custom code for automation? | ✅ Make.com for MVP |
| Will AI screening interview be in MVP? | ✅ Out of scope for MVP |
| Email acknowledgement automation — MVP or post? | ✅ Post-MVP |
| Where will Tally forms be linked from? (Job postings, website, etc.) | ⬜ To decide |

---

## Repository Structure

```
Recruiting-Automation/
  docs/
    project-plan.md       ← this file
    setup.md              ← step-by-step setup guide
  prompts/
    candidate-evaluation.md   ← Claude prompt template
  make-scenarios/
    README.md             ← Make.com scenario build guide
  sheets/
    candidate-tracker-columns.md  ← Column spec for Google Sheets
```
