# NSQL Extensions

An **extension** specializes the host-agnostic NSQL core (`../core/`) for one
host system. NSQL ⊃ SQL: `relational/` — SQL-style data queries — is the
canonical extension, the special case the protocol is named after.

## What an extension is

An extension is a directory under `extensions/`, named for its host. It
supplies, for that host:

| Artifact | Purpose | Required |
|----------|---------|----------|
| `protocol.yaml` | concrete confirmation formats (rendering the core `read` / `operation` / `disambiguation` shapes) and concrete disambiguation triggers | yes |
| `grammar.ebnf` | the host's operation grammar — the closed operation set and its syntax | yes |
| `semantics.md` | formal semantics by citation — each operation mapped to a host operation that already has a specification (see `../core/semantics.md`) | yes |
| `dictionary.yaml` | host / domain vocabulary — terms, aliases, definitions | optional |

An extension's `protocol.yaml` `meta.extends` MUST point at
`../../core/protocol.yaml`.

## What an extension supplies, in §4 terms

(`§` references are to `../docs/concept.md`.)

- **A closed operation set** — the `function` core (§4.2). The host's verbs;
  finite; enumerated in the extension's `grammar.ebnf`.
- **Concrete disambiguation triggers** under the core taxonomy
  (`function_ambiguity` / `argument_vagueness` / `argument_ambiguity`).
- **A host-semantics citation** — `semantics.md` maps each operation to the
  host's own, pre-existing formal operation. NSQL invents no semantics (§4.4).

## Versioned extension

Adding a host operation is a deliberate, spec-bearing act: a new
`grammar.ebnf` production *plus* a new row in the extension's `semantics.md`
citation table, in a new version. This is *versioned formal extension*
(§4.5) — not the organic drift of natural language.

## Worked example — a non-relational extension

`../examples/email_archive_case_study.md` documents NSQL adopted for Apple
Mail archive operations (the `che-apple-mail-mcp` plugin). It is the
reference example of building an extension for a non-relational host:

- the operation set is mail operations (`archive`, `move`, `delete`, search),
  not relational algebra;
- the pipeline format ports directly: `Mail -> filter(...) -> sort(...)`;
- the disambiguation taxonomy ports — concrete triggers are re-derived for
  the mail domain (relational D1–D5 became mail E1–E5);
- it surfaces the **enforcement** gap — see below.

## Enforcement is the host's job

NSQL is a *protocol*, not a *runtime*. A perfectly-written confirmation
protocol can still be skipped by a host LLM under conversational load — NSQL
has no recourse of its own. An adopting host SHOULD bind each protocol phase
(`core/protocol.yaml` `workflow`) to a commitment device that makes a skipped
phase observable; the email case study uses a task-tracker entry per phase.
NSQL's responsibility ends at specifying the protocol — enforcing it is the
host's.
