# Agent team

For Mona's Project Pulse dashboard, I will use a small specialist team orchestrated through GitHub Copilot CLI in a Codespace. The custom agent definitions live under `.github/agents/` and each agent has a specific role in the workflow.

- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsible for breaking the project into phases, delegating work to the Planner, Coder, and Designer, assigning file scopes, and verifying that the integrated result works together. Definition: `.github/agents/orchestrator.agent.md`.
- Planner — Model: Claude Opus 4.7 (copilot). Responsible for researching the repository, understanding dependencies and edge cases, and producing a practical implementation plan with ordered steps, file ownership, validation expectations, and open questions. Definition: `.github/agents/planner.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsible for implementing code, fixing logic issues, and creating any required runnable app support files, such as launch configuration for the dashboard. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsible for the dashboard's UI/UX, accessibility, layout, interaction flow, and visual polish so the first screen clearly reads as a Project Pulse frontend. Definition: `.github/agents/designer.agent.md`.

This setup uses GitHub Copilot CLI in a Codespace as the orchestration layer, with the Orchestrator coordinating specialist agents while the learner keeps control of git operations.
