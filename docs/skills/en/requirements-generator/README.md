# Requirements & Constraints Generator Skill

A skill that analyzes product/service documentation to automatically generate structured requirements specifications and constraints documents.

## When to Use

When you have source documents (product briefs, feature specs, workshop slide summaries, etc.) and want to:
- Generate a software requirements specification
- Generate a constraints document
- Generate both at once

## Usage

### Generate Requirements Specification

```
Analyze this PDF and generate a requirements specification
```

### Generate Constraints Document

```
Create a constraints document based on this product brief
```

### Generate Both

```
Generate both requirements and constraints documents from this document
```

## Supported Source Document Formats

- PDF
- Markdown
- Images (slide captures, etc.)
- Other text-based documents

## Output Files

- Requirements: `{project_abbreviation}_requirements.md`
- Constraints: `{project_abbreviation}_constraints.md`

## Core Principles

- Derived only from information explicitly stated in source documents — no guessing
- Items that cannot be confirmed are marked as "Insufficient Information"
- Summary table of insufficient information items provided at the end of the document
- Follows the language of the source document (technical terms kept in English)

## Requirements ID Schema

| Type | Format | Example |
|------|--------|---------|
| Functional Requirement | `FR-{area}-{seq}` | FR-CON-001, FR-MON-002 |
| Non-Functional Requirement | `NFR-{seq}` | NFR-001 |
| History Storage | `FR-HIST-{seq}` | FR-HIST-001 |

## Constraints Categories

| Category | Prefix | Example |
|----------|--------|---------|
| Platform/Device | PC | PC-001 |
| Technical | TC | TC-001 |
| Business | BC | BC-001 |
| Legal/Regulatory | LC | LC-001 |
| Pending Decision | PD | PD-001 |
| Assumption | AS | AS-001 |
