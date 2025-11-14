# AI Utils

Claude Code plugin marketplace containing custom slash commands and utilities.

## Installation

Install the entire marketplace:

```bash
/plugin marketplace add ibihim/ai-utils
```

Or install individual plugins:

```bash
/plugin install learning-plugin@ai-utils
/plugin install status-line-plugin@ai-utils
```

## Plugins

### Learning Plugin

Educational commands for understanding concepts from first principles.

**Commands:**
- `/learning:build-from-scratch <topic>` - Creates Liz Rice-style walkthroughs building concepts incrementally with working code
- `/learning:explain <topic>` - Explains topics using WHAT → WHY → HOW structure

**Example:**
```
/learning:build-from-scratch container runtime
/learning:explain RAFT consensus algorithm
```

### Status Line Plugin

Enhanced status line showing session information with git integration.

**Commands:**
- `/status-line:install-status-line` - Installs custom status line script
- `/status-line:install-prompt-hook` - Installs optional prompt capture hook

**Status line format:**
```
📦 [version] | 🧬 [model] | 🗂️ [path:branch] | 🎨 [style] | 💬 [last prompt...]
```

**Prerequisites:**
- `jq` - JSON parsing (required)
- `git` - Branch display (optional)

## Creating Plugins

Use the template:

```bash
cp -r template/my-first-plugin plugins/your-plugin-name
```

See [AGENTS.md](AGENTS.md) for detailed plugin development guidelines including:
- Command definition format (Linux man page structure)
- Plugin architecture and structure
- Development workflow
- Testing procedures

## Repository Structure

```
.
├── .claude-plugin/
│   └── marketplace.json       # Marketplace configuration
├── plugins/
│   ├── learning/              # Learning plugin
│   └── status-line/           # Status line plugin
├── template/
│   └── my-first-plugin/       # Plugin template
├── AGENTS.md                  # AI agent development guide
└── CLAUDE.md                  # → AGENTS.md
```

## Contributing

Commands follow Linux man page structure with sections:
Name, Synopsis, Description, Implementation, Return Value, Examples, Arguments.

Refer to [AGENTS.md](AGENTS.md) for complete contribution guidelines.
