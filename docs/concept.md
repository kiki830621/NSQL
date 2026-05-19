# NSQL: A Confirmation Protocol for Human-AI Communication

> **Version**: 1.2 (Whitepaper) — describes NSQL v4.0
> **Date**: 2026-05-19 (§4 added; re-scope)
> **Status**: Draft

## Abstract

Natural language is inherently ambiguous. Traditional approaches to human-AI communication either require humans to learn formal languages (high learning curve) or rely on AI to interpret intent without verification (hidden assumptions). This paper introduces NSQL, a confirmation protocol that inverts the traditional paradigm: instead of humans writing structured queries for AI to parse, AI outputs structured confirmations for humans to verify. This approach eliminates the learning curve while maintaining precision through explicit consensus-building.

**Key insight**: NSQL is not a language for humans to write. It is a format for AI to show what it understood, enabling humans to confirm or correct before execution.

---

## 1. Introduction

### 1.1 The Problem

When humans communicate with AI systems for data queries or operations, two fundamental challenges arise:

1. **Vagueness**: Terms with unclear boundaries (e.g., "recent", "high-value", "active")
2. **Ambiguity**: Terms with multiple discrete meanings (e.g., "sales" could mean revenue or quantity)

Vagueness and ambiguity describe the *kind* of unclarity. Orthogonal to it is the *location*: any misalignment sits in either the **function** (the operation requested) or the **argument** (the entity it acts on). Crossing the two axes:

| | Function (the operation) | Argument (the referent) |
|---|---|---|
| **Vagueness** (fuzzy boundary) | "tidy up these emails" | "high-value customers" |
| **Ambiguity** (discrete senses) | "handle this" — archive or delete? | "sales" — revenue or quantity? |

The location axis tells the protocol *where* to seek confirmation. The two locations behave very differently — §4 develops this into NSQL's architecture.

These challenges lead to a critical question: How can we ensure AI correctly understands human intent before taking action?

### 1.2 Why Traditional Approaches Fall Short

| Approach | Description | Limitation |
|----------|-------------|------------|
| **Formal Languages (SQL)** | Precise, unambiguous | High learning curve; excludes non-technical users |
| **Pure NLP** | No learning curve | Hidden assumptions; no verification before execution |
| **Interactive Q&A** | Clarifies through dialogue | Lacks structure; inconsistent experience |

### 1.3 The NSQL Solution

NSQL proposes a **confirmation protocol** where:

1. Humans speak naturally
2. AI parses and identifies potential ambiguities
3. AI presents a structured confirmation
4. Humans verify, correct, or approve
5. Only then does execution occur

This creates a **consensus loop** that eliminates ambiguity while requiring zero learning from users.

---

## 2. The Confirmation Protocol

### 2.1 Core Principle: AI Writes, Human Reads

The fundamental inversion:

```
Traditional:  Human ──(learns formal language)──> AI ──> Execute
NSQL:         Human ──(natural language)──> AI ──(structured confirmation)──> Human ──> Execute
```

This design choice has three critical implications:

1. **Zero learning curve**: Users never need to learn syntax
2. **Verifiable understanding**: Users see exactly what AI understood
3. **Iterative refinement**: Errors are caught before execution

### 2.2 The Confirmation Loop

```
┌─────────────────────────────────────────────────────────────┐
│                    Confirmation Loop                         │
│                                                              │
│   Human                          AI                          │
│     │                             │                          │
│     │  Natural language request   │                          │
│     │ ─────────────────────────>  │                          │
│     │                             │  Parse & analyze         │
│     │                             │                          │
│     │  Structured confirmation    │                          │
│     │ <─────────────────────────  │                          │
│     │                             │                          │
│     │  Confirm / Correct          │                          │
│     │ ─────────────────────────>  │                          │
│     │                             │                          │
│     │  (Iterate if needed)        │                          │
│     │ <────────────────────────>  │                          │
│     │                             │                          │
│     │  Approved                   │                          │
│     │ ─────────────────────────>  │                          │
│     │                             │  Execute                 │
│     │  Results                    │                          │
│     │ <─────────────────────────  │                          │
└─────────────────────────────────────────────────────────────┘
```

