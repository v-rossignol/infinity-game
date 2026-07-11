# Metadata

## Scope

Metadata is required **only for knowledge documents**.

Navigation and governance documents do not require YAML front matter.

| Document | Metadata |
|----------|----------|
| Knowledge document | Required |
| index.md | Not required |
| log.md | Not required |
| AGENTS.md | Not required |
| Reference documentation | Optional |

---

## Required Fields

Knowledge documents SHOULD begin with YAML front matter.

Example:

```yaml
---
title: Password Policy
author: Jane Doe
created: 2026-07-06
updated: 2026-07-06
status: approved
tags:
  - security
---
```

Recommended fields:

- title
- author
- created
- updated
- status
- tags

---

## Status

Suggested values:

- draft
- review
- approved
- archived

Metadata should be updated whenever the knowledge changes.