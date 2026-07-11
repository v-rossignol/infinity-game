# OKF Specification

## Purpose

An **Organized Knowledge Framework (OKF)** is a hierarchical repository that organizes knowledge into coherent domains.

Its objectives are to:

- provide a single source of truth;
- simplify knowledge discovery;
- preserve relationships between topics;
- support long-term maintenance by both humans and AI agents.

The directory structure is part of the knowledge model.

---

# Repository Model

An OKF is organized as a tree.

- Each directory represents a knowledge domain.
- Knowledge is organized hierarchically.
- Documents belong to one primary knowledge domain.

The repository shall remain fully navigable from its root.

---



# Directory Structure

Every directory **MUST** contain:

- `index.md`
- `log.md`

Additional files and subdirectories may be added as needed.

Example:

```text
knowledge/
│
├── index.md
├── log.md
│
├── topic-a.md
├── topic-b.md
│
├── subdomain-1/
│   ├── index.md
│   ├── log.md
│   └── ...
│
└── subdomain-2/
    ├── index.md
    ├── log.md
    └── ...
```

---



# Document Types

The OKF distinguishes several document types.

## Knowledge documents

Knowledge documents contain domain knowledge.

Examples:

- policies
- procedures
- architecture
- specifications
- meeting summaries
- tutorials

Knowledge documents require metadata.

---



## Index documents

`index.md` defines the navigation for a directory.

It provides:

- purpose of the knowledge domain;
- list of documents;
- list of subdirectories;
- short descriptions;
- links to related domains.

Every directory shall contain exactly one `index.md`.

---



## Log documents

`log.md` records the evolution of a knowledge domain.

Typical entries include:

- document creation;
- document updates;
- document moves;
- document renames;
- archival;
- structural changes.

The log is a concise history, not a version control system.

Every directory shall contain exactly one `log.md`.

---



## Governance documents

Governance documents define repository behavior.

Examples:

- `AGENTS.md`

They do not contain domain knowledge.

---



## Reference documents

Reference documents define repository conventions and specifications.

Examples:

- metadata conventions;
- naming conventions;
- style guides;
- OKF specification.

---



# Navigation

Navigation is provided through the hierarchy of `index.md` files.

Every document shall be reachable by following successive directory indexes from the repository root.

No knowledge document should become orphaned.

---



# Cross References

Knowledge documents may reference related knowledge located in other domains.

Cross references should be used instead of duplicating information whenever practical.

Each piece of knowledge should have one authoritative location.

---



# Metadata

Metadata applies only to knowledge documents.

Navigation, logging, governance, and reference documents do not require metadata unless explicitly specified.

Metadata conventions are defined in `reference/metadata.md`.

---



# Naming

Naming conventions are defined in `reference/naming.md`.

Repositories should use descriptive, stable names for both directories and documents.

---



# Maintenance

Whenever the repository changes:

- affected `index.md` files shall be updated;
- affected `log.md` files shall record the change;
- internal links shall remain valid;
- navigation shall remain consistent.

---



# Repository Quality

An OKF repository should always remain:

- coherent;
- searchable;
- navigable;
- maintainable;
- internally consistent;
- free of unnecessary duplication.

Knowledge should evolve by updating existing documents whenever practical rather than creating redundant ones.

---



# Authority

This document defines the structure and conventions of an Organized Knowledge Framework.

Operational behavior for AI agents is defined in `AGENTS.md`.