### 2.3 Confirmation Formats

NSQL defines three primary confirmation formats:

#### Query Confirmation

```
I understand you want:

transform {source} to {result}
as {operations}
[grouped by {dimensions}]
[where {conditions}]
[ordered by {sort}]
[limit {n}]

Is this correct?
```

#### Operation Confirmation

```
I will perform:

{action} on {target}
with {parameters}

This will affect {N} records. Proceed?
```

#### Disambiguation Options

```
"{term}" could mean:

1. {interpretation_1} (Recommended)
2. {interpretation_2}
3. {interpretation_3}

Which do you mean?
```

---

## 3. Design Principles

### 3.1 Structured but Readable

The confirmation format must be:
- **Formal enough** to be unambiguous
- **Natural enough** for non-technical users to understand

This is achieved through:
- English-like keywords (`transform`, `grouped by`, `where`)
- Clear visual structure (indentation, line breaks)
- No technical jargon (no JOINs, no subqueries)

### 3.2 Explicit over Implicit

Every assumption must be surfaced:
- Time ranges made explicit (`2024-11-01 to 2024-11-30`)
- Aggregations stated (`sum`, `average`, `count`)
- Filters clearly shown (`where status = 'active'`)

**Anti-pattern**: Silently assuming "last month" means calendar month.
**NSQL approach**: Ask user to choose between calendar month and rolling 30 days.

### 3.3 Reversible Consensus

Until the user explicitly confirms:
- Nothing is executed
- The interpretation can be modified
- Additional context can be incorporated

This principle ensures **no irreversible action occurs based on a misunderstanding**.

---

## 4. The Function/Argument Model

> Added 2026-05-19. This section is NSQL's architecture. The repository is structured to match it — a host-agnostic `core/` and per-host `extensions/` (relational SQL-query is the canonical one, `extensions/relational/`).
>
> **On the name.** NSQL ⊃ SQL. NSQL is the general confirmation protocol; relational SQL-query is its *canonical special case* — the one the protocol is named after, shipped as `extensions/relational/`. The `SQL` in "NSQL" names a real, present component, not a historical origin.

### 4.1 Every request is a function over arguments

A request to operate on a computer decomposes into a **function** — the operation to perform — and its **arguments** — the entities it acts on. *"Archive the marketing emails from last quarter"* is the function `archive` applied to the argument *the marketing emails from last quarter*.

This is the Fregean decomposition (function + argument), not the classical subject/predicate one. The distinction is load-bearing: subject/predicate cannot express *multiple generality* ("every customer's largest order"); function/argument with quantifier scope can. NSQL's grammar is built on function/argument for this reason.

### 4.2 The closed core and the open frontier

Function and argument are not symmetric. They sit on opposite sides of a seam:

| | **Function** (the operation) | **Argument** (the referent) |
|---|---|---|
| Domain | **Closed** — a computer performs finitely many kinds of operation | **Open** — human concepts, resolved into machine values |
| Meaning (sense) | **Pinned** by a formal semantics | **Open** — varies with user, domain, and purpose |
| Resolved by | formal semantics (§4.4) | the confirmation loop (§2) |
| Termination | **Guaranteed** — a pick from a finite set | **Local only** — each turn closes; the sense space never does |

The function side is closed because the set of operations a computer can perform is finite and enumerable. The argument side is open because "high-value customer" is not a thing inside the computer — it is a human concept that must be *negotiated* into one.

This is why a formal semantics and the §2 confirmation loop coexist: **the closed core (operations) carries a formal semantics; the open frontier (arguments) is resolved by confirmation.** Function/argument is the seam between them.

### 4.3 Vagueness and ambiguity across the seam

§1.1 distinguished vagueness (fuzzy boundary) from ambiguity (discrete senses). The closed/open seam predicts where each lives:

- **The closed operation domain acts as a vagueness→ambiguity converter.** A vague verb ("tidy up these emails") cannot stay vague: projected onto the finite operation set, it becomes a discrete choice ("archive? delete? sort?"). Function unclarity is therefore always *ambiguity-shaped* — resolved by a pick from a closed menu.
- **The argument domain has no such converter.** "High-value" has no finite menu to collapse onto, so its vagueness *stays* vagueness — resolved not by a 1/2/3 pick but by forcing an explicit value and negotiating it.

