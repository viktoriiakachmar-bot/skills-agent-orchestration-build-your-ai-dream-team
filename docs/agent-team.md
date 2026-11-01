# Agent team

I will use GitHub Copilot CLI in a Codespace to coordinate the team building Mona's Project Pulse dashboard. Each agent has a defined role and file scope; the Orchestrator manages handoffs and checks the integrated result.

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| Orchestrator | Claude Opus 5.5 (copilot) | Coordinates the specialist agents, turns the plan into dependency-aware phases, assigns file ownership, and verifies the integrated dashboard. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 5.5 (copilot) | Researches the repository and relevant documentation, then produces implementation steps, dependencies, risks, edge cases, and validation expectations without writing code. | `.github/agents/planner.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements the assigned dashboard logic with clear, testable behavior and explicit errors; it can also create assigned runnable-app support such as the Project Pulse launch configuration. | `.github/agents/coder.agent.md` |
| Designer | Gemini 3.7 Flash (copilot) | Owns UI/UX, accessibility, interaction flow, responsive styling, and the polished Project Pulse dashboard presentation. | `.github/agents/designer.agent.md` |

All specialists stay within their Orchestrator-assigned files and do not stage, commit, or push changes; git operations remain under the learner's control through Copilot CLI.

## How the team will work together

1. The Orchestrator asks the Planner to inspect the repository and create an implementation plan in `docs/project-pulse-plan.md`, including phases, dependencies, file ownership, risks, and validation expectations.
2. The Orchestrator reviews the plan, assigns explicit file scopes, and sequences dependent work. The Designer defines the dashboard's layout, accessibility, responsive behavior, and visual direction; the Coder implements the static app and its launch configuration. They can work in parallel when their assigned files do not overlap, and coordinate through the plan when one task depends on the other's output.
3. The Orchestrator integrates the specialists' work and verifies that the Project Pulse dashboard presents project names, owners, statuses, recent activity, and priority, and that the launch configuration opens `app/index.html` from the `app/` directory.
4. The Orchestrator reports validation results and any remaining risks. The learner reviews the outcome and controls all staging, commits, and pushes through Copilot CLI.
