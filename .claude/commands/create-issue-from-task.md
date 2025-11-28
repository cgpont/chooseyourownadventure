# create-issue-from-task

Create a GitHub issue from a specific task in the PRD @docs/PRD.md, designed to be concise and completable in a couple hours.

## Usage

```
/create-issue-from-task <task-number>
```

## Arguments

- `task-number` - The task number from the PRD to create as a separate GitHub issue

## Description

This command creates a focused GitHub issue from one of the tasks defined in the PRD by:

1. Extracting the specific task details from the PRD
2. Creating a concise, actionable issue that can be completed in 1-2 hours
3. Including proper acceptance criteria and implementation details
4. Adding appropriate labels (task, phase-specific labels)
5. Following the project's containerized development approach

The created issue will include:
- Clear task description and requirements
- Container-specific commands where applicable
- Acceptance criteria checklist
- Time estimate (≤2 hours)
- Appropriate labels for categorization

## Example

```
/create-issue 1
```

This would create a GitHub issue for "Task 1" with all the specific requirements, container commands, and acceptance criteria needed to complete the task in ~2 hours.

## Notes

- Each task is designed to be atomic and completable independently
- All tasks follow the containerized development approach defined in CLAUDE.md
- Issues include specific container commands where applicable