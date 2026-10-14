---
name: game_qa_bug_screener
display_name: Game QA Bug Report Duplicate & Severity Screener Orchestrator
description: Central Orchestrator managing a 3-agent pipeline (Planner, Executor, Reviewer) to parse raw bug reports, deduplicate active tickets, audit reproduction steps, and verify severity levels before human triage approval.
version: 1.0.0
framework: NIST AI RMF
agents:
  - role: Planner
    name: Bug Parser (Agent 1)
  - role: Executor
    name: Deduplication Engine (Agent 2)
  - role: Reviewer
    name: Severity & Repro Auditor (Agent 3)
skills:
  - references/parse_bug_submission.md
  - references/deduplicate_qa_tickets.md
  - references/verify_severity_rating.md
pipeline: PLAN >> ORCHESTRATE >> EXECUTE >> CHECK >> HUMAN APPROVAL >> OUTPUT
---

# Game QA Bug Report Duplicate & Severity Screener Orchestrator

**Project Context:** SIP10 AI Governance Orchestration Final Project

## Project Description
Game testers submit thousands of raw bug reports during playtests, burdening QA leads with manually filtering duplicates, missing steps to reproduce, and incorrectly rated severity levels. This system automates the preliminary triage process to ensure only high-quality, unique tickets reach the development team.

## AI Agents
1. **Agent 1 - Planner (Bug Parser):** Ingests incoming tester submissions, isolating crash logs, build numbers, platform specifications, and issue descriptions.
2. **Agent 2 - Executor (Deduplication Engine):** Categorizes bugs by subsystem (e.g., audio, physics, graphics) and matches submissions against active database tickets to identify duplicates.
3. **Agent 3 - Reviewer (Severity & Repro Auditor):** Audits reproduction steps for completeness and checks assigned severity ratings against studio triage guidelines.

## Orchestration Workflow
The Orchestrator strictly enforces the following 6-stage execution pipeline:
**PLAN >> ORCHESTRATE >> EXECUTE >> CHECK >> HUMAN APPROVAL >> OUTPUT**

1. **PLAN:** Agent 1 uses `references/parse_bug_submission.md` to extract core bug data and telemetry.
2. **ORCHESTRATE:** The Orchestrator routes the extracted submission structure to Agent 2.
3. **EXECUTE:** Agent 2 uses `references/deduplicate_qa_tickets.md` to assign a subsystem category and calculate duplicate confidence scores against existing tickets.
4. **CHECK:** Agent 3 uses `references/verify_severity_rating.md` to validate reproduction steps and verify severity ratings.
5. **HUMAN APPROVAL:** A QA Lead / Triage Lead grants human approval before pushing verified tickets to development teams.
6. **OUTPUT:** The validated bug report is published to the development backlog.

## NIST AI RMF Alignment
* **GOVERN:** The AI operates exclusively within the studio's internal issue tracker and bug database, preventing unreleased game data from leaking externally.
* **MAP:** Identifies risks such as the AI mistakenly closing a unique bug as a duplicate or inappropriately downgrading a critical crash severity.
* **MEASURE:** Tracks the deduplication accuracy rate and the frequency of human overrides during the approval phase.
* **MANAGE:** Enforces a mandatory QA Lead review gate to prevent automated ticket deletion or direct assignment to developers without human validation.