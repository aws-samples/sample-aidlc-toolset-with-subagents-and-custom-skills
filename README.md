# AI-DLC ToolSet with Subagents and Custom Skills

This sample demonstrates how to configure an [AI-DLC (AI-Driven Development Life Cycle)](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/) execution environment using a Main-Subagent architecture, MCP Server connections, and Custom Skills for team-based parallel development workflows.

> **This toolset is designed for [Kiro CLI](https://kiro.dev).**

> ⚠️ **Disclaimer**: This is sample code, for non-production usage. You should work with your security and legal teams to meet your organizational security, regulatory and compliance requirements before deployment.

## Overview

The AI-DLC ToolSet is a ready-to-use configuration that bundles:

- **Agent Structure** — Main Agent (orchestrator) + 2 Subagents (Reverse Engineering, Code Generation)
- **MCP Server Connections** — context7, aws-knowledge-mcp-server, tavily
- **Custom Skills** — requirements-generator, git-merge, drawio
- **Steering Rules** — AI-DLC core workflow and rule details

## Directory Structure

```
.kiro/
├── agents/                    # Agent definitions and prompts
│   ├── aidlc-main.json
│   ├── aidlc-code-generation.json
│   ├── aidlc-reverse-engineering.json
│   └── prompts/
├── skills/                    # Custom Skills
│   ├── requirements-generator/
│   ├── git-merge/
│   ├── drawio/
│   └── skill-creator/        # → See "Adding skill-creator" below
├── steering/                  # Steering rules
│   └── aws-aidlc-rules/
└── aws-aidlc-rule-details/    # Detailed rule references
docs/
└── skills/                    # Skill documentation
    └── en/
```

## Getting Started

### Prerequisites

- [Kiro CLI](https://kiro.dev) installed
- MCP Servers configured (context7, aws-knowledge-mcp-server, tavily)

### Usage

1. Copy the `.kiro/` directory into your project root
2. Configure MCP Server connections in your Kiro CLI settings
3. Start a Kiro CLI session — the AI-DLC workflow will be available

### Adding skill-creator

The `skill-creator` skill (used to generate new Custom Skills such as code review skills) is maintained by Anthropic and is **not included** in this repository. To add it:

```bash
# Clone from Anthropic's skills repository
git clone https://github.com/anthropics/skills.git /tmp/skills

# Copy skill-creator into your .kiro/skills/
cp -r /tmp/skills/skill-creator .kiro/skills/

# Clean up
rm -rf /tmp/skills
```

For more information, see the [skill-creator documentation](https://github.com/anthropics/skills/tree/main/skill-creator).

## Custom Skills

| Skill | Description |
|-------|-------------|
| **requirements-generator** | Analyzes source documents (PDF, markdown, images) to generate structured requirements and constraints documents |
| **git-merge** | Resolves git merge conflicts in AI-DLC projects where multiple developers work on different units in parallel |
| **drawio** | Generates draw.io diagrams as `.drawio` files with optional PNG/SVG/PDF export |

See [docs/skills/en/](docs/skills/en/) for detailed documentation on each skill.

## Agent Architecture

```
┌─────────────────────────────────────────┐
│            aidlc-main                   │
│     (Orchestrator + State Manager)      │
└──────────┬──────────────┬───────────────┘
           │              │
           ▼              ▼
┌──────────────────┐  ┌──────────────────────┐
│ aidlc-reverse-   │  │ aidlc-code-          │
│ engineering      │  │ generation           │
│ (Brownfield      │  │ (Unit-level code     │
│  analysis)       │  │  generation)         │
└──────────────────┘  └──────────────────────┘
```

- **aidlc-main**: Orchestrates the full AI-DLC workflow, manages state, handles user approvals
- **aidlc-reverse-engineering**: Analyzes existing codebases (Brownfield projects) and generates 8 artifact documents
- **aidlc-code-generation**: Executes approved Code Generation Plans, producing code per unit

Each Subagent runs in its own context window and returns only a structured summary to the Main Agent, preserving context efficiency.

## Related Resources

- [AI-DLC Blog Post](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/)
- [Open-Sourcing Adaptive Workflows for AI-DLC](https://aws.amazon.com/blogs/devops/open-sourcing-adaptive-workflows-for-ai-driven-development-life-cycle-ai-dlc/)
- [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows)
- [Kiro CLI](https://kiro.dev)

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the [LICENSE](LICENSE) file.
