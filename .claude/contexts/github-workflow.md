# GitHub Workflow

## Branch Workflow Process
1. **Start from Issue**: All work begins with a GitHub issue assignment
2. **Create Feature Branch**: 
   - Branch from `main`
   - Use naming convention: `feature/issue-X-short-description`
   - Examples: `feature/issue-5-setup-laravel`, `fix/issue-10-auth-bug`
3. **Development**:
   - Make necessary changes in the feature branch
   - Create commits following **Conventional Commits** specification
   - Push commits to the feature branch
4. **Pull Request**:
   - Create PR from feature branch to `main`
   - Include detailed description of changes
   - Link to the original issue with "Closes #X" or "Fixes #X"
   - Request review
5. **Code Review & Merge**:
   - Review and approve changes
   - Merge PR to main
   - **Delete feature branch** after successful merge
6. **Branch Cleanup**: Ensure temporary branches are removed to keep repository clean

## Branch Naming Conventions
```
feature/issue-X-short-description    # New features
fix/issue-X-short-description        # Bug fixes  
docs/issue-X-short-description       # Documentation changes
refactor/issue-X-short-description   # Code refactoring
chore/issue-X-short-description      # Maintenance tasks
```

## Commit Standards
All commits must follow the **Conventional Commits** specification:

### Format
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Types
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation only changes
- `style`: Changes that do not affect the meaning of the code
- `refactor`: A code change that neither fixes a bug nor adds a feature
- `perf`: A code change that improves performance
- `test`: Adding missing tests or correcting existing tests
- `chore`: Changes to the build process or auxiliary tools

### Examples
- `feat: add internationalization support for global markets`
- `fix: resolve authentication token validation issue`
- `docs: update API documentation with new endpoints`
- `refactor: optimize database queries for product search`

## Communication Standards
- **Conciseness**: Comments in commits, PRs and issues must be concise, simple and short
- **Focus**: Focus only on important things, avoid unnecessary details
- **Clarity**: Use clear and direct language

## Pull Request Workflow
**IMPORTANT**: Before implementing any changes:
1. **Create Pull Request First**: Always create a PR with a detailed implementation plan before writing code
2. **Wait for Approval**: Do not implement changes until the PR receives approval from project maintainers
3. **Implementation**: Only after approval, implement the planned changes in the existing PR
4. **Review Process**: All changes go through code review before merging

This ensures proper project coordination and prevents unauthorized modifications.