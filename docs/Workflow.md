# LeadFlow Workflow

## Overview

LeadFlow combines a Python-based lead generation pipeline
with Gemini-powered AI qualification and deterministic
quality-control rules.

The original lead generation workflow and AI qualification
operate as separate processing stages.

## Phase 1 — Lead Generation and Data Processing

1. **Keyword Discovery**
   - Discover potential businesses using target keywords.

2. **Website Collection**
   - Collect candidate business websites.

3. **Website Data Extraction**
   - Extract relevant business and website information.

4. **Data Cleaning and Normalization**
   - Standardize collected information.

5. **Validation and Quality Assurance**
   - Validate records and identify records requiring review.

6. **CRM Export**
   - Generate structured CRM-ready datasets.

## Phase 2 — Gemini AI Qualification

The AI qualification component reads validated website data
from the existing LeadFlow pipeline.

1. **Data Adapter**
   - Read the validated website CSV.
   - Extract homepage text snippets.
   - Skip records without website text.

2. **Gemini Classification**
   - Submit website text to Gemini 2.5 Flash.
   - Request structured JSON containing classification,
     confidence, evidence, reasoning, and review status.

3. **Pydantic Validation**
   - Validate the response structure.
   - Enforce permitted classifications.
   - Validate confidence values and required evidence.

4. **Evidence Verification**
   - Normalize supporting evidence and source text.
   - Check whether the evidence appears in the source.

5. **Deterministic Decision Guard**
   - Apply the 0.85 confidence threshold.
   - Require review for unsupported evidence or uncertainty.
   - Classify results as candidate, review, or
     exclude_candidate.

6. **Safe Error Handling**
   - Route missing-source records and processing
     exceptions to review.
   - Log processing exceptions.

7. **AI Results Export**
   - Export AI qualification results to a separate CSV.

## Phase 3 — CRM and AI Integration

The main application integrates previously generated
AI results without making additional Gemini API calls.

1. Load existing CRM quality-control results.
2. Load available AI qualification results.
3. Match records using normalized business domains.
4. Preserve existing CRM exclusion and review decisions.
5. Require completed AI qualification before retaining
   an existing CRM-approved record.
6. Export the integrated results using timestamped filenames.

AI classification alone cannot automatically approve
a CRM lead.

## Verified Execution Results

A September 2026 local execution produced one completed
Gemini qualification result.

The inspected CRM-AI integration dataset contained
114 records.

| Final Decision | Records |
|---|---:|
| Keep | 1 |
| Review | 109 |
| Exclude Candidate | 4 |
| Total | 114 |

Only one record in the inspected AI results dataset
had completed Gemini qualification.

The remaining records were processed according to
the existing CRM decisions and integration rules.

## Reliability Principles

- Structured AI responses
- Schema validation
- Source-text evidence matching
- Deterministic qualification rules
- Conservative handling of uncertainty
- Manual-review fallback
- Preservation of existing CRM quality-control decisions
- Separate AI results and timestamped integration outputs

## Repository Notice

This document describes the LeadFlow workflow without
exposing proprietary implementation modules, credentials,
or sensitive business datasets.
