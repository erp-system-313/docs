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

## Pull Request Review Checklist

### PR Author - Before Submitting

- [ ] PR title follows conventional commit format (`type(scope): description`)
- [ ] PR description explains **what** and **why** (not how)
- [ ] All tests pass locally
- [ ] No console.log/printStackTrace left in code
- [ ] No commented-out code
- [ ] No debug/development-only code
- [ ] Sensitive data (credentials, tokens) not included
- [ ] Related issue linked (`Fixes #123` or `Closes #123`)
- [ ] Self-reviewed the diff before requesting review

### Reviewer - General Code Review

#### Functionality
- [ ] Code does what the PR claims
- [ ] Edge cases handled appropriately
- [ ] Error handling is in place
- [ ] No obvious bugs or logic errors

#### Code Quality
- [ ] Follows project naming conventions
- [ ] Code is readable and well-organized
- [ ] Functions are small and focused
- [ ] No code duplication without justification
- [ ] Magic numbers/names extracted to constants

#### Security
- [ ] No sensitive data hardcoded
- [ ] Input validation present
- [ ] SQL injection prevented (parameterized queries)
- [ ] XSS prevention (proper escaping)

#### Performance
- [ ] No N+1 queries
- [ ] Large data sets handled efficiently
- [ ] Proper pagination where applicable
- [ ] No memory leaks

#### Testing
- [ ] Unit tests added/updated for new logic
- [ ] Tests are meaningful and not just coverage
- [ ] Happy path and error cases covered

### Reviewer - Frontend Specific

- [ ] Components properly typed (TypeScript)
- [ ] No `any` types without justification
- [ ] Responsive design works on required breakpoints
- [ ] Loading/error states implemented
- [ ] Accessibility: proper ARIA labels, keyboard navigation

### Reviewer - Backend Specific

- [ ] API endpoints follow REST conventions
- [ ] Proper HTTP status codes used
- [ ] Request validation in place
- [ ] DTOs used instead of exposing entities
- [ ] Transactions used appropriately
- [ ] Logging is appropriate (info/warn/error)
- [ ] Database migrations are reversible

### Reviewer - Approval

- [ ] All comments resolved
- [ ] PR can be merged without conflicts
- [ ] No breaking changes without discussion
- [ ] Documentation updated if needed

### After Approval

- [ ] Merge strategy: squash and merge
- [ ] Delete branch after merge
- [ ] Verify CI/CD pipeline passes

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