Consequently, **true (open) vagueness lives only in arguments.** Arguments may also carry ambiguity ("sales" = revenue or quantity, a discrete pick); functions, in practice, carry only ambiguity.

### 4.4 Formal semantics by citation

The function core can be given a formal semantics — but NSQL does not invent one. The operations it renders act on systems that already have precise semantics: relational algebra for queries, POSIX for files, defined operations for email. NSQL's semantics is a thin mapping from each NSQL construct to a host-system operation:

```
NSQL  transform X to Y where C   →  relational algebra
NSQL  archive email E            →  the mail system's archive operation
```

NSQL does not define meaning; it *cites* it. This makes "a formal semantics for NSQL" a bounded mapping exercise rather than an open research effort.

### 4.5 Versioned extension

The operation set is closed *per version*, not closed forever. A new host system — a new file operation, a new tool — extends it. But the extension is deliberate and spec-bearing: a new operation is a new function symbol *plus* its cited semantics, in the manner of successive SQL standards. This is the role of `extensions/`: **versioned formal extension**, not the organic drift of natural language. The operation grammar (`grammar.ebnf`) is therefore normative — a complete specification of the operation core at each version, not a sketch.

### 4.6 What NSQL bounds, and what it does not

NSQL restricts its **rendering** domain to function/argument. It does **not** restrict its **input** domain — humans still speak naturally; that is the protocol's core promise (§2.1).

The discipline is honesty about the gap. When part of a request cannot be operationalized — typically its *purpose*, the context within which it should be read (its **horizon**) — that residue must be **explicitly marked**, not silently dropped. A confirmation must distinguish what NSQL operationalized from what it could not, and is handing back to the human.

---

## 5. Comparison with Alternatives

| Criterion | SQL | Natural Language | NSQL Protocol |
|-----------|-----|------------------|---------------|
| Learning Curve | High | None | None |
| Precision | High | Low | High |
| Verifiability | Low | Low | High |
| User Experience | Technical | Natural | Natural + Verified |
| Error Discovery | After execution | After execution | Before execution |

### Why NSQL Outperforms

1. **vs SQL**: Same precision, no learning required
2. **vs Pure NLP**: Same ease of use, but with verification
3. **vs Interactive Q&A**: Structured format ensures consistency

---

## 6. Conclusion

NSQL represents a paradigm shift in human-AI communication: instead of forcing humans to speak the machine's language, we enable machines to show their understanding in a human-readable form. This confirmation protocol achieves:

1. **Zero learning curve** through natural language input
2. **High precision** through structured confirmation
3. **Verifiable consensus** through explicit approval

The key innovation is recognizing that the bottleneck in human-AI communication is not parsing—modern AI can parse natural language well—but **verification**. By making AI's interpretation visible and confirmable, NSQL eliminates the gap between what humans mean and what AI understands.

---

## References

<!-- TODO: Expand with full citations for academic version -->

- Austin, J.L. (1962). *How to Do Things with Words*. Oxford University Press.
- Clark, H.H. (1996). *Using Language*. Cambridge University Press.
- Frege, G. (1879). *Begriffsschrift*. (Function/argument analysis; §4.1.)
- Grice, H.P. (1975). Logic and Conversation. In *Syntax and Semantics*, Vol. 3.

---

## Appendix: Future Expansion Notes

<!-- These sections are placeholders for v2.0 academic paper -->

### A. Theoretical Background (v2.0)

To be expanded with:
- Speech Act Theory (Austin, Searle)
- Common Ground Theory (Clark, Brennan)
- Relevance Theory (Sperber, Wilson)
- Pragmatics and conversational implicature

### B. Evaluation (v2.0)

To be added:
- User study comparing NSQL vs alternatives
- Error rate analysis
- Time-to-completion metrics
- User satisfaction scores

### C. Extended Use Cases (v2.0)

To be explored:
- Multi-turn complex queries
- Cross-domain applications
- Non-English language support
- Real-time streaming data

---

*Version 1.0 | 2025-12-24*
