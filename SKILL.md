---
name: mirror-forge
description: Analyze AI coding feedback to identify the developer's underlying engineering gaps and convert one-time AI assistance into long-term growth assets. Use this skill whenever: (1) the user asks what problems AI exposed in their code; (2) the user wants reflection on refactoring, optimization, bug fixing, or compilation-error resolution; (3) the user asks to prepare or update their growth playbook; (4) the user requests a weekly review of recurring patterns. MirrorForge is for personal engineering reflection, not generic code review.
disable_model_invocation: true
---

# MirrorForge

Turn AI coding feedback into engineering growth.

## Purpose

MirrorForge is a reflection skill for AI-assisted coding sessions.
It is designed to answer:
- What engineering problems did the user's original code expose?
- What recurring patterns does this reveal about the user's capability gaps?
- What training direction should follow from this case?

MirrorForge is **not** a standard code review tool.
Its job is to analyze the **user's underlying engineering patterns**, not to judge whether the final code is good.

---

## When to Use

Use this skill when:
- The user explicitly invokes `/mirror-forge`
- The user asks: "what problems did I have?", "what gaps did I expose?", "what did this refactor reveal about me?"
- The user wants to analyze AI refactoring, optimization, bug fixing, compilation-error solving, or debugging as a growth signal
- The user wants to prepare or update a growth playbook
- The user asks for a weekly or periodic review of recurring patterns

Do **NOT** use when:
- The user only wants code generation
- The user wants a normal code review
- The user asks only "what is wrong with this code" without asking about **their** capability gaps

Key distinction:
- "what's wrong with this code" → regular code review
- "what problems did I expose / what gaps did I show" → MirrorForge

---

## Core Principle

**Analyze the USER's problems, not the Agent's problems.**

The analysis target is:
- the user's old implementation
- the user's decision patterns
- the user's engineering habits revealed by AI assistance

The analysis target is **NOT**:
- praising the agent
- reviewing only the final improved code
- listing abstract best practices with no evidence

---

## Analysis and Persistence Are Separate Commitments

MirrorForge may analyze automatically when invoked,
but it may **not** assume that analysis should automatically become playbook content.

Default behavior:
- complete the analysis report
- then explicitly hand off to the user
- only prepare or write playbook content according to the persistence rules below

MirrorForge must have **next-step awareness** after analysis.
If the analysis is complete and the user has not yet said whether to record it,
MirrorForge should actively ask whether the user wants this case turned into a playbook draft.

---

## Evidence Hierarchy

Use the strongest available evidence.

1. **Diff-backed code evidence** (strongest)
   - user's old code → AI-improved code
   - best source for code-level capability gaps

2. **Single code snapshot**
   - valid for code reflection when diff is unavailable
   - weaker than diff, but still legitimate code evidence

3. **Dialogue / decision evidence**
   - useful for behavior, prioritization, judgment, or risk-awareness patterns
   - cannot replace code-backed findings when meaningful diff exists

---

# HIGHEST PRIORITY: NEVER LET DIFF ANALYSIS STOP AT "LEDGER ONLY"

This is the main failure mode.

If meaningful diff exists, MirrorForge must not stop at:
- logging diff chunks
- saying "non-diagnostic"
- switching to easier dialogue issues

**If meaningful diff exists, final code-backed findings are mandatory.**

That is the key anti-drift rule.

---

# SECOND HIGHEST PRIORITY: DO NOT MISTAKE SUMMARY FOR ANALYSIS

This is the second major failure mode.

MirrorForge is **not complete** when it only provides:
- a short conclusion
- a label like "weak validation mindset"
- one evidence bullet plus one abstract takeaway

A valid MirrorForge analysis must complete the full reasoning chain:

**raw evidence → what was wrong → why it is truly problematic → why it maps to this gap → confidence + limitation → concrete fix**

If the output only contains **evidence + conclusion**, it is too compressed.
That is a summary, not an analysis.

---

## Analysis Modes

MirrorForge must choose exactly one **Primary Mode** before analysis starts.

### Mode A: Diff-backed Code Reflection
Use when meaningful diff exists.

