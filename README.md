# NSQL: Human-AI Confirmation Protocol

A structured format an AI uses to confirm its understanding of a request
before acting — eliminating ambiguity through a short dialogue.

> **Key insight**: NSQL is not a language humans need to learn. It is a format
> the AI uses to show what it understood, so humans can confirm or correct
> before anything runs.

> **NSQL ⊃ SQL.** NSQL is a *host-agnostic* confirmation protocol. Relational
> SQL-style data queries are its **canonical special case** — the one the
> protocol is named after — shipped as `extensions/relational/`. The `SQL` in
> the name points at a real component, not a limit on scope.

## How It Works

```
┌─────────────────────────────────────────────────────────────────┐
│                       Confirmation Loop                          │
│   Human                          AI                              │
│     │   natural-language request  │                              │
│     │ ───────────────────────────>│  parse → function + arguments │
│     │                             │  identify ambiguity           │
│     │     structured confirmation │                              │
│     │ <───────────────────────────│                              │
│     │   "對" / "不對，改成..."      │                              │
│     │ ───────────────────────────>│  execute or iterate           │
└─────────────────────────────────────────────────────────────────┘
```

A request decomposes into a **function** (the operation) and its
**arguments** (what it acts on). The AI renders that decomposition as a
confirmation; the human confirms or corrects; only then does it run.

## Example (relational host)

**User**: 找出高價值客戶

**AI**: 「高價值客戶」有幾種定義：

1. RFM 分數 ≥ 4（推薦）
2. 年消費 > $10,000
3. Top 10% 消費者

您想用哪一個？

**User**: 1

**AI**: 我理解您要的是：

```
Customers
  -> filter(rfm_r >= 4 AND rfm_f >= 4 AND rfm_m >= 4)
```

這樣對嗎？

**User**: 對 → [執行查詢]

## Architecture

NSQL is a **host-agnostic core** plus per-host **extensions**.

```
nsql/
├── core/                   # the host-agnostic protocol
│   ├── protocol.yaml        #   confirmation loop, format shapes,
│   │                        #   disambiguation taxonomy, workflow
│   └── grammar.ebnf         #   the confirmation envelope (function/argument)
│
├── extensions/             # per-host specializations
│   └── relational/          # SQL-style data queries — the canonical case
│       ├── protocol.yaml    #   pipeline / sql_like / operation formats
│       ├── grammar.ebnf     #   relational grammar
│       └── dictionary.yaml  #   relational + business vocabulary
│
├── docs/
│   ├── concept.md           # whitepaper — §4 is the function/argument model
│   └── guide.md             # implementation guide
│
├── examples/                # dialogue examples
│   └── email_archive_case_study.md   # a non-relational adoption (Apple Mail)
│
├── VERSION                  # 4.0.0
└── archive/                 # historical specifications
```

## Documentation

| Document | Description |
|----------|-------------|
| [Concept Paper](docs/concept.md) | Theory — incl. §4, the function/argument model |
| [Implementation Guide](docs/guide.md) | How to adopt NSQL in an AI system |
| [`core/protocol.yaml`](core/protocol.yaml) | The host-agnostic protocol |
| [`extensions/relational/`](extensions/relational/) | The canonical SQL-query extension |

## Key Principles

1. **Read-Only for Humans** — users confirm/correct, they don't write NSQL.
2. **Explicit over Implicit** — every operation and assumption is visible.
3. **Dynamic Confirmation Loop** — consensus is reached through dialogue.
4. **Clarify before execute** — never guess.

## When to Use NSQL

The shape NSQL fits: a **vague natural-language request that triggers an
operation with real consequences** — data queries, bulk updates, file or
email operations, anything irreversible at scale. Skip it for precise,
single-resource, read-only actions.

---

*NSQL v4.0.0 — host-agnostic confirmation protocol*
