---
description: Fetch and explain a Jira issue using first principles
argument-hint: <JIRA_URL>
allowed-tools: WebFetch, Bash, Grep, Read
---

## Name

learning:jira-explain - fetch and explain a Jira issue

## Synopsis

```
/learning:jira-explain <JIRA_URL>
```

## Description

The `learning:jira-explain` command fetches a Jira issue from its URL and explains it using first principles.
It breaks down the issue into understandable components following the WHAT -> WHY -> HOW structure.

## Implementation

1. **Parse Jira URL**
   - Extract Jira instance hostname and issue key (e.g., OCPBUGS-12345)
   - Validate URL format
   - Support common Jira URL formats

2. **Fetch Jira Issue**
   - Use WebFetch to retrieve the Jira issue page
   - Extract key information:
     - Issue type (Bug, Story, Task, Epic, Feature)
     - Summary/title
     - Description
     - Status and priority
     - Reporter and assignee
     - Components and labels
     - Comments (if relevant)
     - Linked issues and relationships
     - Fix versions and affected versions

3. **Analyze Issue Content**
   - Identify the core problem or requirement
   - Extract technical details and context
   - Note any error messages, logs, or stack traces
   - Identify dependencies and related work

4. **Structure Explanation**
   - **WHAT**: Summarize what the issue is about at a high level
     - Issue type and current status
     - One-sentence problem/requirement statement
     - Scope and affected components

   - **WHY**: Explain the underlying problem and context
     - Why this issue exists or was created
     - Impact on users, systems, or project
     - Business or technical justification
     - Root cause if identified

   - **HOW**: Detail the technical specifics
     - Technical details and implementation notes
     - Steps to reproduce (for bugs)
     - Acceptance criteria (for features/stories)
     - Proposed solutions or approaches
     - Related code areas or components

5. **Present Additional Context**
   - Link relationships (blocks, is blocked by, relates to)
   - Historical context from comments if relevant
   - Priority and urgency indicators
   - Team or component ownership

## Return Value

- **Format**: Inline structured explanation in conversation
- **Structure**: WHAT (overview) -> WHY (context/problem) -> HOW (technical details)
- **Output**: Displayed to user with clear section headers and bullet points

## Examples

1. **Explain Red Hat Jira bug**:
   ```
   /learning:jira-explain https://issues.redhat.com/browse/OCPBUGS-12345
   ```
   Fetches and explains the bug report in structured format.

2. **Explain public Jira issue**:
   ```
   /learning:jira-explain https://jira.atlassian.com/browse/JRASERVER-67890
   ```
   Fetches and explains the Atlassian Jira issue.

3. **Explain story from custom Jira**:
   ```
   /learning:jira-explain https://company.atlassian.net/browse/PROJ-123
   ```
   Works with any Jira instance URL.

## Arguments

- `<JIRA_URL>`: Full URL to a Jira issue (e.g., `https://issues.redhat.com/browse/OCPBUGS-12345`)

## Prerequisites

- Network access to the Jira instance
- Jira issue must be publicly accessible or authentication must be available
- If Jira requires authentication, it may need to be accessed through browser first

## Notes

- Works best with publicly accessible Jira issues
- For private Jira instances requiring authentication, WebFetch may have limitations
- Focuses on technical understanding rather than project management details
- Extracts information from HTML if API access is not available
- Long descriptions or many comments are summarized to focus on key information
- If issue has attachments (screenshots, logs), they are noted but not fetched
- Linked issues are mentioned but not fully explained (use separate command for those)
- For epic-level issues, focuses on overall goal rather than drilling into all child issues
