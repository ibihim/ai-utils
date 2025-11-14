---
description: Create a Liz Rice-style walkthrough that builds a concept from first principles with working code examples
argument-hint: build from scratch | explain like Liz Rice
allowed-tools: Write, Read, Glob, Grep, Bash, TodoWrite
---

## Name

learning:build-from-scratch

## Synopsis

```
/learning:build-from-scratch <topic>
```

## Description

The `learning:build-from-scratch` command creates a comprehensive "Build Your Own X" walkthrough that follows the Liz Rice presentation style.
It builds concepts incrementally from first principles with working code at every step.

## Implementation

1. **Create markdown file structure**
   - Use Write tool to create a new markdown file
   - File location: `.work/learning/build-from-scratch-{topic}.md`

2. **Follow Liz Rice presentation style**
   - Start Simple: Begin with the absolute minimum working example
   - Build Incrementally: Each step adds exactly one new concept
   - Working Code: Every step must have runnable code examples
   - Show the Magic: Demonstrate how complex features emerge from simple building blocks
   - Connect to Real World: Draw parallels to production systems

3. **Structure the walkthrough**
   - Step 1: Simplest possible implementation (core concept only)
   - Step 2: Add one key feature with working examples
   - Step 3: Add another feature, building on previous steps
   - Step 4+: Continue incrementally until full system is built
   - Final Step: Complete working example that demonstrates all concepts

4. **Code quality requirements**
   - All code must be runnable at each step
   - Include clear comments explaining the "why" not just the "what"
   - Show before/after comparisons when adding features
   - Provide test/demo code to verify each step works

5. **Educational focus**
   - Explain the fundamental principles first
   - Show how simple concepts combine to create complexity
   - Make connections to real-world implementations
   - Include "try it yourself" exercises
   - End with "what's next" suggestions for further exploration

## Return Value

- **Format**: Markdown file at `.work/learning/build-from-scratch-{topic}.md`
- **Content**: Complete walkthrough with working code examples at each step

## Examples

1. **Build a container runtime**:
   ```
   /learning:build-from-scratch container runtime
   ```

   Creates walkthrough showing how to build a minimal container runtime from scratch.

2. **Build a key-value database**:
   ```
   /learning:build-from-scratch key-value database
   ```

   Creates incremental guide building a simple key-value store.

## Arguments

- `$ARGUMENTS`: Topic or technology to build from scratch (e.g., "container runtime", "web server", "DNS resolver")
