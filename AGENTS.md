---
Version: 1.0
---

## Governing Rule

- Do not surface a consideration merely because it is relevant. Surface it only if it can change the decision, implementation, or risk.
- Define a clear objective for every plan.
- For tasks that require human involvement, help complete one task at a time. State the task's objective and main point in the first sentence, followed by supporting explanations.
- When considering recommendations, choose the simplest option with the smallest impact.

## Project Structure

- `README.md` — is the main documentation file of a GitHub repository. It explains what the project is, what it does, how to install or use it, and how developers can contribute.
- `docs/` — Diátaxis documentation, discoverable from `docs/index.md`. Documentation follow swebok.
- `.ai/` — AI assistant workspace for temporary files related to AI activities and processes. Safe to delete.
- `tmp/` — Temporary location related to software development activities. Safe to delete.
- `prompts/` — Instruction files or work plans.

Files under `prompts/` contain reusable instructions grouped by feature:
- Use folders named `<feature-number>`, such as `001` and `002`.
- Place general instructions or work plans in the `general` folder.
- Name instruction files `<number>-<task>.md`, such as `001-create-login-screen.md` and `002-create-login-api.md`.

## Git Flow and Release Policy

- The repository uses `main` as the production branch and `development` as the integration branch.
- Both `main` and `development` are protected branches and **MUST** be updated through approved Pull Requests with successful checks.
- Direct pushes, force pushes, and history rewriting on protected branches are prohibited except for documented emergencies.
- Features (`feature/*`) and non-production fixes (`bugfix/*`) **MUST** branch from the latest fetched `development` state.
- Feature and bugfix branches **MUST** merge back into `development` using squash merge.
- Hotfix branches (`hotfix/*`) **MUST** branch from the latest fetched `main` state.
- Production Pull Requests targeting `main` **MUST** originate from `hotfix/*` branches and **MUST** be reviewed and approved with successful checks.
- Production Pull Requests **MUST** use a merge commit, followed by an annotated version tag and the approved deployment procedure.
- After a hotfix is merged into `main`, `main` **MUST** be synchronized back into `development` through an approved Pull Request using a merge commit that preserves Git ancestry.
- The production workflow is `main` → `hotfix/*` → `main` → `development`.
- Branch names **SHOULD** use lowercase kebab-case.
- Commits **SHOULD** follow Conventional Commits.
- Secrets **MUST NOT** be committed.
- Git worktrees and AI coding agents **MUST** follow the same branch rules and operate only on assigned working branches.
- AI coding agents **MUST NOT** merge into protected branches without explicit authorization.

