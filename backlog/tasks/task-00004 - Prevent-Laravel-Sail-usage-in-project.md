---
id: task-00004
title: Prevent Laravel Sail usage in project
status: Done
assignee:
  - codex
created_date: '2025-11-05 09:24'
updated_date: '2025-11-05 09:40'
labels: []
dependencies: []
---

## Description

<!-- SECTION:DESCRIPTION:BEGIN -->
Ensure the repository enforces plain docker compose workflows by removing Laravel Sail and adding safeguards/documentation against its use.
<!-- SECTION:DESCRIPTION:END -->

## Acceptance Criteria
<!-- AC:BEGIN -->
- [x] #1 Laravel Sail dependency and artifacts are removed from the project.
- [x] #2 Documentation explicitly states that Sail and other Docker wrappers are unsupported.
- [x] #3 Repository includes guardrails (e.g., Git hooks or scripts) preventing Sail from being reintroduced.
<!-- AC:END -->

## Implementation Plan

<!-- SECTION:PLAN:BEGIN -->
- Remove `laravel/sail` from composer dependencies and clean any associated vendor binaries.
- Update repository documentation to state docker compose is the only supported workflow and describe hook setup.
- Add Git hook guardrails to fail commits introducing Sail references, with instructions for enabling them.
<!-- SECTION:PLAN:END -->

## Implementation Notes

<!-- SECTION:NOTES:BEGIN -->
Removed laravel/sail from composer require-dev (and symfony/yaml), verified Laravel still runs, updated README to document docker-compose-only policy and hook setup, and added `.githooks/pre-commit` guard that blocks Sail artifacts or commands.
<!-- SECTION:NOTES:END -->
