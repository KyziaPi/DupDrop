# Skill: Verify Severity Rating
**Assigned Role:** Agent 3 - Reviewer

## Objective
Audit reproduction steps for completeness and verify requested severity ratings against studio triage guidelines.

## Output Guidance (Gold Standard)
* **Reference File:** You **must** format your final output to match the exact structure, layout, and tone demonstrated in `/examples/guidance/sample_output_approved_ticket.md`.

## Studio Severity Guidelines
* **Blocker / Critical:** Hard crashes to desktop/dashboard, progression-blocking bugs, corrupted save files, build failure.
* **Major:** Broken core gameplay mechanics without crash, severe visual tearing, soft locks with workaround available.
* **Minor:** Non-blocking visual glitches, minor clipping, missing secondary sound effects, UI alignment issues.
* **Trivial:** Typos, minor cosmetic issues, edge-case easter egg bugs.

## Execution Instructions
1. **Audit Reproduction Steps:**
   * Verify if steps follow a clear, sequential path (1, 2, 3...).
   * **Pass:** Steps are reproducible and specific.
   * **Fail/Flag:** Steps are missing, ambiguous (e.g., "it just happened"), or non-sequential.
2. **Verify Severity Rating:**
   * Compare tester-requested severity against Studio Severity Guidelines.
   * Flag over-rated severity (e.g., tester set "Blocker" for a minor visual typo).
   * Flag under-rated severity (e.g., tester set "Minor" for a crash log).
3. **Formulate Final Recommendation:**
   * State original severity vs. AI-recommended severity with clear reasoning.
4. **Compile Final Draft:** Produce a unified triage summary marked **`[PENDING QA LEAD APPROVAL]`**.