# Branch Flow - Single-Task Autonomous Development Workflow

## Overview

Branch Flow is a simplified, branch-based autonomous coding workflow inspired by Auto-Claude. It provides structured planning, implementation, QA validation, and merge capabilities—all using standard git branches instead of worktrees.

**Key Principles:**
- **Single task focus**: One task at a time for complex work
- **Branch-based isolation**: Standard git branches, not worktrees
- **Spec-driven development**: Every task starts with a specification
- **Self-validating QA**: Built-in quality assurance before merge
- **Memory persistence**: Context maintained across sessions

## Workflow Phases

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   1. SPEC   │───▶│  2. PLAN    │───▶│  3. BUILD   │───▶│  4. REVIEW  │───▶│  5. MERGE   │
│   Create    │    │   Design    │    │  Implement  │    │   QA Loop   │    │  Integrate  │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

### Phase 1: SPEC (Specification)
- Define what you want to build
- Capture requirements and acceptance criteria
- Save to `.branch-flow/specs/`

### Phase 2: PLAN (Design)
- Analyze codebase for implementation approach
- Break down into atomic tasks
- Create implementation checklist
- Save to `.branch-flow/plans/`

### Phase 3: BUILD (Implementation)
- Create feature branch from base
- Implement according to plan
- Follow TDD when appropriate
- Regular commits with meaningful messages

### Phase 4: REVIEW (QA Validation)
- Run tests and linting
- Verify against spec requirements
- Self-review code quality
- Loop until all checks pass

### Phase 5: MERGE (Integration)
- Create PR or merge directly
- Update memory with learnings
- Clean up working files

## Directory Structure

```
your-project/
├── .branch-flow/
│   ├── specs/           # Task specifications
│   │   └── 001-feature-name.md
│   ├── plans/           # Implementation plans
│   │   └── 001-feature-name-plan.md
│   ├── docs/            # Library documentation references
│   │   └── react-query-ref.md
│   ├── memory/          # Persistent context
│   │   ├── project-context.md
│   │   ├── decisions.md
│   │   └── learnings.md
│   ├── scripts/         # Utility scripts
│   │   └── bf-search.py
│   ├── index/           # Semantic search index (auto-created)
│   ├── current-task.json # Active task state
│   └── config.json      # Workflow configuration
└── .claude/
    └── commands/        # Slash commands
```

## Commands

| Command | Description |
|---------|-------------|
| `/bf:init` | Initialize Branch Flow in project |
| `/bf:spec` | Create new task specification |
| `/bf:plan` | Generate implementation plan from spec |
| `/bf:build` | Start implementation (creates branch) |
| `/bf:review` | Run QA validation loop |
| `/bf:merge` | Complete task and integrate |
| `/bf:status` | Show current task status |
| `/bf:abort` | Abandon current task |
| `/bf:search` | Semantic search across codebase & memory |
| `/bf:similar` | Find files similar to a given file |
| `/bf:index` | Build/update semantic search index |
| `/bf:docs` | Fetch & summarize library documentation |
| `/bf:ideate` | Analyze codebase and generate improvement ideas |
| `/bf:ideas` | List and filter generated ideas |
| `/bf:idea-approve` | Create GitHub issue from idea |
| `/bf:idea-dismiss` | Dismiss idea from list |

## Usage Example

```bash
# Initialize in your project
/bf:init

# Create a new task
/bf:spec "Add user authentication with JWT"

# Generate implementation plan
/bf:plan

# Start building (creates branch: bf/001-add-user-auth)
/bf:build

# ... Claude implements according to plan ...

# Run QA validation
/bf:review

# Complete and merge
/bf:merge
```

## Configuration

`.branch-flow/config.json`:
```json
{
  "baseBranch": "main",
  "branchPrefix": "bf/",
  "autoCommit": true,
  "requireTests": true,
  "requireLint": true,
  "autoMerge": false,
  "prTemplate": true
}
```

## Memory System