Characteristics:
- Primary evidence = before/after diff
- Code-backed findings are mandatory
- Dialogue findings are optional supplements only

### Mode B: Code Snapshot Reflection
Use when no meaningful diff exists but there is code.

Characteristics:
- Primary evidence = code snapshot
- Code findings are allowed, but must be marked snapshot-based
- Do not pretend snapshot evidence is diff evidence

### Mode C: Dialogue-only Reflection
Use only when no usable code evidence exists.

Characteristics:
- Findings must remain behavior / judgment / process oriented
- Do not invent code-level issues without code evidence

---

## Scope Selection

Before analysis, determine whether this is:
- **Single-session** analysis
- **Cross-session** analysis

If cross-session:
1. Search and list candidate sessions
2. Assign a short alias to each session: `S1`, `S2`, `S3`, ...
3. For each candidate, show:
   - alias
   - date range / timestamp
   - inferred title or topic
   - one-line summary
   - relevant files/modules when available
4. Optionally show the full session ID for reference only
5. STOP and wait for user selection
6. Analyze only selected sessions

No exceptions.

### Cross-session candidate presentation rule
Do **NOT** require the user to type long full session IDs.

Valid user selections must include:
- `S1`
- `S1,S2`
- `all`
- `all except S2`
- `exclude S3`

If the user responds with aliases, treat that as the canonical selection input.

### Human-readable session menu example
```text
[S1] 2026-03-18 ~ 2026-03-19
Title: parseGSDFile 收尾与验证
Summary: helper 提取、日志整理、死代码清理、baseline 验证

[S2] 2026-03-12 ~ 2026-03-18
Title: parseGSDFile 架构分析与重构计划
Summary: 样本覆盖、SubmoduleList 假设、重构计划

请选择要分析的会话：S1 / S2 / S1,S2 / all
```

---

# Execution Protocol

Follow the phases in order.
Do not skip phases.
Do not jump to final conclusions before passing the required gates.

---

## Phase 0: Scope and Mode Declaration

Must declare:
- Analysis Scope: single-session or cross-session
- Primary Mode: A / B / C
- Meaningful Diff Exists: yes / no
- Code Snapshot Exists: yes / no
- Dialogue Evidence Exists: yes / no

### Meaningful Diff Definition
A diff is meaningful if it includes one or more of the following:
- behavior-affecting logic changes
- structural refactoring of non-trivial logic
- defensive checks / bounds / error-handling changes
- decomposition of large logic into helpers
- deletion of redundant or suspicious legacy logic
- changes that reveal what the user's old code was lacking

A purely formatting-only or rename-only diff is not meaningful.

### Gate
If Primary Mode is A, `Meaningful Diff Exists` must be `yes`.
Otherwise choose Mode B or C.

---

## Phase 1: Evidence Inventory

Inventory all available evidence before issuing findings.

Must list:
- files touched
- modules involved
- available diffs
- code snapshots
- key dialogue evidence
- constraints / evidence limitations

Do **NOT** write final issues yet.

---

## Phase 2: Diff Chunk Ledger (Mode A only)

If Primary Mode is A, build a ledger of **all meaningful diff chunks**.

For each meaningful diff chunk, record:
- Chunk ID
- file / location
- short change summary
- preliminary classification
- brief rationale

### Allowed classifications
Each meaningful diff chunk must be classified as **exactly one** of:

1. **Direct Code Issue**
   - the old code itself exposed a user engineering gap
   - this chunk is strong enough to become or seed a code-backed finding

2. **Supporting Evidence**
   - this chunk reinforces an existing Direct Code Issue
   - it does not need to become a separate issue

3. **Truly Neutral Refactor**
   - behavior-preserving cleanup only
   - does not reveal a meaningful weakness in the old code
   - requires explicit justification

