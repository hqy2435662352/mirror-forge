[简体中文](README.zh-CN.md) | **English**

# MirrorForge

**Turn AI-assisted coding sessions into structured engineering growth.**

MirrorForge is a reflection skill for AI-assisted coding workflows.  
Most AI coding tools help fix code. MirrorForge helps explain what your **original code, decisions, and habits** revealed about your engineering gaps — then turns one-time AI assistance into reusable growth signals.

> **Analyze the user, not the agent.**

---

## Why MirrorForge exists

AI coding assistants are great at getting you unstuck.

But they often leave the most important question unanswered:

> **What did my original implementation reveal about how I work?**

A bug fix can hide recurring patterns such as:

- weak defensive coding around boundaries and assumptions
- implementation-first, validation-late workflow
- shallow modularization or unclear separation of concerns
- overly optimistic risk judgment during refactors
- repeated engineering habits that quietly reappear across sessions

MirrorForge exists to turn AI-assisted coding from a short-term productivity boost into a long-term learning loop.

---

## What it is

MirrorForge is an **evidence-first reflection skill** for AI-assisted coding sessions.

It is designed to answer questions like:

- What engineering problems did my original code expose?
- What capability gaps did this refactor or debugging session reveal?
- What recurring patterns are starting to show up across sessions?
- What training direction should follow from this case?
- Should this case be turned into a reusable playbook draft?

---

## What it is not

MirrorForge is **not**:

- a normal code review tool
- a generic coaching prompt
- a tool for praising the final AI-generated solution
- a summary generator that outputs abstract labels without evidence

It does **not** primarily evaluate whether the final code is “good.”

Its job is to analyze the **developer signal** revealed by AI assistance.

---

## Core idea

MirrorForge focuses on:

- the user's old implementation
- the user's decision patterns
- the user's engineering habits revealed during AI-assisted coding

That means the center of gravity is not:

- “the AI refactor was elegant”
- “the final code looks cleaner”
- “here are some generic best practices”

The question is always:

> **What was exposed about the developer through this case?**

---

## How it works

MirrorForge uses the **strongest available evidence first**.

### Evidence hierarchy

1. **Diff-backed code evidence**  
   The strongest source. Best when the user's original code and the AI-improved code can be compared directly.

2. **Single code snapshot**  
   Valid when diff is unavailable, but weaker than before/after evidence.

3. **Dialogue / decision evidence**  
   Useful for process, prioritization, and judgment patterns — but it does not replace code-backed findings when meaningful diff exists.

### Analysis modes

MirrorForge selects exactly one primary mode:

- **Mode A — Diff-backed Code Reflection**  
  Use when meaningful diff exists. Code-backed findings are mandatory.

- **Mode B — Code Snapshot Reflection**  
  Use when code exists but before/after diff is unavailable.

- **Mode C — Dialogue-only Reflection**  
  Use only when no usable code evidence exists.

### What makes the workflow different

MirrorForge is intentionally strict:

- it does not allow diff analysis to stop at chunk logging or vague takeaways
- it does not mistake a short conclusion for a real analysis
- it requires a full reasoning chain from evidence to gap mapping
- it keeps persistence separate from analysis

A valid finding should move through a chain like:

**raw evidence → what was wrong → why it is problematic → why it maps to this gap → confidence and limitation → concrete fix pattern**

---

## Why it feels different from ordinary reflection prompts

Most reflection prompts do one of two things:

- summarize the final code
- generate abstract labels like “weak validation mindset”

MirrorForge is built to resist both.

Its workflow is designed around:

- **evidence-first diagnosis**
- **explicit reasoning chains**
- **hard gates against shallow summaries**
- **bounded claims instead of overconfident generalization**
- **playbook-ready output without assuming auto-persistence**

That makes it less like a vibe-based coach and more like a reflective engineering audit.

---

## Quick example

You ask an AI assistant to help fix a messy refactor.

The code gets corrected.

A typical assistant might stop at:

> “The refactor is cleaner now and validation has been improved.”

MirrorForge goes further and asks:

- What was wrong in the original version?
- Was that merely style, or a real engineering risk?
- What behavior pattern produced that code?
- Is this a one-off issue or part of a recurring capability gap?

A simplified output might look like this:

### Code-backed finding

The original implementation mixed domain assumptions with shortcut branching logic, causing missing validation and brittle control flow.

### Root gap hypothesis

**Implementation-first, validation-late workflow**

### Why this gap fits

The issue is not only “forgot to validate input.”  
The deeper pattern is that branch logic was written before boundary assumptions were stabilized.

### Risk

