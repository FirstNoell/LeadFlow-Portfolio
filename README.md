# LeadFlow Portfolio

## Gemini-Powered Lead Generation & Python Automation

LeadFlow is a Python-based lead generation and data automation
system that combines traditional data processing with Gemini-powered
AI qualification and deterministic validation.

The system discovers, processes, validates, and prepares business
lead data for CRM integration.

This public repository demonstrates the system architecture,
engineering approach, workflow, and selected execution results
without exposing proprietary implementation details.

## Key Features

- Automated business website discovery and data extraction
- Data cleaning, normalization, and validation
- Modular Python processing pipeline
- Gemini 2.5 Flash integration for AI-assisted lead qualification
- Structured JSON responses validated using Pydantic
- Source-text evidence verification
- Deterministic qualification rules
- Confidence-based review decisions
- Exception handling and safe manual-review fallback
- Integration of AI results with existing CRM quality-control decisions
- Separate AI results export and timestamped CRM integration outputs

## Technology Stack

- Python
- Pandas
- Google GenAI SDK
- Gemini 2.5 Flash
- Pydantic
- REST API integration
- CSV data processing

## System Architecture

The AI qualification component operates separately from
the existing lead generation pipeline.

```text
Existing LeadFlow Pipeline
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
Pydantic Validation
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

AI qualification is executed separately. The main application
integrates previously generated AI results without making
additional Gemini API calls.

## AI Reliability and Validation

LeadFlow does not rely exclusively on LLM-generated decisions.

Its AI qualification component includes:

- Structured response requirements
- Pydantic schema validation
- Source-text evidence matching
- A confidence threshold of 0.85
- Deterministic classification rules
- Manual-review decisions for uncertain classifications
- Exception logging and review fallback

An AI qualification candidate is not automatically
an approved CRM lead.

Existing CRM quality-control decisions remain part
of the final integration process.

## Verified Execution Results

A September 2026 local execution produced one completed
Gemini qualification result.

The existing CRM dataset contained 114 records.

Following integration with the available AI results:

| Final Decision | Records |
|---|---:|
| Keep | 1 |
| Review | 109 |
| Exclude Candidate | 4 |
| Total | 114 |

Only one record had a completed Gemini qualification result
in the inspected AI results dataset.

Records without completed AI qualification may require
additional review. These results do not represent
114 completed Gemini classifications.

## Repository Contents

- `docs/` — Architecture and workflow documentation
- `sample_data/` — Selected sample outputs
- `screenshots/` — Execution evidence
- `requirements.txt` — Project dependencies

## Repository Notice

This repository is a portfolio showcase.

Core implementation modules, proprietary business logic,
credentials, and sensitive datasets are intentionally excluded.

## Project Status

The Python data-processing pipeline and Gemini-powered
AI qualification components have been implemented.

Selected execution outputs and integration results have
been inspected. Comprehensive automated test coverage
and large-scale Gemini qualification performance have
not been established by the evidence presented here.
