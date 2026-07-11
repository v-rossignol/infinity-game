# [AGENTS.md](http://AGENTS.md)

## Purpose

This repository is an **Organized Knowledge Framework (OKF)**.

Your role is to maintain the OKF as a coherent, searchable, and navigable knowledge system.

The repository structure is part of the knowledge and shall be preserved.

---

## Repository Model

The repository is an OKF tree.

Each directory represents a knowledge domain.

Every directory **MUST** contain:

- `index.md` — navigation and overview
- `log.md` — history of changes within the directory

The collection of all `index.md` files defines the canonical navigation graph of the repository.

No document shall become unreachable from the repository root.

---



## Core Responsibilities

The agent shall:

- classify new knowledge;
- update existing knowledge;
- avoid duplicate content;
- maintain `index.md` and `log.md`;
- maintain cross-references;
- preserve repository consistency;
- improve discoverability.

---



## Workflow



### Before creating a document

- Search for existing information.
- Prefer updating an existing document.
- Create a new document only when necessary.



### After creating, modifying, moving, renaming, archiving, or deleting a document

- Update the corresponding `index.md`.
- Record the change in the corresponding `log.md`.
- Update parent indexes if necessary.
- Verify internal links.
- Verify repository navigation.

---



## Rules

The agent **MUST**

- maintain an `index.md` in every directory;
- maintain a `log.md` in every directory;
- keep every document reachable through the OKF tree;
- preserve internal links;
- preserve metadata;
- avoid duplicate knowledge;
- explain significant structural changes;
- ask for clarification when document placement is ambiguous.
- apply metadata according to `reference/metadata.md`;

The agent **MUST NOT**

- create orphan documents;
- duplicate existing knowledge;
- silently change document meaning;
- invent factual content or metadata;
- delete knowledge without explicit instruction;
- leave a directory without an `index.md` or `log.md`.

---



## Quality Checklist

Before completing any task, verify that:

- every affected directory contains both `index.md` and `log.md`;
- every affected `index.md` has been updated;
- every affected `log.md` records the change;
- internal links remain valid;
- no duplicate knowledge was introduced;
- every document remains reachable from the repository root.

---



## Reference Documentation

Repository conventions are defined in the `reference/` directory.

This includes, for example:

- `reference/okf-spec.md`
- `reference/indexes.md`
- `reference/logs.md`
- `reference/metadata.md`
- `reference/naming.md`

If a conflict exists between this file and the reference documentation, this file defines the agent's required behavior.

---



## Objective

Continuously improve the OKF so that it remains:

- coherent;
- searchable;
- maintainable;
- navigable;
- internally consistent;
- usable by both humans and AI agents.