Likely to overestimate correctness in adjacent refactors where assumptions feel “obvious.”

### Next minimal action

Before writing branch logic, force a short validation checklist:

- What assumptions am I making about shape, nullability, and type?
- Which assumptions are guaranteed by the caller, and which are only hoped for?
- What would break first if those assumptions are false?

---

## Examples

MirrorForge is easier to understand when you can see both a short example and a full end-to-end case.

- [Quick example](docs/examples/quick-example.md) — a one-page walkthrough of the core workflow
- [Full analysis report](docs/examples/parsegsdfile-analysis-report.md) — a real Mode A case with inventory, diff ledger, findings, and gap mapping
- [Generated playbook](docs/examples/parsegsdfile-playbook.md) — the playbook produced from that full case


## Playbook handoff

MirrorForge treats **analysis** and **persistence** as separate commitments.

That means:

- it may complete an analysis automatically
- but it should **not assume** that every analysis should be saved
- after the analysis, it explicitly asks whether the case should be turned into a playbook draft

When approved, the target path is:

```text
docs/mirrorforge-playbook.md
```

The goal of the playbook is not to store full reports forever.  
It is to preserve the **minimum durable context** needed to remember:

- which gap was observed
- how confident the diagnosis was
- what evidence anchored it
- what warning signs to watch for next time
- what concrete defensive habit should follow

### Example playbook entry

```md
## GAP 01: Weak Boundary Defense

- Status: Observed
- Confidence: Medium
- Last Updated: 2026-03-19
- Source Sessions: [S1]

### Representative Evidence
- Code/Artifact:
  - Branch logic assumed `input.data` existed and had usable structure before shape validation was complete.
- Dialogue/Decision:
  - The implementation focus stayed on making the main path work before boundary assumptions were made explicit.

### Trigger Conditions
- Refactors involving parsing, external input, or implicit structure assumptions
- Situations where “this field should exist” is treated as sufficient validation

### Next Minimal Action
- Add a pre-branch validation pass before any control-flow expansion.

### Defensive Checklist
- [ ] Did I verify shape, nullability, and type before using the field?
- [ ] Am I depending on an assumption that only exists in my head?
- [ ] If this assumption fails, where is the first unsafe use?

### Where It May Hurt Next
- Parser refactors
- Data transformation helpers
- Boundary-heavy integration code
```

---

## Installation

### Claude Code

Clone the repository into your local skills directory:

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/<your-name>/mirrorforge.git
```

Expected path:

```text
~/.claude/skills/mirrorforge/SKILL.md
```

You can then invoke it in a workflow such as:

```text
/mirror-forge What engineering problems did this debugging session reveal about me?
```

### Other agent environments

MirrorForge is designed as a reusable workflow spec and can be adapted to other agent environments that support custom skills, system prompts, or instruction modules.

---

## When to use it

Use MirrorForge when the real question is not:

> “What is wrong with this code?”

but rather:

> “What did this case reveal about my engineering gaps?”

Typical use cases:

- AI-assisted refactors
- debugging sessions
- bug fixing
- compilation-error resolution
- optimization work
- cross-session review of recurring patterns
- preparing a growth playbook from multiple cases

---

## Repository structure

```text
mirrorforge/
├─ README.md
├─ README.zh-CN.md
├─ SKILL.md
└─ examples/
   ├─ quick-example.md
   ├─ parsegsdfile-analysis-report.md
   └─ parsegsdfile-playbook.md
```

Suggested roles:

- `README.md` — project overview, positioning, installation, and usage
- `README.zh-CN.md` — Chinese version of the project overview
- `SKILL.md` — full execution protocol and hard gates
- `examples/quick-example.md` — a short walkthrough for first-time readers
- `examples/parsegsdfile-analysis-report.md` — full analysis report example
- `examples/parsegsdfile-playbook.md` — generated playbook example
---

## Design philosophy

MirrorForge is built around a simple belief:

> **AI coding should not only improve the code. It should also improve the developer.**

The most valuable part of an AI-assisted session is not always the fix itself.

Sometimes it is the pattern the fix exposes.

MirrorForge tries to make those patterns visible, structured, and reusable.

---

## Roadmap

Potential future directions:

- richer cross-session pattern synthesis
- language-specific reflection heuristics
- better playbook compaction rules
- example packs for different engineering domains
- integration templates for more agent environments

---

## Contributing

Contributions are welcome, especially around:

- realistic reflection examples
- language-specific evidence patterns
- stronger playbook schemas
- better host-environment integration guides
- improvements to analysis reliability and anti-drift rules

---

## License

MIT
