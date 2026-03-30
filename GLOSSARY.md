# GREP Engineering — Glossary

> Every term is grep-able. Usage: `grep "GREP-001" GLOSSARY.md`

---

GREP-001: GREP Engineering — Practice of replacing LLM memory with deterministic search in AI-assisted workflows. A 1973 Unix technique applied to 2026 AI systems.

GREP-002: Deterministic search — Search that returns the same result every time given the same input, regardless of model state, context length, or session history. Unlike LLM recall, it does not degrade.

GREP-003: Token reduction — 91% fewer tokens per verification cycle compared to memory-based LLM verification. At 10,000 users/week: 91M tokens saved, ~$273,000/week systemic savings.

GREP-004: Bidirectional verification — Direction A checks what is missing from the working document. Direction B checks what is orphaned (present in working document but absent from reference). Both directions are required for complete verification.

GREP-005: Context window drift — The tendency of transformer models to lose recall accuracy for items positioned in the middle of long contexts. Items at the beginning and end are recalled more reliably. This is architectural, not a bug.

GREP-006: [DECISION] tag — Structured label format `[DECISION: X]` for decisions made in chat conversations. Recoverable by search in future sessions. Stored in INSIGHTS.md with date.

GREP-007: [INSIGHT] tag — Structured label format `[INSIGHT: X]` for discovery moments with emotional or strategic impact. Stored in INSIGHTS.md with date and context.

GREP-008: [IDEA] tag — Structured label format `[IDEA: X]` for new undeveloped ideas to explore later. Stored in INSIGHTS.md with date.

GREP-009: Orphaned item — An item present in the working document but absent from the reference document. May indicate a cancelled task, a superseded decision, or a documentation error. Flagged in Direction B of the protocol.

GREP-010: Flush-cache — Immediate cache invalidation endpoint. Equivalent to forcing a fresh search instead of using cached recall. In GREP Engineering: the second verification pass that confirms changes were actually written.

GREP-011: Ken Thompson — Creator of grep (1973, Bell Labs, Unix). Co-creator of Unix and the C programming language. Not to be confused with Linus Torvalds, who created Linux in 1991.

GREP-012: Deterministic consensus — Concept from blockchain systems where verification relies on cryptographic proof rather than trust or memory. Conceptual ancestor of GREP Engineering's approach to AI workflow verification.

GREP-013: GREP-check — Application of GREP Engineering to code repositories. The LLM maps the repository structure via grep before analyzing, reducing the context needed and improving error detection. Extension of the core methodology to software development workflows.

GREP-014: Session tags — Structured labels ([DECISION], [INSIGHT], [IDEA]) that make semantic search more precise by anchoring embeddings to structured content. Enable grep-based retrieval across sessions without consuming tokens on full context recall.

GREP-015: Prior art — Public timestamped publication that establishes authorship of a methodology. The GitHub commit timestamp serves as prior art for GREP Engineering, providing legal evidence of first publication without requiring a patent.

---

*Usage examples:*
```bash
grep "token reduction" GLOSSARY.md
grep "GREP-011" GLOSSARY.md
grep "deterministic" GLOSSARY.md
```
