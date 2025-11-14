# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Claude Code plugin marketplace repository containing custom slash commands, hooks, and utilities.
The codebase is organized into three main areas:

- `plugins/`: Published plugins available in the marketplace
- `template/`: Template for creating new plugins
- `.claude-plugin/marketplace.json`: Marketplace configuration

## Repository Structure

### Marketplace Configuration

The repository uses `.claude-plugin/marketplace.json` to define available plugins:
- `ibihims-ai-utils` marketplace contains two plugins:
  - `learning-plugin`: Educational commands for understanding concepts
  - `status-line-plugin`: Enhanced status line with git/session info

### Plugin Architecture

Each plugin follows this structure:
```
plugins/<plugin-name>/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata (name, version, author)
├── commands/                # Slash commands (markdown files)
├── hooks/                   # Event hooks (bash scripts)
└── scripts/                 # Helper scripts
```

### Command Definition Format

All commands follow the Linux man page structure for consistency.

**Required YAML frontmatter:**
```markdown
---
description: Brief command description
argument-hint: [optional] [arguments]  # Optional: shown in autocomplete
allowed-tools: Tool1, Tool2            # Optional: restrict available tools
---
```

**Required man page sections:**

1. **Name** - Command identifier
   ```markdown
   ## Name
   plugin-name:command-name
   ```

2. **Synopsis** - Usage syntax
   ```markdown
   ## Synopsis
   ```
   /plugin-name:command-name <arguments>
   ```
   ```

3. **Description** - What the command does
   ```markdown
   ## Description
   The `plugin-name:command-name` command does X, Y, and Z...
   ```

4. **Implementation** - Step-by-step how it works
   ```markdown
   ## Implementation
   1. **Phase name**: Description
      - Step details
      - Tool usage
   ```

5. **Return Value** - Command output format
   ```markdown
   ## Return Value
   - **Format**: Description of output
   - **Location**: File path if applicable
   ```

6. **Examples** - Usage examples (recommended)
   ```markdown
   ## Examples
   1. **Use case description**:
      ```
      /plugin-name:command-name args
      ```
      Expected result description.
   ```

7. **Arguments** - Argument descriptions
   ```markdown
   ## Arguments
   - `$ARGUMENTS`: Description of what arguments are expected
   ```

**Optional sections:**
- **Prerequisites**: Required tools/dependencies with installation commands
- **Notes**: Additional context or caveats

## Available Plugins

### Learning Plugin

Commands for educational purposes:

1. `/build-from-scratch <topic>`: Creates Liz Rice-style walkthroughs
   - Builds concepts from first principles
   - Incremental steps with working code examples
   - Each step must be runnable

2. `/explain <topic>`: Explains topics using first principles
   - WHAT: High-level overview
   - WHY: Problem being solved
   - HOW: Important details

### Status Line Plugin

Custom status line displaying session information:

1. `/config:install-status-line`: Installs status line script
   - Copies `status-line.sh` to `~/.claude/status_line.sh`
   - Updates `~/.claude/settings.json` with status line config
   - Requires `jq` for JSON parsing

2. `/config:install-prompt-hooks`: Installs prompt capture hook
   - Saves user prompts to `/tmp/claude-sessions/prompts-{session_id}.txt`
   - Optional: only needed for prompt display in status line
   - Requires `jq` for JSON parsing

Status line format:
```
📦 [version] | 🧬 [model] | 🗂️ [path:branch] | 🎨 [style] | 💬 [last prompt...]
```

## Development Workflow

### When to Add a Command vs Create a Plugin

1. **Add to existing plugin**: Command fits existing plugin's scope
2. **Create new plugin**: Multiple related commands that warrant their own namespace
3. **Use template**: Start from `template/my-first-plugin/` for new plugins

### Creating a New Command

1. **Create command file**:
   ```bash
   touch plugins/{plugin-name}/commands/{command-name}.md
   ```

2. **Follow man page format** (see Command Definition Format above)

3. **Test the command**:
   - Restart Claude Code or start new session
   - Verify command appears in autocomplete
   - Test command execution

### Creating New Plugins

1. **Use the template**:
   ```bash
   cp -r template/my-first-plugin plugins/{new-plugin-name}
   ```

2. **Update plugin metadata**:
   - Edit `plugins/{new-plugin-name}/.claude-plugin/plugin.json`
   - Set name, description, version, author

3. **Create commands**:
   - Add command markdown files to `commands/`
   - Follow man page structure for each command

4. **Register in marketplace**:
   - Edit `.claude-plugin/marketplace.json`
   - Add plugin entry to `plugins` array

5. **Test plugin**:
   - Install plugin: `/plugin install {plugin-name}`
   - Verify all commands work

### Testing Plugins

Since this repository contains no executable code (only markdown commands and bash scripts):
- Test commands by installing the plugin and running slash commands
- Verify hooks by checking their output in expected locations
- For status line: Check `~/.claude/status_line.sh` output manually

### Installing Plugins

From within Claude Code:
```
/plugin install <plugin-name>
```

Plugin is loaded from `.claude-plugin/marketplace.json`.

## Dependencies

Required system tools:
- `jq`: JSON parsing in bash scripts (status-line plugin)
- `git`: Git branch display in status line

## File Locations

- Plugin configs: `~/.claude/settings.json`
- Status line script: `~/.claude/status_line.sh`
- Prompt hook: `~/.claude/hooks/session-prompt-hook.sh`
- Captured prompts: `/tmp/claude-sessions/prompts-{session_id}.txt`
