# Recruiting Automation

AI-powered recruiting framework for end-to-end candidate screening and management.

## Overview

This system automates the recruiting pipeline using LLMs to screen applicants, organize candidate materials in Google Drive, and surface ranked evaluations to hiring managers.

## Architecture

- **Application Form:** Tally.so (per role)
- **Automation:** Make.com
- **File Storage:** Google Drive
- **Candidate Tracking:** Google Sheets
- **LLM Evaluation:** Claude API

## Repository Structure

```
Recruiting-Automation/
  prompts/          # LLM evaluation prompt templates
  make-scenarios/   # Make.com scenario documentation & JSON exports
  sheets/           # Google Sheets templates & column specs
  docs/             # Process documentation & setup guides
```

## Setup Guide

See `docs/setup.md` for step-by-step instructions to stand up the MVP.

## Workflow

1. Candidate submits Tally form (resume + cover letter + info)
2. Make.com automation triggers:
   - Creates candidate folder in Google Drive
   - Adds candidate row to tracking Sheet
   - Sends materials to Claude API for evaluation
   - Writes LLM evaluation back to Drive + Sheet
3. Hiring manager reviews ranked candidates in Google Sheet