### Strict rules
- No meaningful diff chunk may remain unclassified
- `Truly Neutral Refactor` is allowed only with explicit explanation
- Neutral chunks do **not** count toward required code issue coverage
- If a chunk is labeled `Direct Code Issue`, it **must** continue into Phase 3
- If a chunk is labeled `Supporting Evidence`, it may reinforce a later finding, but it may **not** seed a standalone finding by itself
- If a chunk is labeled `Truly Neutral Refactor`, it may **not** later appear as evidence for a finding or root gap

### Classification Consistency Rule
Ledger classification and final synthesis must be consistent.

That means:
- `Direct Code Issue` → may become or seed a code-backed finding
- `Supporting Evidence` → may reinforce a code-backed finding, but is not itself a standalone issue
- `Truly Neutral Refactor` → may not later be cited as diagnostic evidence

A chunk may not be called `Truly Neutral Refactor` in the ledger and then be used later to justify a capability gap.
If it supports a gap, classify it as `Supporting Evidence` instead.

### Anti-escape rule
Do **NOT** use `Truly Neutral Refactor` as a default bucket.
If most meaningful chunks are marked neutral, you must explain why and still satisfy the Mode A completion gate below.

---

## Phase 3: Candidate Extraction

### Mode A requirement
If Primary Mode is A, Phase 3 must extract code-backed issue candidates **before** any dialogue-level candidates are written.

For every `Direct Code Issue` chunk from the ledger, do one of the following:
- create a standalone code-backed issue candidate
- merge it into a named code-backed issue candidate

A `Direct Code Issue` chunk may **not disappear** in later phases.

### Candidate extraction order
1. Code-backed issue candidates from diff
2. Additional dialogue-level issue candidates (optional)

Dialogue findings may supplement code findings.
They may **not replace** code findings in Mode A.

### Required fields for each code-backed issue candidate
- Title
- Source Chunks
- Raw Evidence
- Literal Excerpt
- What Was Wrong in the Old Code
- Why This Is Actually Problematic
- Why This Is Not Neutral
- Gap Mapping Rationale
- Confidence
- Limitation
- Concrete Fix Pattern
- Suggested Root Gap Direction

### Required fields for each dialogue-backed issue candidate
- Title
- Raw Evidence Quotes
- Literal Excerpt
- What Decision Pattern It Reveals
- Why This Is Actually Problematic
- Gap Mapping Rationale
- Confidence
- Limitation
- Concrete Fix Pattern

### MANDATORY EVIDENCE FORMAT FOR MODE A
Every code-backed issue candidate must contain a diff block:

```markdown
```diff
- user's OLD code / old pattern
+ AI's NEW code / improved pattern
```
Location: [file:line-line]
```

If you cannot show the relevant diff block, do not claim a Mode A code issue from that chunk.

### Literal Excerpt Rule
Every final finding must preserve at least **one literal excerpt** from the underlying evidence.

Acceptable literal excerpts include:
- an exact diff block
- an exact code snippet
- an exact script / command / output snippet
- an exact dialogue quote

Paraphrase-only evidence is insufficient.
A prose summary like "the user added validation scripts later" does **not** satisfy this rule unless it includes at least one literal artifact excerpt or quote.

### MANDATORY OLD-CODE DIAGNOSIS RULE
For every code-backed issue candidate:
- the diff block is mandatory
- you MUST explicitly diagnose the weakness in the **OLD** code
- you MUST explain why the **NEW** code resolves or mitigates that weakness

If you only describe the new code, the issue is invalid.

---

## Phase 4: Final Synthesis

Merge candidates into final findings.

### Required output order
If Primary Mode is A:
1. **Code-backed Findings**
2. **Dialogue-backed Findings** (optional)
3. Root Capability Gaps
4. Risk Assessment
5. Training Recommendations

The output is invalid if it contains only dialogue findings while meaningful diff exists.

### Code-backed Findings requirements
Each final code-backed finding must include:
- Type: Code
- Source Chunks
- Raw Evidence with diff block
- Literal Excerpt
- What Was Wrong
- Why This Is Actually Problematic
- Why This Is Not Neutral
- Gap Mapping Rationale
- Confidence
- Limitation
- Concrete Fix Pattern
- Related Patterns:
  - cite specific files/modules only if confirmed by evidence
  - otherwise write: `No confirmed parallel occurrence in current evidence.`

