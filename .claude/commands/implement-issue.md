# implement-issue

Implement a GitHub issue using the Laravel backend engineer agent following the project workflow.

## Usage

```
/implement-issue <issue-number>
```

## Arguments

- `issue-number` - The GitHub issue number to implement

## Description

This command implements the specified GitHub issue by:

1. Using the php-laravel-backend-engineer agent to handle the implementation
2. Following the branch-based workflow defined in CLAUDE.md
3. Adhering to all project context instructions and quality standards
4. Creating appropriate branches, commits, and pull requests as needed

The agent will analyze the issue, create the necessary implementation following Laravel best practices, run tests, and prepare the changes for review according to the project's GitHub workflow.

## Example

```
/implement-issue 15
```

This would implement issue #15 using the specialized Laravel backend engineer agent while following all project guidelines and workflow requirements.