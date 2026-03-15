# Make.com Scenario Guide

## Overview

One Make scenario handles the full intake-to-evaluation pipeline when a candidate submits a Tally form.

## Trigger
**Tally — Watch Responses** (webhook trigger)

## Scenario Steps

### Module 1: Tally Webhook
- Trigger: New form submission
- Outputs: All form fields + file upload URLs

### Module 2: Google Drive — Create Folder
- Location: `Recruiting/[Role Name] - [Date]/Candidates/`
- Folder name: `[LastName, FirstName]`

### Module 3: Google Drive — Upload Resume
- Download file from Tally upload URL
- Upload to candidate folder
- Output: File ID + shareable link

### Module 4: Google Drive — Upload Cover Letter
- Same as Module 3 for cover letter

### Module 5: Google Sheets — Add Row
- Sheet: Candidate tracker for the role
- Populate: Timestamp, Name, Email, Phone, LinkedIn, file links, folder link

### Module 6: Google Drive — Get File Content (Resume)
- Read resume text content for LLM input

### Module 7: Google Drive — Get File Content (Cover Letter)
- Read cover letter text content for LLM input

### Module 8: HTTP — Claude API Call
- URL: `https://api.anthropic.com/v1/messages`
- Method: POST
- Headers:
  - `x-api-key: {{your_anthropic_api_key}}`
  - `anthropic-version: 2023-06-01`
  - `content-type: application/json`
- Body: See `prompts/candidate-evaluation.md` for prompt structure

### Module 9: JSON — Parse Response
- Parse Claude API response to extract evaluation JSON

### Module 10: Google Sheets — Update Row
- Update the candidate row with LLM evaluation fields
- (Score, Summary, Strengths, Concerns, Questions, Decision)

### Module 11: Google Drive — Create Evaluation Doc
- Create a Google Doc in the candidate folder
- Name: `LLM Evaluation - [Candidate Name]`
- Content: Formatted version of the LLM output

## Tips
- Use Make's error handling to catch API failures
- Store your Anthropic API key in Make's connections/secrets vault
- Test with a sample submission before going live