### Dialogue-backed Findings requirements
Dialogue findings are allowed only when backed by quotes or explicit conversational evidence.
They must include at least one literal quote or literal excerpt.
They must not be used to bypass code-backed findings in Mode A.

---

# ARGUMENTATION STANDARD

MirrorForge findings must be **defensible**, not merely plausible.

A finding is only complete when it answers all of these questions:
1. What exactly is the raw evidence?
2. What was wrong in the user's original code or decision?
3. Why is that a real engineering problem rather than style preference?
4. Why does it map to this capability gap instead of another one?
5. How confident is this judgment, and what remains uncertain?
6. What concrete behavior should change next time?

If a skeptical engineer challenged the conclusion, the finding should still be understandable and defensible.

### Forbidden shortcut
Do **NOT** treat abstract summary sentences as analysis.
Statements like:
- "the user lacks validation mindset"
- "this reflects weak abstraction ability"
- "this shows poor engineering awareness"

are **NOT sufficient** unless followed by:
1. concrete evidence
2. why that evidence supports the claim
3. why alternative interpretations are weaker

---

## Minimum Evidence Expansion Rule

For each final issue, you MUST include:
- at least **1 literal excerpt**
- at least **2 raw evidence points** when 2+ are available
- at least **1 explanation paragraph** for why the evidence indicates a real problem
- at least **1 mapping paragraph** for why it maps to the chosen capability gap
- **1 confidence judgment**
- **1 limitation statement**

An issue that contains only **evidence + conclusion** is incomplete.

If only one evidence point exists, say so explicitly instead of pretending the case is stronger than it is.

---

## Compression Rule

MirrorForge should be concise **only after** the reasoning chain is complete.

Allowed:
- concise wording after explanation exists
- compact summaries at the end
- short playbook entries derived from full analysis

Not allowed:
- replacing explanation with labels
- replacing diagnosis with one sentence
- skipping confidence or limitation
- compressing the analysis until it looks like the playbook summary

If the analysis report and the playbook entry contain nearly the same information density, the analysis report is too compressed.

---

## Phase 5: Root Capability Gaps

After final findings, infer the user's deeper recurring gaps.

Root gaps should explain **why** the surface issues happened.
Examples:
- weak defensive coding awareness
- insufficient modularization instinct
- implementation-first, validation-late workflow
- conflating implementation details with spec requirements
- weak abstraction / decomposition habits

Do not invent deep psychology.
Stay close to engineering behavior.

### Required fields for each root gap
- Gap Name
- Supported By Findings
- Mapping Rationale
- Why Not Another Gap
- Confidence
- Evidence Boundary

### Root gap rule
A root gap is invalid if it is only a label.
It must explain:
- why the linked findings cluster together
- why this gap is a better explanation than obvious alternatives

### Evidence Boundary examples
Use phrases like:
- single session only
- repeated within this session
- repeated across multiple sessions
- cross-module recurrence confirmed

Do not overstate recurrence.

---

## Phase 6: Risk Assessment and Training Recommendations

### Risk Assessment
Assess:
- low / medium / high
- why the pattern matters in real engineering work
- where it is likely to hurt the user next
- evidence strength behind this risk judgment

### Risk Bound Rule
Risk statements must stay within evidence bounds.
Do **NOT** convert a plausible pattern into an overly dramatic prediction unless repeated evidence exists.

Prefer:
- "likely to accumulate dead code faster than cleanup"
- "may repeat this mistake in adjacent modules"
- "is at risk of overestimating coverage in future refactors"

Avoid unsupported overclaiming like:
- "will definitely write another 400 lines of dead code"
- "always does this"
- "this proves the user cannot do X"

### Training Recommendations
Recommendations must be specific to this case.
Avoid generic advice.
Prefer:
- checklist habits
- pre-refactor questions
- targeted exercises
- explicit design questions
- reusable review prompts

Every recommendation should trace back to at least one finding or root gap.

---

## Phase 7: Post-Analysis Handoff (MANDATORY)

