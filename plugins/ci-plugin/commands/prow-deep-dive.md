---
description: Deep root cause analysis of a specific Prow CI test failure
argument-hint: <FAILURE_REFERENCE>
allowed-tools: WebFetch, Bash, Grep, Read
---

## Name

ci:prow-deep-dive - perform comprehensive root cause analysis of a Prow test failure

## Synopsis

```
/ci:prow-deep-dive <FAILURE_REFERENCE>
```

## Description

The `ci:prow-deep-dive` command performs a comprehensive, detailed analysis of a specific test failure including complete log analysis, pattern matching with explanations, detailed correlation with PR code changes, step-by-step debugging recommendations, and historical context.
This is typically used after `/ci:prow-triage` identifies specific failures needing deeper investigation.

## Implementation

1. **Parse Input and Gather Context**
   - Extract job information from failure reference
   - If numeric reference (e.g., "1"), retrieve details from previous `/ci:prow-triage` session
   - Fetch complete PR context if not already available
   - Use WebFetch to get PR details from GitHub API

2. **Download All Artifacts**
   - Fetch build log (`build-log.txt`)
   - Download JUnit XML files (`artifacts/junit*.xml`)
   - Get pod information (`podinfo.json`)
   - Fetch finished metadata (`finished.json`)
   - Get started metadata (`started.json`)
   - Download CI operator logs (`ci-operator.log`)
   - Use WebFetch for gcsweb-ci artifact URLs

3. **Comprehensive Log Analysis**
   - Extract all error messages from logs
   - Identify the first failure point chronologically
   - Trace failure propagation through logs
   - Find related log entries before and after failure
   - Look for warnings that preceded errors
   - Build failure timeline with timestamps

4. **Pattern Deep Dive**
   - Match against ALL known patterns (infrastructure, code, external)
   - Explain why each pattern matched with evidence
   - Show confidence calculation breakdown
   - Identify competing hypotheses if multiple patterns match
   - Consider pattern context and surrounding log lines

5. **Code Correlation Analysis**
   - Load complete PR diff from GitHub
   - For each changed file:
     - Check if file is directly tested by failing test
     - Check if file is imported by failing test
     - Check if filename/path appears in error messages
   - Calculate detailed correlation score (0.0-1.0)
   - Explain correlation reasoning with specific evidence

6. **Historical Context** (if accessible)
   - Search for similar past failures in Prow history
   - Note if test is known to be flaky
   - Reference similar past failures if patterns match
   - Check OpenShift/Kubernetes known issues

7. **Environment Analysis**
   - Review pod scheduling details from podinfo
   - Check resource allocation (CPU/memory requests and limits)
   - Analyze timing information (duration, timeout settings)
   - Look for environmental factors (node status, namespace issues)

8. **Generate Comprehensive Report**
   - Create failure timeline with chronological events
   - Show critical error messages with log snippets
   - Display pattern matching analysis with confidence breakdown
   - Present code correlation with changed file details
   - Show environment and resource analysis
   - Provide root cause assessment with supporting evidence
   - Give specific debugging recommendations with code examples
   - Include all reference links (logs, artifacts, PR diff)

9. **Provide Actionable Recommendations**
   - For infrastructure failures: recommend retry with monitoring
   - For code failures: show specific files/lines needing changes with suggested fixes
   - For unclear cases: provide step-by-step investigation approach
   - Include commands for local reproduction and verification

## Return Value

- **Format**: Comprehensive analysis report with timeline, log analysis, pattern matching, correlation, environment details, root cause assessment, and debugging recommendations
- **Output**: Displayed to user with rich formatting including Unicode box drawing, emoji indicators, code blocks, and linked references
- **Action**: May offer to post `/retest` if infrastructure failure confirmed with high confidence

## Examples

1. **Deep dive using Prow URL**:
   ```
   /ci:prow-deep-dive https://prow.ci.openshift.org/view/gs/test-platform-results/pr-logs/pull/30159/pull-ci-openshift-origin-main-e2e-metal-ipi-ovn-ipv6/1990760520294076416
   ```
   Performs deep analysis of specific job failure.

2. **Deep dive using PR and job name**:
   ```
   /ci:prow-deep-dive openshift/origin#30159 e2e-metal-ipi-ovn-ipv6
   ```
   Analyzes specific job failure for PR #30159.

3. **Deep dive by failure number from triage**:
   ```
   /ci:prow-deep-dive 1
   ```
   After running `/ci:prow-triage`, analyzes failure #1 from triage results.

## Arguments

- `<FAILURE_REFERENCE>`: Full Prow URL to specific job build, `owner/repo#number jobname` format, or numeric reference to failure from previous `/ci:prow-triage` output

## Prerequisites

- `gh` CLI tool must be installed and authenticated for accessing GitHub PR details
  ```bash
  gh auth login
  ```

## Notes

- This command downloads and analyzes multiple artifact files (logs, JUnit XML, metadata)
- Log snippets are shown with surrounding context lines for better understanding
- Confidence scores show breakdown: base pattern + phase adjustment + correlation factor + historical data
- Code correlation scores: >0.7 (high/likely PR caused), 0.4-0.7 (medium/possible), <0.4 (low/unlikely)
- Timeline uses emoji indicators: ℹ️ (info), ⚠️ (warning), ❌ (error), 💥 (critical failure), 📋 (cleanup)
- For code failures, provides specific file:line references and suggested fixes with code examples
- Environment analysis includes actual resource usage if metrics available in pod info
- Alternative explanations provided when confidence is not very high
- All gcsweb-ci artifact URLs use: `https://gcsweb-ci.apps.ci.l2s4.p1.openshiftapps.com/gcs/...`
- Setup phase failures (pre-test) are almost always infrastructure issues
- Teardown phase failures are usually infrastructure and can often be ignored
- Test execution phase failures require careful correlation analysis