The memory layer persists insights across sessions:

- **project-context.md**: Architecture, patterns, conventions
- **decisions.md**: Key decisions and rationale
- **learnings.md**: What worked, what didn't

Memory is automatically updated after each completed task.

## Semantic Search

Branch Flow includes local semantic search powered by Ollama:

### Setup
```bash
ollama serve                    # Start Ollama
ollama pull nomic-embed-text    # Pull model (auto during install)
/bf:index                       # Build search index
```

### Usage
```bash
/bf:search "authentication patterns"    # Search everything
/bf:search "error handling" --code      # Code only
/bf:search "why JWT" --memory           # Memory only
/bf:similar src/auth/jwt.ts             # Find similar files
```

### Model Configuration
Environment variables:
- `BF_EMBEDDING_MODEL` - Model name (default: nomic-embed-text)
- `BF_OLLAMA_URL` - Ollama URL (default: http://localhost:11434)

Or set in `.branch-flow/config.json`:
```json
{
  "embedding": {
    "model": "nomic-embed-text",
    "dimensions": 768
  }
}
```

## Documentation Research (Context7)

Fetch and summarize external library documentation efficiently:

```bash
/bf:docs react-query           # Fetch and summarize
/bf:docs "prisma relations"    # Specific feature
```

**How it works:**
1. Context7 MCP fetches latest docs
2. Only relevant APIs/patterns extracted
3. Saved to `.branch-flow/docs/[lib]-ref.md` (~50-100 lines)
4. Full docs discarded, concise reference kept

**During planning:** Automatically prompts to fetch docs for detected dependencies.

**Setup:** Add Context7 to `.claude/mcp.json`:
```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@context7/mcp"]
    }
  }
}
```

## Ideation (Codebase Analysis)

Generate improvement ideas by analyzing your codebase:

```bash
/bf:ideate                     # Analyze and generate 25 ideas
/bf:ideate --category security # Focus on one category
/bf:ideas                      # List all ideas
/bf:ideas 3                    # View idea #3 details
/bf:idea-approve 3             # Create GitHub issue from idea
```

**Categories:**
- `</>` Code Quality - Duplication, complexity, testing, linting
- `✨` UI/UX - Usability, accessibility, consistency
- `🔒` Security - Vulnerabilities, auth, data exposure
- `📚` Documentation - Missing docs, API docs, architecture
- `⚡` Performance - Slow operations, caching, optimization

**Workflow:**
```
/bf:ideate          → Generate ideas (saved to .branch-flow/ideas/)
/bf:ideas           → Review and prioritize
/bf:idea-approve 3  → Creates GitHub issue #45
/bf:spec "Fix #45"  → Start spec linked to issue
/bf:merge           → PR closes issue automatically
```

## UI/UX Pro Skill (Optional)

When installed, provides design system guidance for UI-related work.

**Install:**
```bash
npm install -g uipro-cli
uipro init --ai claude
```

**When to Use:**

Automatically consult `.claude/skills/uipro/SKILL.md` when working on:
- User interface design or implementation
- Component styling and visual design
- Accessibility (a11y) concerns
- Responsive layouts and mobile design
- Forms, modals, buttons, navigation patterns
- Loading states, empty states, error states

**CLI Commands:**
```bash
uipro patterns <type>    # Get design patterns (button, form, modal, etc.)
uipro a11y <file>        # Accessibility audit
uipro colors <color>     # Generate accessible color palette
uipro check              # Review design consistency
```

## Best Practices

1. **One task at a time**: Complete or abort before starting new
2. **Clear specs**: Detailed requirements prevent scope creep
3. **Small tasks**: Break large features into smaller specs
4. **Review often**: Run `/bf:review` frequently during build
5. **Document decisions**: Update memory with context

## Integration with Claude Code

This workflow integrates with Claude Code's native features:
- Uses `/agents` for specialized tasks
- Leverages hooks for automation
- Compatible with other skills and plugins
