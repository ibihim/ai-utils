---
description: Save conversation as comprehensive markdown with YAML frontmatter
argument-hint: [optional title]
allowed-tools: Write
---

## Name

output:markdown

## Synopsis

```
/output:markdown [title]
```

## Description

The `output:markdown` command saves the current conversation as a
comprehensive markdown file with YAML frontmatter, proper formatting,
and sentence-per-line structure for easier editing.

This is ideal for creating permanent documentation of important
discussions, learning sessions, or technical explanations.

## Implementation

1. **Capture conversation context**
   - Review entire conversation history
   - Identify key topics, code examples, and concepts
   - Extract relevant sources, tags, and aliases

2. **Generate YAML frontmatter**
   - Create frontmatter with:
     - `aliases`: Alternative names or references for the content
     - `sources`: URLs or references cited in the discussion
     - `tags`: Topic keywords for categorization
   - Use proper YAML list syntax

3. **Format content with comprehensive markdown**
   - Use sentence-per-line formatting (easier for editing)
   - Headers (##, ###, ####) for hierarchy
   - `inline code` for commands, files, variables
   - Code blocks with language identifiers
   - **Bold** for important concepts
   - _Italics_ for technical terms
   - > Blockquotes for notes and warnings
   - Tables for comparisons
   - Task lists for actionable items
   - Horizontal rules (`---`) for major sections

4. **Apply formatting constraints**
   - Target 80-100 column width (sentence-per-line helps)
   - UTF-8 encoding
   - Newline at end of file (as per CLAUDE.md requirements)
   - Clear hierarchy and structure

5. **Save with timestamped filename**
   - Use Write tool to create file in current directory
   - Filename format: `YYYYMMDDHHMM-$MEANINGFUL_TITLE.md`
   - If title not provided, derive from conversation topic
   - Replace spaces with hyphens, lowercase

6. **Confirm to user**
   - Confirm file creation with full path
   - Provide brief summary of content saved

## Return Value

- **Format**: Markdown file with YAML frontmatter
- **Location**: Current working directory
- **Filename**: `YYYYMMDDHHMM-meaningful-title.md`
- **Encoding**: UTF-8 with newline at end

## Examples

1. **Document current discussion**:
   ```
   /output:markdown go-error-handling
   ```

   Creates `202511141530-go-error-handling.md` with the conversation
   formatted as comprehensive markdown.

2. **Save learning session**:
   ```
   /output:markdown kubernetes-networking
   ```

   Generates timestamped markdown file documenting the Kubernetes
   networking discussion.

3. **No title provided**:
   ```
   /output:markdown
   ```

   Derives meaningful title from conversation topic automatically.

## Arguments

- `[title]`: Optional title used in filename (e.g., "go-patterns",
  "learning-session"). If omitted, derived from conversation context.
  Spaces converted to hyphens, lowercased.
