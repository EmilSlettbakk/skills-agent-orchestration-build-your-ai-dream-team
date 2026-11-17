# Agent team

For Mona's Project Pulse dashboard, I will use a small custom agent team defined under `.github/agents/` and orchestrated from GitHub Copilot CLI in a Codespace.

- **Orchestrator** — `Orchestrator` is the coordination agent. Its target model is **Claude Opus 5.5 (copilot)**, and it is responsible for breaking the work into phases, delegating tasks to specialists, and ensuring the pieces fit together. The definition lives at `.github/agents/orchestrator.agent.md`.
- **Planner** — `Planner` is the planning specialist. Its target model is **Claude Opus 5.5 (copilot)**, and it creates the implementation plan, identifies file ownership, estimates dependencies, highlights parallel work, and defines validation expectations. The definition lives at `.github/agents/planner.agent.md`.
- **Designer** — `Designer` is the UI/UX specialist. Its target model is **Gemini 3.7 Flash (copilot)**, and it focuses on dashboard layout, accessibility, information hierarchy, responsiveness, and polished visual design for the Project Pulse frontend. The definition lives at `.github/agents/designer.agent.md`.
- **Coder** — `Coder` is the implementation specialist. Its target model is **GPT-5.5 (copilot)**, and it writes the actual dashboard files (`app/index.html`, `app/styles.css`, `app/project-data.json`) and support configuration such as `.vscode/launch.json` when assigned. The definition lives at `.github/agents/coder.agent.md`.

This workflow uses GitHub Copilot CLI in a Codespace so the learner can coordinate the agents from the terminal, delegate the work explicitly, and verify the final Project Pulse dashboard as a cohesive deliverable.
