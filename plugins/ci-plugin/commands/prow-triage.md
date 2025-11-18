---
description: Triage Prow CI test failures for a GitHub PR
argument-hint: <PR_URL_OR_REFERENCE>
allowed-tools: WebFetch, Bash, Grep, Read
---

## Name

ci:prow-triage - analyze and classify Prow CI test failures

## Synopsis

```
/ci:prow-triage <PR_URL_OR_REFERENCE>
```

## Description

The `ci:prow-triage` command analyzes all test failures for a GitHub Pull Request in Prow CI and classifies each failure as infrastructure-related, code-related, external dependency issue, or unknown.
This helps quickly determine whether failures are safe to retry or require code changes.

## Implementation

1. **Parse PR Reference**
   - Extract owner, repo, and PR number from argument
   - Support formats: full GitHub URL, `owner/repo#number`, or Prow URL
   - Use Grep/Read to extract PR info if needed

2. **Fetch PR Information**
   - Use WebFetch to get PR details from GitHub API: `https://api.github.com/repos/{owner}/{repo}/pulls/{number}`
   - Extract changed files and diff information
   - Get list of check runs and their status

3. **Analyze Failed Checks**
   - For each failed check, parse Prow CI URLs from check details
   - Access build logs via gcsweb-ci URLs
   - Download JUnit XML artifacts if available
   - Look for `finished.json` to understand failure context

4. **Pattern Matching and Classification**
   - Apply infrastructure patterns (network timeouts, OOMKilled, ImagePullBackOff, 500 errors)
   - Apply code patterns (AssertionError, panic, compilation failures)
   - Calculate confidence scores based on pattern matches

5. **Correlation Analysis**
   - Check if test files or related source files were modified in PR
   - Adjust confidence scores based on correlation
   - High correlation with changes → likely code issue
   - No correlation → likely infrastructure issue

6. **Phase Analysis**
   - Identify failure phase (setup/test/teardown)
   - Setup/teardown failures → boost infrastructure confidence
   - Test execution → requires deeper analysis

7. **Generate Triage Report**
   - Summarize total failures by category with percentages
   - List each failure with classification, confidence, evidence, and recommendations
   - Provide overall assessment (SAFE TO RETRY / NEEDS ATTENTION / MIXED RESULTS)
   - Include links to logs and artifacts

8. **Automatic Actions**
   - If all failures are infrastructure (>80% confidence) and no code issues: post `/retest` comment using `gh pr comment`
   - If code issues found (>70% confidence): recommend `/ci:prow-deep-dive` for detailed analysis
   - If mixed/unclear: recommend manual review

## Return Value

- **Format**: Structured triage report with failure classifications, confidence scores, and recommendations
- **Output**: Displayed to user with clear visual formatting using Unicode box drawing characters
- **Action**: May automatically post `/retest` comment if all failures are infrastructure-related

## Examples

1. **Triage PR using GitHub URL**:
   ```
   /ci:prow-triage https://github.com/openshift/origin/pull/30159
   ```
   Analyzes all test failures for PR #30159 in openshift/origin.

2. **Triage PR using short reference**:
   ```
   /ci:prow-triage openshift/origin#30159
   ```
   Same result using shorthand notation.

3. **Triage using Prow URL**:
   ```
   /ci:prow-triage https://prow.ci.openshift.org/view/gs/test-platform-results/pr-logs/pull/30159/...
   ```
   Extracts PR info from Prow URL and performs triage.

## Arguments

- `<PR_URL_OR_REFERENCE>`: GitHub PR URL, `owner/repo#number` format, or Prow CI URL pointing to a PR build

## Prerequisites

- `gh` CLI tool must be installed and authenticated for posting retest comments
  ```bash
  gh auth login
  ```

## Notes

- Infrastructure patterns include: connection timeouts, 500 errors, OOMKilled, ImagePullBackOff, resource constraints
- Code patterns include: AssertionError, panic, nil pointer, compilation failures
- Confidence scores range from 0-100% based on pattern strength, correlation, and phase
- Setup phase failures are almost always infrastructure (95%+ confidence)
- Known infrastructure issues (e.g., Ansible Galaxy 500 errors) have pre-set high confidence
- The command will NOT auto-retry if any code issues are detected above 70% confidence
- Links to build logs use gcsweb-ci URLs: `https://gcsweb-ci.apps.ci.l2s4.p1.openshiftapps.com/gcs/...`
