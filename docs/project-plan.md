# Recruiting Automation — Project Plan

**Last Updated:** 2026-03-15
**Status:** MVP In Progress — Step 2 Complete

---

## How to Resume This Project With Claude

Share this file at the start of any new session:
> "Here is my project plan: [paste contents or link to this file]. Please review and help me continue from where I left off."

---

## Company Context

- **Company:** Synaptiq (synaptiq.ai)
- **Size:** ~10 people, growing to ~30 in 18 months
- **Hiring Manager (MVP):** Rich Manegio
- **Google Workspace:** Active (Drive + Sheets in use)

---

## Problem We Are Solving

Synaptiq has no formal recruiting process today. Hiring has been ad-hoc and manual. We want a scalable, repeatable, AI-powered system that lets us quickly screen, identify, and hire the best candidates — ideally within a two-week cycle.

---

## Full Vision (All Phases)

The end-state system will:
- Accept applications via synaptiq.ai/careers
- Ingest and organize all candidate materials in Google Drive automatically
- Screen candidates using LLM evaluation against job requirements and Synaptitudes
- Maintain a live, rank-ordered candidate list across all roles
- Generate standard + customized interview questions per candidate
- Track candidates through all interview stages with rolling interview notes
- Offer candidates an optional AI-powered screening interview
- Communicate proactively with candidates throughout the process
- Automate offer letters and next-step communications

---

## MVP Scope

**The core loop: Application In → Organized in Drive → AI Evaluation Out → Hiring Manager Decides**

### In Scope
- Candidate submits application via form (name, email, phone, LinkedIn URL, GitHub URL, resume, cover letter)
- Materials auto-filed in Google Drive with structured folder hierarchy
- Documents stored in original format AND converted to Google Docs
- Candidate auto-added to Google Sheets tracking database
- Claude evaluates each candidate and scores them on:
  - **Job Match** (1–5): How well the candidate meets role requirements
  - **Cultural Fit** (1–5): Alignment with Synaptitudes and team culture
  - **Career Trajectory** (1–5): Is this role the right fit for where they are in their career? Under/over/well-qualified?
  - **Overall Rating** (1–5): Composite rating
- Qualitative output per candidate: strengths, risks, overall fit summary
- Custom interview questions generated per candidate (standard + role-specific)
- Consolidated ranked candidate list, auto-updated as new applicants arrive
- System is role-agnostic: roles, job descriptions, and criteria are inputs, not hardcoded

### Out of Scope (Post-MVP)
- Candidate-facing communications (acknowledgements, status updates)
- Automated offer letters
- AI screening interview (third-party)
- Multi-stage interview tracking and rolling interview notes
- Job posting automation
- Website-embedded application form (synaptiq.ai/careers)
- Automated scheduling (Google Scheduler)

---

## Tech Stack

| Component | Tool | Notes |
|---|---|---|
| Application form | Tally.so | Free tier; supports file uploads; easy Make.com integration |
| Automation | Make.com | Connects all tools; handles file routing + API calls |
| File storage | Google Drive | Already in use |
| Candidate database | Google Sheets | One master sheet + one sheet per role |
| LLM Evaluation | Claude API | Model: `claude-sonnet-4-6`; long context, cost-effective |

---

## Google Drive Folder Structure

```
Recruiting/
  _Templates/
    _Job Description Template.docx
    _Synaptitudes.pdf
    _Candidate Tracker Template (Sheet)
  [Role Title] - [YYYY-MM-DD]/
    _Job Description - [Role Title].pdf
    _Synaptitudes.pdf
    _Candidate Tracker (link to Sheet)
    Candidates/
      [LastName, FirstName]/
        Resume - [LastName, FirstName].pdf
        Resume - [LastName, FirstName].gdoc        ← Google Docs version
        Cover Letter - [LastName, FirstName].pdf
        Cover Letter - [LastName, FirstName].gdoc  ← Google Docs version
        LLM Evaluation - [LastName, FirstName].gdoc
```

---

## Google Sheets Structure

### Master Tracker (all roles, all candidates)
One sheet to rule them all — rank-ordered by Overall Rating.

### Per-Role Sheet
Filtered view of the master for each open role.

### Columns

| Column | Source |
|---|---|
| Rank | Auto-calculated (sorted by Overall Rating) |
| Timestamp | Auto (Make) |
| Role | Form / Make |
| Full Name | Form |
| Email | Form |
| Phone | Form |
| LinkedIn URL | Form |
| GitHub URL | Form |
| Resume Link | GDrive (Make) |
| Cover Letter Link | GDrive (Make) |
| Candidate Folder Link | GDrive (Make) |
| Job Match Score (1–5) | Claude API |
| Cultural Fit Score (1–5) | Claude API |
| Career Trajectory Score (1–5) | Claude API |
| Overall Rating (1–5) | Claude API |
| Strengths | Claude API |
| Risks / Concerns | Claude API |
| Fit Summary | Claude API |
| Interview Questions | Claude API |
| Stage | Manual |
| Hiring Manager Notes | Manual |

---

## LLM Evaluation Output (JSON)

