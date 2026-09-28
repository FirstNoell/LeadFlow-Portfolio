# LeadFlow System Architecture

## Overview

LeadFlow combines a modular Python lead-generation pipeline
with Gemini-powered AI qualification and deterministic
validation.

The AI qualification component operates separately from
the original data-processing pipeline.

## High-Level Architecture

```text
Business Website Discovery
          |
          v
Website Data Extraction
          |
          v
Data Cleaning and Validation
          |
          v
Validated Website Dataset
          |
          v
AI Data Adapter
          |
          v
Gemini 2.5 Flash
          |
          v
Structured JSON Response
          |
          v
Pydantic Schema Validation
          |
          v
Source Evidence Verification
          |
          v
Deterministic Decision Guard
          |
          v
AI Qualification Results
          |
          v
Separate AI Results CSV
          |
          v
CRM Integration
          |
          v
Timestamped Integrated Output
```

## AI Qualification

LeadFlow uses Gemini 2.5 Flash to classify businesses
using extracted website text.

The system requests structured JSON output containing:

- Classification
- Confidence
- Supporting evidence
- Reasoning
- Manual-review indicator

Pydantic validates the response before further processing.

## Evidence Validation

Supporting evidence is checked against normalized
source text.

Evidence that cannot be matched triggers manual review.

This mechanism verifies text matching, not the factual
correctness of the model's interpretation.

## Deterministic Decision Guard

The decision guard applies additional rules:

- Confidence below 0.85 requires review.
- Unsupported evidence requires review.
- Uncertain classifications require review.
- Clearly identified non-target categories may be
  marked as exclusion candidates.
- Qualified direct-brand classifications may be
  marked as candidates.

A candidate is not automatically an approved CRM lead.

## Error Handling

Missing website text and AI processing exceptions
result in review decisions.

Exceptions are logged for troubleshooting.

## CRM Integration

AI results are combined with existing CRM
quality-control decisions using Pandas.

Existing exclusion and review decisions are preserved.

Records without completed AI qualification
require review.

The integration process generates timestamped output
files to preserve previous integration results.

## Design Principles

- Modular architecture
- Separation of AI qualification and CRM integration
- Structured AI responses
- Deterministic validation
- Conservative handling of uncertainty
- Preservation of existing CRM quality-control decisions
- Traceable processing and review decisions

## Repository Notice

This public repository documents the architecture
without exposing proprietary implementation modules,
credentials, or sensitive datasets.
