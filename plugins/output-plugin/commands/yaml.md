---
description: Format conversation as structured YAML with hierarchical organization
argument-hint: [optional filename]
allowed-tools: Write
---

## Name

output:yaml

## Synopsis

```
/output:yaml [filename]
```

## Description

The `output:yaml` command structures the current conversation as valid
YAML with hierarchical organization, making it easy to parse and process
programmatically.

This is useful for configuration-style documentation or when you need
machine-readable output that can be processed by scripts.

## Implementation

1. **Capture conversation context**
   - Review entire conversation history
   - Extract key tasks, actions, changes, and outcomes
   - Identify files modified, commands run, and next steps

2. **Structure as hierarchical YAML**
   - Use standard structure:
     - `task`: Brief description of what was discussed/done
     - `status`: success|in-progress|failed
     - `details`: Action, target, changes with descriptive keys
     - `files`: List of files with path, action, description
     - `commands`: List of commands that were run
     - `notes`: Important considerations or caveats
     - `next_steps`: Recommended follow-up actions
   - Use 2-space indentation (YAML standard)
   - Include descriptive comments using `#`
   - Use absolute file paths where applicable
   - Maintain parseable YAML syntax

3. **Apply YAML formatting rules**
   - Consistent 2-space indentation
   - Appropriate data types (strings, lists, objects)
   - Logical nesting (not overly deep)
   - Descriptive keys that self-document
   - Comments for context where helpful

4. **Save to file**
   - Use Write tool to create file in current directory
   - Filename format: `conversation-YYYYMMDDHHMM.yaml` (or custom)
   - Ensure valid YAML syntax
   - Newline at end of file

5. **Confirm to user**
   - Confirm file creation with full path
   - Optionally show sample of YAML structure

## Return Value

- **Format**: Valid YAML file with hierarchical structure
- **Location**: Current working directory
- **Filename**: `conversation-YYYYMMDDHHMM.yaml` or custom name
- **Syntax**: Parseable YAML with 2-space indentation

## Examples

1. **Document current session**:
   ```
   /output:yaml
   ```

   Creates `conversation-202511141530.yaml` with structured YAML.

2. **Custom filename**:
   ```
   /output:yaml refactoring-summary
   ```

   Generates `refactoring-summary.yaml` with the conversation data.

3. **Example YAML structure**:
   ```yaml
   task: "Implement user authentication feature"
   status: "success"
   details:
     action: "Added JWT-based authentication"
     target: "/home/user/project/auth"
     changes: 3
   files:
     - path: "/home/user/project/auth/jwt.go"
       action: "created"
       description: "JWT token generation and validation"
     - path: "/home/user/project/auth/middleware.go"
       action: "created"
       description: "Authentication middleware for routes"
   commands:
     - "go build ./..."
     - "go test ./auth/..."
   notes:
     - "Remember to add JWT secret to environment variables"
     - "Update API documentation with auth requirements"
   next_steps:
     - "Add refresh token functionality"
     - "Implement role-based access control"
   ```

## Arguments

- `[filename]`: Optional filename (without extension) for the YAML file.
  If omitted, uses `conversation-YYYYMMDDHHMM.yaml`. Extension `.yaml`
  is added automatically.