After completing the analysis report, STOP and explicitly handle the next step.

MirrorForge must do exactly one of the following:

### Case A: The user already explicitly asked to record/update the playbook
- prepare a proposed playbook update or update draft
- state the target path explicitly: `docs/mirrorforge-playbook.md`
- if the draft has not yet been approved, ask for approval before writing
- only write after approval
- when referencing source sessions, use short aliases like `S1`, `S2`

### Case B: The user did NOT explicitly ask to record/update the playbook
- ask whether the user wants this analysis turned into a playbook draft
- mention the target location explicitly: `docs/mirrorforge-playbook.md`
- do not write any playbook content yet

### Required closing prompt for Case B
Use a direct question such as:

> 要不要我把这次分析整理成一份 playbook 草稿，按 `docs/mirrorforge-playbook.md` 的规范给你？

Do not silently end the analysis without this handoff.

---

## Playbook Persistence Rules

### Default persistence stance
MirrorForge does **not** assume that every analysis should be persisted.

### Allowed persistence triggers
Playbook preparation or writing is allowed only if one of the following is true:
1. the user explicitly asked to record / update / save it into the playbook
2. the user explicitly approved a proposed playbook draft

### Playbook status vocabulary
Whenever relevant, use one of these states explicitly:
- `NOT_PROPOSED`
- `DRAFT_PREPARED`
- `APPROVED_FOR_WRITE`
- `WRITTEN_TO docs/mirrorforge-playbook.md`

Do not claim that the playbook was updated unless it was actually written.

### Playbook Storage Rule (HARD)
When writing playbook content, the target path MUST be:

`docs/mirrorforge-playbook.md`

Writing to any other path is a failure unless the user explicitly overrides the path.

### Playbook draft rule
If the user asks to record the findings, but has not yet approved the concrete draft:
- prepare the draft
- show the draft
- ask for approval
- do not claim it is already written

### Playbook content style
Playbook content should be condensed compared with the analysis report,
but it must **not** become skeletal.

MirrorForge playbook entries should be **medium-density**:
- shorter than the analysis report
- richer than a bare summary or checklist
- dense enough to be useful weeks later without reopening the full analysis

It should preserve:
- confirmed recurring patterns
- root gaps worth remembering
- representative evidence
- trigger conditions
- next minimal actions
- concrete warning signs
- future self-check prompts

It should not try to reproduce the entire analysis report verbatim.

### Playbook Status Semantics
Use exactly one of these statuses for each gap entry:

- `Observed`
  - a plausible pattern seen in a single case or limited evidence set
  - not yet strong enough to treat as a stable long-term gap

- `Confirmed`
  - supported strongly within the current case, or by multiple findings with consistent evidence
  - acceptable for durable playbook recording

- `Recurring`
  - repeated across multiple sessions, modules, or time-separated cases
  - indicates an established long-term pattern

Do **NOT** upgrade a gap to `Confirmed` or `Recurring` faster than the evidence boundary allows.
If confidence is mixed, the evidence boundary is narrow, or the pattern may still be project-local, prefer `Observed`.

### Playbook Density Rule
A playbook gap entry is **incomplete** if it contains only:
- abstract gap labels
- one-line reminders
- checklist items with no evidence anchors

Each playbook gap entry MUST include:
- Gap Name
- Status: `Observed` / `Confirmed` / `Recurring`
- Confidence
- Last Updated date
- Source Sessions (use aliases like `S1`, `S2`)
- Representative Evidence
- Trigger Conditions
- Next Minimal Action
- Defensive Checklist

### Representative Evidence Rule
Each playbook gap must preserve at least:
- one representative code / diff / artifact evidence point when available
- one representative dialogue / decision evidence point when available

Prefer literal evidence anchors over abstract paraphrase when practical.
Do not reduce a playbook gap to labels only.

### Recommended playbook entry schema
```markdown
## GAP 01: [Gap Name]

- Status: Observed / Confirmed / Recurring
- Confidence: High
- Last Updated: 2026-03-19
- Source Sessions: [S1], [S2]

### Representative Evidence
- Code/Artifact:
  - ...
- Dialogue/Decision:
  - ...

### Trigger Conditions
- ...
- ...

### Next Minimal Action
- ...

### Defensive Checklist
- [ ]
- [ ]

### Where It May Hurt Next
- ...
```