```json
{
  "job_match_score": 2,
  "cultural_fit_score": 1,
  "career_trajectory_score": 2,
  "overall_rating": 2,
  "strengths": ["...", "..."],
  "risks": ["...", "..."],
  "fit_summary": "...",
  "standard_interview_questions": ["...", "...", "..."],
  "custom_interview_questions": ["...", "...", "..."],
  "decision": "Advance | Hold | Pass",
  "decision_rationale": "..."
}
```

**Scoring key:** 1 = Best fit, 5 = Worst fit

---

## MVP Workflow

```
Candidate visits job posting
  → Submits Tally form (info + resume + cover letter)
  → Make.com triggers automatically:
      1. Creates candidate folder in GDrive
      2. Uploads files (PDF + converts to Google Docs)
      3. Adds row to Google Sheets tracker
      4. Sends materials to Claude API for evaluation
      5. Writes scores + summary back to Sheet
      6. Creates LLM Evaluation doc in GDrive candidate folder
      7. Re-sorts master tracker by Overall Rating
  → Rich opens tracker, reviews ranked list, reads summaries
  → Decides: Advance / Hold / Pass
```

---

## Build Order

### Step 1 — Google Drive Structure ⬜
- [ ] Create `Recruiting/` top-level folder
- [ ] Create `_Templates/` subfolder with Synaptitudes + tracker template
- [ ] Confirm naming convention

### Step 2 — Google Sheets Tracker ⬜
- [ ] Create Master Tracker sheet with all columns
- [ ] Set up rank-ordering formula
- [ ] Create template to duplicate per role

### Step 3 — Tally Application Form ⬜
- [ ] Create one master form (role-agnostic)
- [ ] Fields: Name, Email, Phone, LinkedIn URL, GitHub URL, Resume upload, Cover Letter upload, Role applying for
- [ ] Connect Tally webhook to Make.com

### Step 4 — Claude Evaluation Prompt ⬜
- [ ] Write system prompt (company context + Synaptitudes)
- [ ] Write user prompt template with variable placeholders
- [ ] Test manually with sample resume + cover letter
- [ ] Iterate until output quality is solid
- [ ] Finalize JSON output structure

### Step 5 — Make.com Automation Scenario ⬜
- [ ] Set up Make.com account
- [ ] Connect Tally webhook trigger
- [ ] Module: Create GDrive candidate folder
- [ ] Module: Upload + convert files to Google Docs
- [ ] Module: Add row to Google Sheets
- [ ] Module: Extract file text for Claude
- [ ] Module: Call Claude API
- [ ] Module: Parse JSON response
- [ ] Module: Update Sheet with scores + summary
- [ ] Module: Create LLM Evaluation doc in GDrive
- [ ] Test end-to-end with a sample submission

---

## Context Documents Needed for Claude Prompt

- [ ] Synaptitudes (cultural tenets with full descriptions)
- [ ] Company brief (what Synaptiq does, types of projects, team culture)
- [ ] Job description template (role-agnostic structure)
- [ ] Evaluation rubric (what does a great Synaptiq candidate look like, regardless of role?)

---

## Open Questions

| Question | Status |
|---|---|
| Application form: Tally.so vs embedded on synaptiq.ai/careers | ⬜ Tally for MVP, website for Phase 2 |
| Candidate DB: Google Sheets vs Airtable | ⬜ Pending decision |
| Make.com account: exists or needs setup? | ⬜ Pending |
| Anthropic API key: exists or needs setup? | ⬜ Pending |
| Scoring direction confirmed: 1 = best, 5 = worst | ✅ Confirmed |
| System is role-agnostic (role/JD are inputs) | ✅ Confirmed |
| Specific roles for MVP | ✅ Not needed — system-first approach |

---

## Google Drive Folder IDs

> These are needed for Make.com configuration.

| Folder | ID |
|---|---|
| `_Automation` (root) | `112ubR99hjiofzA0UzrQe9AXjywV0_P4g` |
| `Candidates` (inside Data Engineer (Test) - 2026-03-15) | `1Qh1yRfaX6L1XHJAn72Hqb0BQ3y-i2Hv7` |

**Full path in Shared Drive:**
`07 - Recruiting > _Automation > Active Roles > [Role Title] - [YYYY-MM-DD] > Candidates`

---

## Session Log

### 2026-03-15 — Session 1
**Decisions made:**
- MVP scope defined: core loop only (application → Drive → Sheet → LLM evaluation → ranked list)
- Tech stack selected: Tally + Make.com + GDrive + Google Sheets + Claude API
- Scoring model: 1–5 scale (1 = best) across Job Match, Cultural Fit, Career Trajectory, Overall Rating
- System must be role-agnostic; roles and job descriptions are inputs
- Ranked candidate list is in MVP scope
- Post-MVP: candidate communications, AI interview, scheduling, offer letters
- GitHub repo created: github.com/FocusedDiversity/Recruiting-Automation

**Completed:**
- GitHub repo initialized and pushed
- Initial project structure created (prompts/, make-scenarios/, sheets/, docs/)
- Project plan written (this document)

**Next session should start at:** Step 3 — Google Sheets Master Tracker

### 2026-03-15 — Session 1 (continued)
**Completed:**
- Step 2: Google Drive folder structure created in corporate shared drive
- Folder IDs captured for Make.com use
- Naming convention confirmed: `[Role Title] - [YYYY-MM-DD]` for roles, `LastName, FirstName` for candidates
