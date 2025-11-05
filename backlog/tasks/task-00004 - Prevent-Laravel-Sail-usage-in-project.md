---
id: task-00004
title: Prevent Laravel Sail usage in project
status: In Progress
assignee:
  - codex
created_date: '2025-11-05 09:24'
updated_date: '2025-11-05 09:24'
labels: []
dependencies: []
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Ensure the repository enforces plain docker compose workflows by removing Laravel Sail and adding safeguards/documentation against its use.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [ ] #1 Laravel Sail dependency and artifacts are removed from the project.
- [ ] #2 Documentation explicitly states that Sail and other Docker wrappers are unsupported.
- [ ] #3 Repository includes guardrails (e.g., Git hooks or scripts) preventing Sail from being reintroduced.
<!-- AC:END -->
