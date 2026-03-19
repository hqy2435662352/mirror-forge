# MirrorForge Playbook

## Overview

This playbook records recurring improvement patterns identified through engineering-gap analysis. Each entry includes trigger conditions, evidence boundaries, and a defensive checklist.

---

## Gap 1: Contract Awareness

- **Status**: Observed
- **Confidence**: High
- **Last Updated**: 2026-03-19
- **Source Session**: 2026-03-12 `parseGSDFile` analysis/refactor
- **Derived From**: Finding 1

#### Representative Evidence

**Code Evidence**:
```cpp
// The original code mixed parsing logic with the JSON structure contract
// without explicitly documenting which boundary was real.
// S1 Oracle: "The most important judgment here is not 'this file is too long so we should split it,'
// but first confirming what external contracts it actually carries."
```

**Dialogue Evidence**:
```
"The current module is not a pure XML parser. It simultaneously carries GSD file scanning,
XML-to-JSON structural adaptation, and local GSD cache generation / synchronization..."
```

#### Trigger Conditions

- Before starting any refactor
- Requests containing words like “split,” “extract,” or “simplify”

#### Next Minimal Action

Before starting the refactor, answer: `Who consumes this code, and what do they actually depend on?`

#### Defensive Checklist

- [ ] All consumer modules have been identified
- [ ] It is explicit whether consumers depend on a C++ interface or a data contract (JSON / file format)
- [ ] A behavior baseline has been established
- [ ] A consumer-blind regression check has been defined

#### Where It May Hurt Next

If this step is skipped, a future refactor of `device_bind.cpp` may accidentally change the JSON output structure and break `PNConfigLibFileDesign`.

---

## Gap 2: Legacy Behavior Respect

- **Status**: Confirmed
- **Confidence**: High
- **Last Updated**: 2026-03-19
- **Source Session**: 2026-03-12 `parseGSDFile` analysis/refactor
- **Derived From**: Finding 2

#### Representative Evidence

**Code Evidence**:
```cpp
// The three I/O length derivation paths correspond to different GSD sample scenarios:
// 1. DAP virtual submodule: explicit Length first
// 2. Module virtual submodule: explicit Length, otherwise derive from DataType
// 3. Top-level SubmoduleList: computed via CountIODataLength(DataType)
// S1 Oracle: "This kind of 'imperfect but stable' difference must not be unified in the first pass."
```

**Dialogue Evidence**:
```
"Unify the three I/O length derivation rules" was listed under pseudo-optimizations
(high risk, should not be mixed into the refactor).
```

#### Trigger Conditions

- You notice “three almost identical pieces of code”
- You feel tempted to “unify” or “merge” repeated logic
- You want to “fix” a historical inconsistency

#### Next Minimal Action

For code that “looks duplicated but behaves differently,” run sample-coverage analysis first and ask: `Is this a bug, or an intentional historical behavior?`

#### Defensive Checklist

- [ ] Confirmed whether the multiple code paths are intentional differences rather than accidental duplication
- [ ] Verified that samples cover all distinct branches
- [ ] If sample coverage is insufficient, new samples were added first
- [ ] The reason for the difference has been explicitly recorded in analysis notes

#### Where It May Hurt Next

If “historical inconsistency” is treated as a bug to be cleaned up, existing GSD sample behavior may break — for example, parsing output for devices like PLC200smart could flip from `null` to an object (or vice versa) without downstream consumers being prepared.

---

## Gap 3: Refactoring Endgame Awareness

- **Status**: Observed
- **Confidence**: Medium
- **Last Updated**: 2026-03-19
- **Source Session**: 2026-03-18 `parseGSDFile` commit
- **Derived From**: Finding 3

#### Representative Evidence

**Dialogue Evidence**:
```
User: "The core goal has been achieved — from 2200 lines down to 1800 (-18%),
470 lines of dead code removed, and 7 helper functions extracted."
User: "The directions listed in the document are nice-to-have, not must-have."
User: "The next step should be defensive rather than aggressive."
```

#### Trigger Conditions

- After a refactor task has started
- When phrases like “we could still...” or “while we're here...” start appearing
- When work is continuing without an explicit finish line

#### Next Minimal Action

Define completion criteria before starting. Once the criteria are met, stop and run a review.

#### Defensive Checklist

- [ ] Completion criteria were defined before the work started
- [ ] The completion criteria were documented
- [ ] A review happens once the criteria are reached
- [ ] Work does not continue merely because more optimization is possible

#### Where It May Hurt Next

A refactor may sprawl indefinitely, turning “reduce complexity” into “restructure every neighboring module,” consuming time without proportional return.

---

## Gap 4: Change Boundary Discipline

- **Status**: Observed
- **Confidence**: Medium
- **Last Updated**: 2026-03-19
- **Source Session**: 2026-03-18 `parseGSDFile` commit
- **Derived From**: Finding 4

#### Representative Evidence

**Dialogue Evidence**:
```
Assistant: "GSD-related refactor content (should commit)" vs "non-GSD content (should not commit)"
User: "Commit only the GSD-related part; please draft the commit content first."
```

#### Trigger Conditions

- Right before commit
- When phrases like “let's commit this too” or “I only changed this a little bit” appear

#### Next Minimal Action

Before commit, ask: `What does this commit change? What does it explicitly not change?`

#### Defensive Checklist

- [ ] The commit message matches the actual scope of the change
- [ ] No unrelated edits are bundled in
- [ ] Multiple concerns have been split into separate commits
- [ ] `git add -p` was used when precise staging was required

#### Where It May Hurt Next

Code review load increases, change reasons become harder to trace, and rollback costs rise.

---

## Gap 5: Verification Automation Awareness

- **Status**: Confirmed
- **Confidence**: High
- **Last Updated**: 2026-03-19
- **Source Session**: 2026-03-18 `parseGSDFile` commit
- **Derived From**: Finding 5

#### Representative Evidence

**Code Evidence**:
```bash
# scripts/verify_gsd_baseline.sh
for file in "${SAMPLE_FILES[@]}"; do
    if ! diff -q -b "$TEMP_DIR/$file" "$BASELINE_DIR/$file"; then
        echo -e "${RED}✗ $file - CHANGED${NC}"
        CHANGES=true
```

**Dialogue Evidence**:
```
User: "What is the purpose of the automated verification script?"
Assistant: "At its core, it prevents the tragedy of 'I only meant to change a small thing,
but accidentally broke behavior without noticing.'"
```

#### Trigger Conditions

- Any refactor that touches an external contract
- Changes to JSON schema, file format, or protocol output

#### Next Minimal Action

Establish the baseline verification mechanism first, then make the change.

#### Defensive Checklist

- [ ] A baseline output has been captured
- [ ] An automated verification script has been created
- [ ] Verification runs after every change
- [ ] The diff must be zero before proceeding

#### Where It May Hurt Next

Without a baseline, behavior may break in unknown scenarios and the regression may not be caught in time.

---

## Update Log

| Date | Update | Status |
|---|---|---|
| 2026-03-19 | Initial entry, derived from the `parseGSDFile` refactor analysis | Observed |
