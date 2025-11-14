---
description: Explains based on first principles with WHAT -> WHY -> HOW
argument-hint: explain based on first-principles | explain with executive summary
---

## Name

learning:explain

## Synopsis

```
/learning:explain <topic>
```

## Description

The `learning:explain` command provides explanations based on first principles, following a structured WHAT -> WHY -> HOW approach.
This format delivers high-level understanding before diving into details.

## Implementation

1. **Explain from first principles**
   - Start with foundational concepts
   - Build understanding incrementally
   - Avoid assuming prior knowledge

2. **Follow the WHAT -> WHY -> HOW structure**
   - **WHAT**: Provide high-level overview and executive summary
   - **WHY**: Explain what problem it solves and its purpose
   - **HOW**: Detail the important implementation specifics

3. **Presentation style**
   - Clear, concise language
   - Progressive disclosure of complexity
   - Connect concepts to familiar ideas when helpful

## Return Value

- **Format**: Inline explanation in conversation
- **Structure**: WHAT (overview) -> WHY (problem/purpose) -> HOW (details)

## Examples

1. **Explain Kubernetes scheduler**:
   ```
   /learning:explain Kubernetes scheduler
   ```

   Provides structured explanation starting with what it is, why it exists, then how it works.

2. **Explain RAFT consensus**:
   ```
   /learning:explain RAFT consensus algorithm
   ```

   Breaks down the consensus algorithm from first principles.

## Arguments

- `$ARGUMENTS`: Topic to explain (e.g., "Kubernetes scheduler", "RAFT consensus", "container networking")
