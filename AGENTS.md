# Agent Rules for malt Project

## Workflow: speckit (Spec-Driven Development)

Follow the speckit workflow **to the letter**:
1. **specify** - Create specification
2. **plan** - Create implementation plan  
3. **tasks** - Generate task breakdown
4. **implement** - Execute implementation

Review gates required at each stage.

## Git Branch Strategy

**ALWAYS create a feature branch before making any changes:**
- Branch naming: `feature/<short-description>` or `fix/<short-description>`
- Never commit directly to `main`
- Branch from latest `main`

## Task & Issue Management

**ALWAYS create tasks and GitHub issues before implementing:**
- Create GitHub issue for each feature/fix
- Break down into tasks in issue or project board
- Reference issue numbers in commits/branches

## Pull Request Process

When implementation is complete:
1. Create PR from feature branch to `main`
2. Link all related GitHub issues in PR description (`Closes #X`, `Fixes #Y`)
3. Ensure all CI checks pass
4. **Squash merge** into `main`
5. Delete feature branch after merge

## Commit Messages

Follow conventional commits:
- `feat: <description>` for features
- `fix: <description>` for bug fixes
- `chore: <description>` for maintenance
- Include issue reference: `feat: add user auth (Closes #12)`

## Speckit Commands

Use these speckit commands via the workflow:
- `speckit.specify` - Generate specification
- `speckit.plan` - Create implementation plan
- `speckit.tasks` - Generate task breakdown
- `speckit.implement` - Execute implementation

Integration: auto (uses project's initialized integration)