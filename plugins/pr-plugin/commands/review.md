---
description: Review PR changes for critical issues
argument-hint: [PR_URL_OR_DIFF]
allowed-tools: Bash, Read, Grep, WebFetch
---

## Name

pr:review - analyze PR changes for critical issues

## Synopsis

```
/pr:review [PR_URL_OR_DIFF]
```

## Description

The `pr:review` command analyzes changes in a pull request and focuses on identifying
critical issues related to bugs, performance, security, and correctness.

The review is intentionally concise.
Only critical issues that must be addressed before merging are highlighted.
Style or minor suggestions are skipped unless they impact performance, security, or
correctness.

## Implementation

1. **Gather Changes**
   - If PR URL provided: use `gh pr diff <URL>` to fetch the diff
   - If no argument: use `git diff` for staged changes or `git diff HEAD~1` for
     last commit
   - Parse the diff to understand what files and code sections changed

2. **Analyze for Critical Issues**
   - **Bugs**: Logic errors, null pointer risks, unhandled edge cases, race conditions
   - **Performance**: O(n^2) loops, memory leaks, unnecessary allocations,
     blocking operations
   - **Security**: Input validation, injection vulnerabilities, hardcoded secrets,
     insecure defaults
   - **Correctness**: Off-by-one errors, incorrect type handling, missing error checks

3. **Generate Review**
   - If critical issues found: list them in short bullet points
   - If no critical issues found: provide simple approval
   - Sign off with checkbox emoji indicating result

## Return Value

- **Format**: Concise review summary
- **Output**: Displayed directly to user
- **Sign-off**:
  - Approved: checkbox emoji followed by "approved"
  - Issues found: checkbox emoji followed by "issues found"

## Examples

1. **Review a GitHub PR**:
   ```
   /pr:review https://github.com/owner/repo/pull/123
   ```
   Fetches and analyzes the PR diff.

2. **Review current staged changes**:
   ```
   /pr:review
   ```
   Analyzes `git diff` output for staged changes.

3. **Review last commit**:
   ```
   /pr:review HEAD~1
   ```
   Analyzes changes from the previous commit.

## Arguments

- `[PR_URL_OR_DIFF]`: Optional.
  GitHub PR URL, git ref, or omit to review staged changes.

## Prerequisites

- `gh` CLI for GitHub PR URLs:
  ```bash
  gh auth login
  ```

## Notes

- This is a focused, critical-issues-only review.
- Minor style suggestions are intentionally omitted.
- The review prioritizes issues that could cause production incidents.
