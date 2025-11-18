---
description: Generate self-contained HTML with embedded modern styling and open in browser
argument-hint: [optional description]
allowed-tools: Write, Bash
---

## Name

output:website

## Synopsis

```
/output:website [description]
```

## Description

The `output:website` command generates a complete, self-contained HTML
document with embedded modern CSS styling from the current conversation.
The file is automatically opened in your default browser.

This is useful for preserving important discussions or learning sessions
in a visually appealing, shareable format.

## Implementation

1. **Capture conversation context**
   - Review the entire conversation history
   - Extract key points, code examples, and explanations
   - Structure content logically with headers and sections

2. **Generate HTML with embedded styling**
   - Create complete HTML5 document structure
   - Embed modern CSS with:
     - Color palette: blues (#2563eb), grays, greens for success
     - Responsive design with max-width 900px
     - Line height 1.8 and font-size 14px for code blocks
     - Syntax highlighting with color-coded classes:
       - `.comment`: Gray comments
       - `.keyword`: Blue keywords
       - `.function`: Purple functions
       - `.string`: Green strings
       - `.type`: Cyan type annotations
   - Styled sections for info/warning/error/success messages
   - Proper formatting with no minified code

3. **Save with timestamped filename**
   - Use Write tool to create file in current directory
   - Filename format: `cc_website_<description>_YYYYMMDD_HHMMSS.html`
   - If description not provided, derive from conversation topic
   - Ensure proper indentation and formatting

4. **Open in browser**
   - Use Bash tool with `xdg-open` to open the HTML file
   - File opens in default system browser

5. **Confirm to user**
   - Provide summary of content saved
   - Include file path for reference

## Return Value

- **Format**: Self-contained HTML5 file with embedded CSS
- **Location**: Current working directory
- **Filename**: `cc_website_<description>_YYYYMMDD_HHMMSS.html`
- **Action**: Automatically opened in default browser

## Examples

1. **Document current discussion**:
   ```
   /output:website discussion
   ```

   Generates `cc_website_discussion_20251114_153022.html` with the
   conversation formatted with modern styling.

2. **Save learning session**:
   ```
   /output:website kubernetes-concepts
   ```

   Creates timestamped HTML documenting the Kubernetes learning session.

3. **No description provided**:
   ```
   /output:website
   ```

   Derives description from conversation topic automatically.

## Arguments

- `[description]`: Optional brief description used in filename
  (e.g., "discussion", "learning-session"). If omitted, derived from
  conversation context.
