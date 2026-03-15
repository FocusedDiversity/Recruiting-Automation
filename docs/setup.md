# MVP Setup Guide

## Prerequisites
- Google Workspace account (Drive + Sheets)
- Tally.so account (free tier works)
- Make.com account (free tier works for MVP)
- Anthropic API key (claude.ai/api)

## Step 1: Google Drive Structure

Manually create the following folder structure in Google Drive:

```
Recruiting/
  [Role A Title] - [Start Date]/
    _Job Description.pdf
    _Synaptitudes.pdf
    Candidates/
  [Role B Title] - [Start Date]/
    _Job Description.pdf
    _Synaptitudes.pdf
    Candidates/
```

## Step 2: Google Sheets Tracker

Create one Sheet per role. See `sheets/candidate-tracker-columns.md` for the full column spec.

## Step 3: Tally Application Form

Create one form per role. Required fields:
- Full Name
- Email
- Phone
- LinkedIn Profile URL
- Resume (file upload)
- Cover Letter (file upload)
- Hidden field: Role (set to role name)

## Step 4: Make.com Scenario

See `make-scenarios/README.md` for the full scenario build guide.

## Step 5: Claude API

- Get API key at console.anthropic.com
- See `prompts/candidate-evaluation.md` for the evaluation prompt template
- Recommended model: `claude-sonnet-4-6`
