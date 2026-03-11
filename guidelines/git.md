# Git Workflow Guidelines

## Commit Message Types

Following Conventional Commits:

| Type | Description |
|------|-------------|
| **feat** | New feature for the user (not just code refactoring) |
| **fix** | Bug fix for the user (not fix to build script) |
| **docs** | Documentation changes only |
| **style** | Code style changes (formatting, semicolons, etc.) - no logic change |
| **refactor** | Code change that neither fixes a bug nor adds a feature |
| **perf** | Code change that improves performance |
| **test** | Adding or correcting tests |
| **chore** | Maintenance tasks, dependencies, tooling changes |
| **build** | Changes to build system or dependencies |
| **ci** | Changes to CI configuration/scripts |
| **revert** | Reverts a previous commit |

### Examples

```
feat(auth): add OAuth2 login support
fix(api): handle null response in user endpoint
docs(readme): update installation instructions
style(css): remove unused margin properties
refactor(utils): extract validation logic to helper
perf(db): add index on user_email column
test(auth): add unit tests for login function
chore(deps): upgrade express to v5
build(docker): update node version to 20
ci(github): add cache for npm install
```

## Commit Message Format

- Format: `type(scope): description`
- Keep subject line under 50 characters
- Use body for detail if needed
- Reference issues when applicable (e.g., `fix(auth): resolve login issue (#123)`)

## Branch Naming Conventions

- Feature: `feature/<feature-name>`
- Bug fix: `fix/<issue-name>`
- Hotfix: `hotfix/<issue-name>`

Examples: `feature/user-login`, `fix/api-timeout`, `hotfix/security-patch`

## Branching Strategy

- Branch from `main` for all new work
- Create a feature branch for each task
- Work locally and push when ready for review

## Pull Request Workflow

1. Push your feature branch to remote
2. Open a PR against `main`
3. Fill out PR description with context and testing steps
4. Address feedback and make changes if needed
5. Leader merges after approval

### PR Requirements

- All comments must be resolved before merge
- Squashed commits on merge
- Leader merges PRs only

## Code Review Guidelines

- Review turnaround: within 24 hours
- Check for correctness, readability, and testing
- Be constructive and specific in feedback
- Owner addresses all comments before merge

## Cross-Team Coordination

- When frontend depends on backend changes:
  - Backend team merges their changes first
  - Frontend team updates dependency and references issue in PR
- Use issues and PR comments to communicate dependencies

## Hotfix Procedure

1. Create branch from `main`: `hotfix/<issue-name>`
2. Make fix and commit with appropriate message
3. Open PR with high priority
4. Get 1 approval
5. Leader merges immediately
