# Game QA Screener Test Suite (5 Test Cases)

## Test Case 1: Grappling Hook Soft-Lock (Unique)
**Raw Tester Submission:**
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

**Expected Triage Output:**
* **Subsystem Category:** Physics/Collision
* **Duplicate Status:** UNIQUE TICKET (No matching active tickets for grappling hook issues).
* **Repro Steps Audit:** PASSED (Clear, 4-step sequential path).
* **Severity Recommendation:** CONFIRMED - Major (Soft-lock requiring mission restart).
* **Action:** Flag for QA Lead approval to publish as a new ticket.

---

## Test Case 2: Skill Tree UI Reset (Unique)
**Raw Tester Submission:**
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

**Expected Triage Output:**
* **Subsystem Category:** UI/UX
* **Duplicate Status:** UNIQUE TICKET.
* **Repro Steps Audit:** PASSED.
* **Severity Recommendation:** CONFIRMED - Minor (Visual UI glitch without affecting actual gameplay progression).
* **Action:** Flag for QA Lead approval to publish as a new ticket.

---

## Test Case 3: Missing Plasma Rifle Audio (Likely Duplicate)
**Raw Tester Submission:**
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

**Expected Triage Output:**
* **Subsystem Category:** Audio
* **Duplicate Status:** LIKELY DUPLICATE (70-89% Match to active ticket `#BUG-1018`: "Missing sound effects completely when firing the heavy plasma rifle").
* **Repro Steps Audit:** PASSED.
* **Severity Recommendation:** DOWNGRADE RECOMMENDED - Minor (Audio absence is annoying but does not break core mechanics).
* **Action:** Flag for QA Lead approval to merge as a duplicate under `#BUG-1018` and downgrade severity.

---

## Test Case 4: Level 2 Building Flicker (Likely Duplicate)
**Raw Tester Submission:**
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

**Expected Triage Output:**
* **Subsystem Category:** Graphics/Rendering
* **Duplicate Status:** LIKELY DUPLICATE (85% Match to active ticket `#BUG-1001`: "Severe texture pop-in on Level 2 skyscraper assets during high-speed driving").
* **Repro Steps Audit:** PASSED.
* **Severity Recommendation:** CONFIRMED - Minor.
* **Action:** Flag for QA Lead approval to merge as a duplicate under `#BUG-1001`.

---

## Test Case 5: Red Barrel Physics Crash (Exact Duplicate)
**Raw Tester Submission:**
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

**Expected Triage Output:**
* **Subsystem Category:** Engine/Crash
* **Duplicate Status:** EXACT DUPLICATE (>90% Match to active ticket `#BUG-1042`: "Explosion physics crash near barrel assets" based on identical `0x88493` crash signature).
* **Repro Steps Audit:** PASSED.
* **Severity Recommendation:** CONFIRMED - Blocker/Critical (Hard crash to dashboard).
* **Action:** Flag for QA Lead approval to recommend merging this report into `#BUG-1042`.