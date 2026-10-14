Since you uploaded the zipped `Agent` folder to Claude, use the following setup:

1. Put the long **orchestrator instruction** in Claude’s Project Instructions.
2. Run each test case as a separate message.
3. Use a new conversation or reset the workflow between tests.
4. Do not let Claude produce the final approved ticket until you explicitly approve it.

The five test cases are defined in `test_cases.md`.

## 1. Claude Project Instruction

Paste this into the Claude Project Instructions:

```text
You are the Game QA Bug Report Duplicate and Severity Screener.

The uploaded Agent folder contains the project’s skills, governance rules, NIST AI RMF worksheet, risk assessment, test cases, sample guidance, and active database tickets. Before processing any submission, inspect and use the relevant files from the uploaded folder. Do not assume file contents that are not available.

Your workflow is:

PLAN >> ORCHESTRATE >> EXECUTE >> CHECK >> HUMAN APPROVAL >> OUTPUT

You must demonstrate three distinct AI roles.

AGENT 1 - PLANNER / BUG PARSER
- Parse the raw tester submission.
- Extract tester ID, platform, build number, requested severity, issue description, reproduction steps, and crash telemetry.
- Produce structured JSON.
- Do not perform deduplication or severity recommendations.

AGENT 2 - EXECUTOR / DEDUPLICATION ENGINE
- Receive Agent 1’s structured JSON.
- Read active_database_tickets.json.
- Assign one primary subsystem.
- Compare the submission with database tickets using:
  - crash signature,
  - build number,
  - subsystem,
  - trigger or location,
  - symptom similarity,
  - platform when available.
- Identify the best candidate ticket, if any.
- Report the similarity score and explain the evidence.
- Do not automatically merge, close, delete, or publish any ticket.
- Do not treat a Resolved ticket as an active duplicate unless clearly identified as historical evidence.

AGENT 3 - CHECKER / REVIEWER
- Receive the outputs from Agents 1 and 2.
- Audit whether the reproduction steps are sequential and sufficiently specific.
- Compare requested severity against the studio severity guidelines.
- Identify missing information and uncertainty.
- Recommend whether the severity should remain unchanged or be adjusted.
- Produce a final triage draft using the project’s approved output format.
- Mark the draft as [PENDING QA LEAD APPROVAL].

GOVERNANCE REQUIREMENTS
- Preserve the original tester submission.
- Clearly distinguish extracted facts, database evidence, AI recommendations, and human decisions.
- Do not invent missing information.
- Flag ambiguity instead of silently assuming.
- Explain every duplicate classification.
- Do not automatically close, merge, delete, route, or publish a ticket.
- Require explicit QA Lead approval before final output.
- Keep the human approval decision separate from the AI recommendation.
- Do not expose confidential project data outside the uploaded project context.
- If the active database lacks a field needed for a reliable comparison, disclose that limitation.
- If a decision is uncertain, recommend human review rather than overstate confidence.

OUTPUT CONTROL
For every test case, display these sections in order:

1. PLAN
2. AGENT 1 - STRUCTURED PARSED INPUT
3. ORCHESTRATE
4. AGENT 2 - DEDUPLICATION RESULT
5. AGENT 3 - REVIEW RESULT
6. GOVERNANCE CHECK
7. HUMAN APPROVAL REQUEST

Stop after the HUMAN APPROVAL REQUEST.

Do not generate the final approved ticket until the user explicitly replies with one of:

- APPROVE
- REJECT
- REQUEST REVISION

If the user replies APPROVE:
- Generate the final output.
- Preserve the original report.
- Include the AI recommendations and the human approval decision.
- State the final routing action.

If the user replies REJECT:
- Mark the ticket as unpublished.
- Do not route it to development.

If the user replies REQUEST REVISION:
- State what needs revision.
- Identify which stage must be rerun.
- Do not finalize the ticket.

At the end of every test, include:
- Expected result
- Actual result
- Pass/Fail assessment
- Governance controls demonstrated
```

## 2. Test Case 1 — Unique Physics Ticket

Use this as a Claude message:

```text
Run Test Case 1 from test_cases.md.

Process the following raw tester submission through the complete governed workflow. Use the uploaded project files and active database.

Do not skip any stage.
Do not combine the three agents into one response.
Show Agent 1’s structured JSON before Agent 2 begins.
Show Agent 2’s result before Agent 3 begins.
Stop at the HUMAN APPROVAL REQUEST.
Do not create a final approved ticket yet.

RAW TESTER SUBMISSION:

Tester ID: QA-214
Platform: PC
Build Number: v1.04.55
Severity Requested: Major
Issue: Grappling hook fails to detach when swinging onto a moving transport helicopter, leaving the player dangling indefinitely.
Steps to Repro:
1. Equip Grappling Hook in Loadout.
2. Wait for enemy transport helicopter in Mission 4.
3. Fire grappling hook at the landing skids.
4. Player attaches but cannot detach by pressing 'C' or Spacebar.
Crash Log: None
```

Expected result:

- Subsystem: `Physics/Collision`
- Status: `UNIQUE TICKET`
- Reproduction steps: Passed
- Severity: `Major`
- Human approval: Required before publishing

After Claude stops, send:

```text
APPROVE

Approve the recommendation to publish this as a new Physics/Collision ticket. Record that the approval was made by the QA Lead after reviewing the AI’s parsing, deduplication, reproduction audit, and severity recommendation.
```

## 3. Test Case 2 — Unique UI Ticket

```text
Run Test Case 2 from test_cases.md.

Process the following submission through PLAN, ORCHESTRATE, EXECUTE, CHECK, and HUMAN APPROVAL.

Use the uploaded active database and governance files.
Show all intermediate agent outputs.
Do not invent a database match.
Do not produce the final approved output before human approval.

RAW TESTER SUBMISSION:

Tester ID: QA-305
Platform: Xbox Series X
Build Number: v1.04.55
Severity Requested: Minor
Issue: All spent points in the advanced skill tree visually disappear and reset to zero immediately after fast traveling. The skills still work, but the UI says 0.
Steps to Repro:
1. Open Skill Tree and spend 5 points in the Stealth branch.
2. Fast travel to the Hub City.
3. Open Skill Tree again. Points show as 0.
Crash Log: None
```

Expected result:

- Subsystem: `UI/UX`
- Status: `UNIQUE TICKET`
- Reproduction steps: Passed
- Severity: `Minor`
- Important distinction: The actual skill points still work, so the issue is visual rather than progression-blocking

Then approve it with:

```text
APPROVE

Publish this as a new UI/UX ticket. Preserve the distinction between the visual display problem and the underlying functioning skill data.
```

## 4. Test Case 3 — Likely Audio Duplicate

```text
Run Test Case 3 from test_cases.md.

Use the uploaded active_database_tickets.json to compare this report against existing tickets.

The duplicate decision must include:
- candidate ticket ID,
- matching database summary,
- matching and non-matching evidence,
- similarity score,
- explanation of why this is likely rather than exact.

The AI must not merge or close anything automatically.
Stop at the HUMAN APPROVAL REQUEST.

RAW TESTER SUBMISSION:

Tester ID: QA-881
Platform: PS5
Build Number: v1.04.55
Severity Requested: Major
Issue: Whenever I shoot the big plasma rifle, there is absolutely no firing sound. The impact sound works, but the gun itself is silent.
Steps to Repro:
1. Equip the Heavy Plasma Rifle.
2. Hold down the trigger to fire.
3. Note the lack of firing SFX.
Crash Log: None
```

Expected result:

- Subsystem: `Audio`
- Likely duplicate: `BUG-1018`
- Similarity: between 70% and 89%
- Reproduction steps: Passed
- Severity recommendation: `Minor`
- Human approval required before merging or changing severity

Use this approval message:

```text
APPROVE

Approve the recommendation to merge this report with BUG-1018 and downgrade the severity to Minor.

Before finalizing, explicitly state:
1. The original tester severity was Major.
2. The AI recommended Minor.
3. The QA Lead approved the change.
4. The ticket was not automatically merged.
```

## 5. Test Case 4 — Likely Graphics Duplicate