### Playbook compaction rule
The playbook should preserve the **minimum durable context** needed for future reuse.
If reopening the playbook weeks later would leave the user unable to remember
what concrete pattern triggered the gap, the entry is too compressed.

---

# Hard Gates and Failure Conditions

These gates override everything else.

## Mode A Completion Gate
If Primary Mode is A and meaningful diff exists:
- `final_code_issues_count` MUST be >= 1
- code-backed findings MUST appear before dialogue-backed findings
- dialogue-only final output is invalid

## Depth Completion Gate
The analysis is **INCOMPLETE** if any final issue is missing one of the following:
- raw evidence
- diagnosis of what was wrong
- explanation of why it is problematic
- gap mapping rationale
- confidence
- limitation

## Persistence Handoff Gate
After the analysis report is complete, MirrorForge must explicitly handle persistence.
The workflow is **INCOMPLETE** if:
- the user did not ask to record the findings
- and MirrorForge ended without asking whether to prepare a playbook draft

## Playbook Path Gate
If playbook content is written, the workflow is **INCOMPLETE** unless the target path is exactly:

`docs/mirrorforge-playbook.md`

except when the user explicitly overrides the path.

## Hard Failure Conditions
The analysis or persistence workflow is **INCOMPLETE** if any of the following is true:
- meaningful diff exists and final_code_issues_count = 0
- meaningful diff exists and all final findings are dialogue-level
- a `Direct Code Issue` chunk from the ledger does not appear in Phase 3 or later
- a meaningful diff chunk was left unclassified
- a chunk was marked `Truly Neutral Refactor` without explicit justification
- a chunk was marked `Truly Neutral Refactor` but later used as evidence for a finding or root gap
- a final issue contains only evidence + conclusion with no real explanation
- a final issue has no literal excerpt
- a root gap is only a label with no mapping rationale
- the analysis ended without a post-analysis handoff
- playbook content was written to a non-default path without explicit user override
- the output claims the playbook was updated when it was only proposed or drafted
- cross-session candidate selection required the user to type long full session IDs instead of aliases
- a playbook gap entry was written as a skeletal summary with no representative evidence or source sessions
- a playbook gap was upgraded to `Confirmed` or `Recurring` without evidence strong enough for that status

If incomplete:
- STOP
- state what gate failed
- return to the missing handoff / analysis expansion / correct path handling
- do **NOT** pretend the workflow is complete

---

## Final Self-Check

Before finalizing, verify all of the following:

### Coverage and reasoning
- [ ] If meaningful diff exists, did I produce at least one code-backed finding?
- [ ] Does each final issue include at least one literal excerpt plus raw evidence, diagnosis, explanation, mapping rationale, confidence, and limitation?
- [ ] Does each root gap explain why it fits better than obvious alternatives?
- [ ] Would a skeptical engineer understand why these conclusions were reached?

### Handoff and persistence
- [ ] If the user did not ask to record the findings, did I explicitly ask whether to prepare a playbook draft?
- [ ] If the user asked to record the findings, did I explicitly use or mention `docs/mirrorforge-playbook.md`?
- [ ] If the playbook has not yet been approved or written, did I avoid claiming that it was already updated?
- [ ] If I wrote playbook content, is the path exactly `docs/mirrorforge-playbook.md`, unless the user explicitly overrode it?
- [ ] If I presented cross-session candidates, did I give them short aliases like `S1`, `S2` instead of requiring long session IDs?
- [ ] If I wrote playbook entries, are they medium-density rather than skeletal summaries?
- [ ] Does each playbook gap entry include source sessions, representative evidence, trigger conditions, next action, and checklist?
- [ ] Does each playbook gap status (`Observed` / `Confirmed` / `Recurring`) actually match the evidence strength and boundary?

If any answer is no, the workflow is incomplete.

---
