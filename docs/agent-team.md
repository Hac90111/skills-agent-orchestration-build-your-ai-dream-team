# Agent team

For Mona's Project Pulse dashboard, I will use a four-agent custom team orchestrated with GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 4.7 (copilot). Responsible for researching the repo, checking relevant docs and dependencies, identifying edge cases, and producing the execution plan. Definition: `.github/agents/planner.agent.md`.
- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsible for breaking the work into phases, delegating tasks to the specialist agents, assigning file scopes, and coordinating progress across the dashboard build. Definition: `.github/agents/orchestrator.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsible for implementing the actual application logic, fixing bugs, and creating any required runnable app support files in the assigned scope. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsible for the UI/UX direction, layout, accessibility, and polished visual design of the Project Pulse dashboard frontend. Definition: `.github/agents/designer.agent.md`.

All agent definitions live under the repository's `.github/agents/` folder, and the work is coordinated through GitHub Copilot CLI in a Codespace so the planner, orchestrator, coder, and designer can collaborate on the implementation in a structured way.
