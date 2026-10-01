# Project Pulse Agent Team

The custom agent team for Mona's Project Pulse dashboard is defined in `.github/agents/` and is designed to split planning, design, implementation, and coordination into clear responsibilities.

## Team overview

| Agent | Model | Responsibility | Agent file |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 | Coordinates the overall request, delegates work, manages dependencies, prevents conflicting file ownership, and verifies the final integration. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 | Researches the repository, identifies risks and constraints, and produces an implementation plan with explicit file ownership, validation steps, and handoff requirements. | `.github/agents/planner.agent.md` |
| Designer | Gemini 3.1 Pro | Owns the dashboard UX, UI structure, accessibility, responsive behavior, and design handoff to implementation. | `.github/agents/designer.agent.md` |
| Coder | GPT-5.5 | Implements the approved plan, writes code, fixes bugs, validates behavior, and keeps changes within the assigned file scope. | `.github/agents/coder.agent.md` |

## How the team will work together

1. The Orchestrator defines the Project Pulse goal and coordinates the overall effort.
2. The Planner researches the codebase, dependencies, and constraints, then lays out the delivery phases with file ownership and validation criteria.
3. The Designer focuses on the dashboard experience: layout, visual hierarchy, accessibility, and responsive UI decisions.
4. The Coder implements the approved design and functionality, staying within the assigned scope and validating the result.
5. The Orchestrator manages the handoff between design and implementation and checks that the final dashboard matches the original request.

This workflow keeps responsibilities clean: the Orchestrator leads, the Planner structures the work, the Designer shapes the product experience, and the Coder builds and verifies the app.