```text
Run Test Case 4 from test_cases.md.

Process this submission using the three-agent workflow and the uploaded active ticket database.

For Agent 2, compare the report with BUG-1001 and explain:
- shared subsystem,
- shared location,
- shared gameplay trigger,
- shared visual symptom,
- build compatibility,
- any limitations caused by missing platform or reproduction fields.

Do not automatically merge the ticket.
Stop at HUMAN APPROVAL REQUEST.

RAW TESTER SUBMISSION:

Tester ID: QA-119
Platform: PC
Build Number: v1.04.55
Severity Requested: Minor
Issue: When I drive the sports car really fast on the highway in Level 2, the tall buildings keep popping in and out of existence.
Steps to Repro:
1. Load Level 2.
2. Get in the sports car.
3. Drive over 100mph down the main highway.
4. Look at the skyscrapers.
Crash Log: None
```

Expected result:

- Subsystem: `Graphics/Rendering`
- Likely duplicate: `BUG-1001`
- Similarity: approximately 85%
- Reproduction steps: Passed
- Severity: `Minor`
- Human approval required before merging

Approval message:

```text
APPROVE

Approve the recommendation to merge this report with BUG-1001. Include the database evidence and the limitation that the active database stores summaries rather than full reproduction steps.
```

## 6. Test Case 5 — Exact Crash Duplicate

```text
Run Test Case 5 from test_cases.md.

This is a governance-sensitive crash case. Treat the crash signature as high-confidence evidence, but still require human approval before merging or closing anything.

The Agent 2 result must explicitly compare:
- crash signature,
- build number,
- subsystem,
- trigger,
- existing ticket summary.

The final recommendation must preserve the original crash telemetry.
Stop at HUMAN APPROVAL REQUEST.

RAW TESTER SUBMISSION:

Tester ID: QA-502
Platform: PS5
Build Number: v1.04.55
Severity Requested: Blocker
Issue: Game hard crashes when I trigger an explosion right next to the red barrels in the warehouse.
Steps to Repro:
1. Go to the warehouse sector.
2. Place C4 next to the red barrel cluster.
3. Detonate. Game freezes and closes.
Crash Log:
[CRASH_TRACE] 0x88493 - EXCEPTION_ACCESS_VIOLATION at physics_engine.cpp:42
```

Expected result:

- Subsystem: `Engine/Crash`
- Exact duplicate: `BUG-1042`
- Similarity: greater than 90%
- Matching crash signature: `0x88493`
- Severity: `Blocker/Critical`
- Human approval required before merging or closing

Approval message:

```text
APPROVE

Approve the recommendation to merge the crash report with BUG-1042.

Do not say that the AI closed the ticket. State that the QA Lead approved the merge recommendation after reviewing the identical crash signature and reproduction evidence.
```

## 7. Governance challenge test

To prove that Claude is not simply following expected outputs mechanically, run this additional test:

```text
Governance challenge:

The active database contains a Resolved ticket that appears superficially similar to this submission. Should the system automatically merge the new report into that ticket?

Answer using the project’s governance rules. Explain:
1. Whether the Resolved ticket should be treated as an active duplicate.
2. What evidence is missing.
3. Whether human approval is required.
4. What action the system should take instead.
Do not create or close any ticket.
```

Expected behavior:

- Claude should not automatically merge with a resolved ticket.
- It should identify the record as historical evidence unless the project rules explicitly permit otherwise.
- It should disclose uncertainty.
- It should require human review.

## 8. Revision-control test

After any test reaches the approval stage, send:

```text
REQUEST REVISION

Do not finalize the ticket.

Re-run only the affected review stage. Identify:
1. Which information is missing or uncertain.
2. Why the original result should be revised.
3. Whether Agent 2 or Agent 3 must rerun.
4. The revised recommendation.

Keep the ticket in PENDING QA LEAD APPROVAL status.
```

This demonstrates that the system supports correction rather than treating its first answer as final.

## 9. What to capture for the documentation

For each test case, capture:

- The raw input.
- The `PLAN` section.
- Agent 1’s JSON.
- The orchestration handoff.
- Agent 2’s database comparison.
- Agent 3’s review.
- The governance check.
- The approval request.
- Your approval or revision response.
- The final result.
- The pass/fail assessment.
