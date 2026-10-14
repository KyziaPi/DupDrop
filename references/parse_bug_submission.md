# Skill: Parse Bug Submission
**Assigned Role:** Agent 1 - Planner

## Objective
Ingest incoming tester submissions to isolate structured metadata from raw text, specifically targeting build numbers, platform tags, crash logs, and issue descriptions.

## Input Guidance
* **Reference File:** Refer to `/examples/guidance/sample_input_raw_bug.txt` for a standard example of the raw, unformatted text you will receive.

## Execution Instructions
1. **Extract Core Metadata:**
   * `build_number`: Extract string matching version pattern (e.g., `v1.04.55`).
   * `platform`: Identify target platform (`PC`, `PS5`, `Xbox Series X`, `Switch`).
   * `tester_id`: Extract reporter identification tag.
   * `requested_severity`: Identify tester-assigned rating (`Blocker`, `Major`, `Minor`, `Trivial`).
2. **Isolate Description & Steps:**
   * Separate the primary bug description from conversational text.
   * Extract any listed steps to reproduce (Repro Steps) into a sequential list.
3. **Extract Crash Telemetry & Stack Traces:**
   * Identify embedded crash logs, memory addresses, or exception codes (e.g., `ER_MEM_OVERFLOW_0x88493`).
   * Flag whether a valid crash log is `PRESENT` or `ABSENT`.
4. **Format Structured Output:**
   * Compile extracted parameters into a clean JSON structure for the Executor agent.