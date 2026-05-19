# NSQL Relational Extension — Semantics

> NSQL v4.0.0. The relational instance of `../../core/semantics.md`
> (formal semantics by citation).

## Host

The host is a **relational data store**. Its formal semantics is **relational
algebra** (Codd) — operations over sets of tuples. NSQL does not redefine it;
the tables below *cite* it.

## Read constructs — pipeline ⟼ relational algebra

| NSQL construct (pipeline)  | Relational-algebra denotation |
|----------------------------|-------------------------------|
| `{source}`                 | a relation R (a set of tuples) |
| `filter(C)`                | selection — σ(R) with condition C |
| `select(cols)`             | projection — π(R) onto `cols` |
| `join(S, on: k)`           | join — R ⋈ S on key `k` |
| `group(D) -> aggregate(A)` | grouping with aggregation — the extended-algebra γ operator: group by `D`, aggregate `A` |
| `sort(spec)`               | output **ordering** — not a relational operator; acts on the rendered result |
| `limit(n)`                 | output **truncation** — not a relational operator; acts on the rendered result |

`sort` and `limit` are *presentation* operations. They are kept in the
grammar because the confirmation must show them, but they have no
relational-algebra denotation — a relation is a set, and a set is unordered.

## sql_like ⟼ pipeline

The `sql_like` format is sugar. `transform S to T as O grouped by D where C`
denotes the same algebra as the pipeline `S -> filter(C) -> group(D) ->
aggregate(O)`. Operation order in `sql_like` is *implicit* (WHERE before
GROUP BY); the pipeline makes it explicit. Where order is ambiguous, prefer
the pipeline — see trigger D5.

## Operation constructs ⟼ relational data-modification

| NSQL `operation` construct          | Denotation |
|-------------------------------------|------------|
| `UPDATE on T with A affecting C`     | `UPDATE T SET A WHERE C` |
| `DELETE on T affecting C`            | `DELETE FROM T WHERE C` |
| `INSERT on T with V`                 | `INSERT INTO T VALUES V` |

These mutate the store.

## Pre/post-conditions the confirmation MUST surface

Per `../../core/semantics.md`, each operation names the conditions a
confirmation must make visible:

- **`operation` (any write)** — the estimated **affected-row count** and
  whether the change is **reversible**. Rendered by the `operation` shape.
- **`join`** — the **join key**. A wrong key silently changes the result set;
  the confirmation must show it.
- **`group` / `aggregate`** — the **aggregate function** (sum vs count vs avg).
  Left implicit, it is a frequent misread — trigger D4.

## Versioned extension

A new relational construct is added by a new `grammar.ebnf` production *plus*
a new row in the tables above, in a new version (`concept.md` §4.5). The
citation tables above are the normative semantics of this extension.
