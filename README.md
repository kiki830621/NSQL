# Natural SQL Language (NSQL)

A conceptual framework for natural language to SQL translation.

> **Status**: Simplified (2025-12-24). Most content archived for reference.

## Directory Structure

```
nsql/
├── README.md           # This file
├── nsql.R              # Main entry point
├── dictionary.yaml     # Core terminology and mappings
├── grammar.ebnf        # Formal grammar specification
├── examples/           # Usage examples
└── 99_archive/         # Archived specifications and rules
```

## Core Files

| File | Purpose |
|------|---------|
| `dictionary.yaml` | Terminology mappings between natural language and SQL |
| `grammar.ebnf` | Formal grammar definition in EBNF notation |
| `examples/` | Example NSQL patterns and translations |

## Usage

Basic concept (implementation incomplete):

```r
source("nsql.R")

# Conceptual translation
nsql_translate("show sales by region", target = "sql")
```

## Archive Contents

The `99_archive/` directory preserves the full historical specification:

- `01_spec/` - Language specification documents
- `02_rules/` - NSQL rules (NSQL_R01-R08, formerly R21-R23, R59-R63)
- `03_extensions/` - Language extensions (graph, temporal, etc.)
- `04_impl/` - Implementation components (parsers, translators)
- `06_workflows/` - AI workflow definitions
- `07_docs/` - Additional documentation
- `99_records/` - Change history

## Historical Context

NSQL was designed as a formal framework for translating natural language data queries to SQL. With advances in LLM capabilities, direct natural language processing has become more practical than maintaining a formal intermediate language.

The core concepts (dictionary, grammar) are preserved for reference, while the detailed specifications are archived.

---
*Last updated: 2025-12-24*
