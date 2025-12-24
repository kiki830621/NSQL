# Natural SQL Language (NSQL)

This directory contains the complete specification and implementation components of the Natural SQL Language (NSQL). NSQL serves as the standard language for data transformation operations in the precision marketing system.

> **Note**: This is an independent subrepo per MP122 Quad-Track Architecture.

## Directory Structure

```
nsql/
├── README.md                 # This file
├── nsql.R                    # Main entry point
├── .gitrepo                  # Subrepo configuration
│
├── 01_spec/                  # Language Specification
│   ├── grammar.ebnf          # Formal grammar (EBNF)
│   ├── dictionary.yaml       # Terms, functions, translations
│   ├── default_rules.md      # Default interpretation rules
│   ├── reference_resolution_rule.md
│   ├── natural_language_rule.md
│   ├── language_usage_principle.md
│   ├── reference_notation_rule.md
│   ├── nsql_set_theory_pragmatics.md
│   ├── modal_pragmatics_principle.md
│   └── multi_representation_principle.md
│
├── 02_rules/                 # NSQL Rules (NSQL_R01-R08)
│   ├── NSQL_R01_dictionary.md           # (was R21)
│   ├── NSQL_R02_interactive_update.md   # (was R22)
│   ├── NSQL_R03_mathematical_precision.md # (was R23)
│   ├── NSQL_R04_component_effect.md     # (was R59)
│   ├── NSQL_R05_ui_effect.md            # (was R60)
│   ├── NSQL_R06_extensionality.md       # (was R61)
│   ├── NSQL_R07_similarity.md           # (was R62)
│   ├── NSQL_R08_index_variability.md    # (was R63)
│   └── RSQL_P01_explicit_evaluation.md  # RSQL principle
│
├── 03_extensions/            # Language Extensions
│   ├── README.md
│   ├── graph_representation/ # Graph theory extensions
│   ├── documentation_syntax/ # LaTeX/Markdown documentation
│   ├── specialized/          # Domain-specific extensions
│   ├── temporal_logic/       # Time-based operations
│   └── IMPLEMENT/           # Implementation patterns
│
├── 04_impl/                  # Implementation Components
│   ├── parsers/              # NSQL parsers
│   ├── translators/          # SQL/dplyr translators
│   ├── validators/           # Validation tools
│   ├── sc_nsql_translate.R
│   ├── sc_nsql_parse.R
│   ├── sc_nsql_execute.R
│   ├── sc_nsql_validate.R
│   └── sc_nsql_dictionary.R
│
├── 05_examples/              # Usage Examples
│   ├── arrow_pattern.nsql
│   ├── transform_pattern.nsql
│   ├── natural_language_pattern.nsql
│   └── ...
│
├── 06_workflows/             # AI Workflows
│   ├── WF003_nsql_query_translation.md
│   └── WF004_nsql_extension_development.md
│
├── 07_docs/                  # Documentation
│   ├── integrated_guide.md   # Complete framework guide
│   └── question_sets.yaml    # Disambiguation questions
│
└── 99_records/               # Change History
    └── dot_notation_addition.md
```

## Terminology Conventions

### File Name vs. Object Name

Always distinguish between these two concepts:

- **File Name**: The name of a file containing code definitions (e.g., `fn_transform_data.R`)
- **Object Name**: The name of the actual code element defined within that file (e.g., `transform_data`)

### Implementation Pattern

For R functions:
- Function file names MUST use the `fn_` prefix (e.g., `fn_transform_data.R`)
- Function object names defined inside those files MUST NOT use the `fn_` prefix (e.g., `transform_data`)

## Usage

NSQL can be used either directly through the defined parsers and translators or through the interactive disambiguation process defined in NSQL_R02.

For translation:
```r
nsql_translate("show sales by region", target="sql")
```

For validation:
```r
nsql_validate("transform Sales to SummaryReport as sum(revenue) as total_revenue grouped by region")
```

## Rule Numbering Migration

As of 2025-12-24, NSQL uses its own independent numbering system:

| New ID | Previous ID | Description |
|--------|-------------|-------------|
| NSQL_R01 | R21 | NSQL Dictionary Rule |
| NSQL_R02 | R22 | Interactive Update Rule |
| NSQL_R03 | R23 | Mathematical Precision |
| NSQL_R04 | R59 | Component Effect Propagation |
| NSQL_R05 | R60 | UI Effect Propagation |
| NSQL_R06 | R61 | Extensionality Principle |
| NSQL_R07 | R62 | Similarity Principle |
| NSQL_R08 | R63 | Index Variability Theory |

## 🔄 AI Workflows

### WF003: NSQL Query Translation
- **Purpose**: Translate natural language queries to SQL using NSQL framework
- **Use case**: When users need to convert natural language descriptions to executable SQL
- **Key features**: Pattern recognition, validation, testing

### WF004: NSQL Extension Development
- **Purpose**: Develop and integrate new NSQL language extensions
- **Use case**: When adding domain-specific functionality to NSQL
- **Key features**: Syntax definition, function implementation, integration testing

## Related Meta-Principles

See `00_principles/docs/en/part1_principles/CH00_fundamental_principles/06_languages/` for:
- **MP083**: Natural SQL Language (core concept)
- **MP087**: NSQL Detailed Specification
- **MP088**: Graph Theory in NSQL
- **MP089**: NSQL Set Theory Foundations
- **MP090**: Radical Translation in NSQL

## Implementation Status

This is the active, authoritative implementation of the NSQL specification. The MP083 document in 00_principles provides a high-level overview but refers to this directory for complete details.

---
*Last updated: 2025-12-24*
