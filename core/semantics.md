# NSQL Core — Formal Semantics by Citation

> NSQL v4.0.0. The model behind `docs/concept.md` §4.4.

## The principle

NSQL's **function** core is closed and formally specifiable (`concept.md`
§4.2). But NSQL does **not** invent a semantics. Every operation it renders
acts on a host system that *already has* a precise semantics — relational
algebra for a relational store, POSIX for a filesystem, a defined operation
set for a mail client.

NSQL's semantics is therefore a **thin citation**: a mapping from each NSQL
construct to an operation in the host's own, pre-existing semantics.

```
NSQL construct        ⟼   host operation  (already formally defined elsewhere)
```

NSQL does not define meaning; it *cites* it. This keeps "a formal semantics
for NSQL" a bounded mapping exercise, not an open research effort.

## What an extension must supply

Each extension provides a `semantics.md` that, for every operation in its
`grammar.ebnf`, names:

1. **the host operation** it denotes;
2. **the host's specification** of that operation — a citation (a standard,
   an algebra, a man page), not a fresh definition;
3. any **pre/post-conditions** the confirmation must make visible (e.g. an
   affected-row count, a reversibility flag).

The canonical instance: `../extensions/relational/semantics.md`.

## Why this is sound

- The function set is closed per host (§4.2) → the citation table is finite.
- The host's semantics already exists and is authoritative → NSQL inherits
  rigor without re-deriving it.
- A new host operation is added by *extending the citation table* in a new
  version — versioned formal extension (§4.5), not organic drift.

## What this document does NOT cover

This model covers the **function** side only. Argument resolution — binding a
human concept ("high-value") to a concrete host value — has no closed
semantics; it is resolved by the confirmation loop (`core/protocol.yaml`).

What cannot be operationalized at all — the *purpose* or horizon of a request
— is marked as **residue**, never silently dropped. See the `residue` entry
under `confirmation_format_shapes` in `core/protocol.yaml` (§4.6).
