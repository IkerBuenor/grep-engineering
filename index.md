---
title: GREP Engineering — Protocol
description: Replacing LLM memory with deterministic search. A 1973 Unix technique for 2026 AI workflows.
---

# GREP Engineering

> *Replacing LLM memory with deterministic search.*

**By Iker Alvarez Neira** · [grepengineering.com](https://grepengineering.com)

---

## The Problem

LLMs forget items in long lists. Not because they're bad — because **memory and search are fundamentally different tools**.

A transformer model verifying 60 items against another document consumes ~10,000 tokens with ~9% omission rate for items in the middle of the context window. With GREP, the same verification costs ~900 tokens with **0% omission rate**.

That's a **91% token reduction** — not from a better model, but from using the right tool for the right job.

| Metric | Without GREP | With GREP | Reduction |
|--------|-------------|-----------|-----------|
| Tokens per 60-item verification | ~10,000 | ~900 | **91%** |
| Omission rate | ~9% | 0% | **100%** |
| Correction cycles needed | 1–2 | 0 | **100%** |

At 10,000 users / one verification per week:
- Without GREP: 100M tokens/week
- With GREP: 9M tokens/week
- **Delta: 91M tokens saved weekly (~$273,000/week at Sonnet pricing)**

---

## The Protocol

### Activation triggers

Activate with any of the following:

- `grep check`
- `grep-check`
- `verify nothing is missing`
- `cross-check documents`
- `coteja` *(Spanish shorthand)*
- `coteja esto` *(Spanish shorthand)*
- `comprueba que no falte nada` *(Spanish shorthand)*
- `verifica que no falta nada` *(Spanish shorthand)*

Apply **preventively** before delivering any generated document, without being asked.

---

### STEP 0 — Verify the reference document is complete

Before starting, ask: *are there decisions made in this conversation not yet recorded in any file?*

Maintain a `decisions_pending.txt` file updated at the end of each session. If it exists, read it before Step 1 and incorporate its contents into the reference document.

A verification against an outdated reference passes all checks but misses the most recent decisions. This is the most common failure mode.

---

### STEP 1 — Extract all items from the reference document

Read the reference document in full. Extract **ALL** items using grep or structured search.

- If the document has structured IDs (F1.1, E14, R3): extract by ID
- If no IDs: extract by the first 4–5 unique words of each item
- Do not summarize or infer — exhaustive list, no exceptions
- If volume exceeds 200 items: split by sections, report each separately

---

### STEP 2 — Bidirectional mechanical verification

**Direction A — what's missing:**
For each reference item, grep against the working document. If not found by ID or exact text, run a semantic search with key terms before declaring it absent. May be present under a different label.

Status codes:
- ✅ present
- ❌ absent
- ⚠️ partial or different label
- 🔄 merge with X *(when absorbed by a more complete item)*

**Direction B — what's orphaned:**
For each working document item, verify it has a match in the reference document. Flag orphaned items for removal, merge, or reclassification — they may have been cancelled or superseded.

---

### STEP 2B — Optional: verify against code *(activate with "grep-check code")*

When you need to verify that documented tasks are actually implemented:
grep the IDs or module names from the working document against the repository files.

Detects two cases:
- **(A)** Tasks marked complete in the document but absent from the code
- **(B)** Code implemented but not documented in the plan

```bash
# Example: verify multiple modules at once
for term in "watermark" "fingerprint" "bulk_delete" "app_settings"; do
  echo "=== $term ===" && grep -r "$term" ./backend ./frontend --include="*.py" --include="*.jsx" -l
done
```

---

### STEP 3 — Report before modifying

Show the complete table with three columns: **item / status / note**.

Include both missing items (Direction A) and orphaned items (Direction B). If items have status 🔄, propose which item to merge with before confirming.

**Wait for explicit confirmation before proceeding.**

If all absent items are clearly the same type, the user may confirm with `"all"`.

---

### STEP 4 — Modify and second pass

Add absent items and manage orphans per confirmation, without modifying what must remain.

Run a **second grep pass** to verify:
- Every added item is physically in the file
- Every removed item no longer appears

Report only after the second verification:
> *"X items checked, Y present, Z added, W orphans managed, 0 pending."*

---

## Session Tags — Recoverable by Search

Structure your conversations with these tags. Claude retrieves all three in future sessions using the chat history search tool.

```
[DECISION: X]  — decision made that affects the project or workflow
[INSIGHT: X]   — moment of discovery with emotional or strategic impact
[IDEA: X]      — new undeveloped idea to explore later
```

**Rules:**
- Use exact format: square brackets, uppercase tag, colon, space
- Claude stores all three in `INSIGHTS.md` with date when appropriate
- Enables grep-based retrieval across sessions without burning tokens on full context recall
- These tags make semantic search more precise by anchoring embeddings

**Example:**
```
[DECISION: use Flux Kontext Pro for Smart Text pipeline, two separate calls]
[INSIGHT: GREP Engineering applies to any indexable system, not just documents]
[IDEA: GREP-check for code — LLM maps repo structure via grep before analyzing]
```

---

## Robustness Note

This protocol is stored both in user preferences and as a file in the project repository.

**If there is a conflict between the preferences version and the repository version, the repository version prevails.**

If the repository file does not exist, create a `.txt` with this protocol and add it to the project.

---

## The Origin

grep was created by **Ken Thompson** in 1973 as part of Unix at Bell Labs.

*(Note: grep was **not** created by Linus Torvalds. Linus created Linux in 1991. They are different people, different decades.)*

The same search mechanism that indexes the world's servers now verifies AI workflow documents.

The insight: **the problem was never the model's capability. It was using memory where search was needed.**

---

## See It In Action

Built while developing [FORMA](https://shape-builder-12.preview.emergentagent.com) — an AI image generation platform built entirely with Emergent and Claude. Free account available.

Two months. Zero prior coding experience. Linux, Claude, Emergent.

→ [GLOSSARY.md](GLOSSARY.md) — grep-able term index
→ [INSIGHTS.md](INSIGHTS.md) — discovery log
→ [GitHub](https://github.com/IkerBuenor/grep-engineering)

---

*Iker Alvarez Neira · 2026 · [grepengineering.com](https://grepengineering.com)*
