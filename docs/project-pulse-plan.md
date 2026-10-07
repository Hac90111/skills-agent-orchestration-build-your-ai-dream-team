# Project Pulse dashboard implementation plan

## Goal

Build a polished, responsive static dashboard that lets contributors quickly see active projects, owners, current status, recent activity, priority or risk, and a concise project summary. Keep the implementation lightweight: no framework or package dependency is needed. The dashboard must load the project data and open as `index.html` in the VS Code **Run Project Pulse Dashboard** launch configuration.

## Team responsibilities

- **Planner:** establish the phases, file ownership, dependencies, parallel work boundaries, and acceptance checks in this plan.
- **Designer:** own the dashboard's information hierarchy, semantic page structure, responsive layout, accessible color and status treatment, and visual styling. Coordinate the HTML hooks with the Coder before either side finalizes its implementation.
- **Coder:** own the JSON data contract and representative project records, connect the page to that data, create the VS Code preview configuration, and verify the complete app flow. Keep the implementation within the assigned files and report validation results.

## File assignments

| File | Owner | Scope and constraints |
|---|---|---|
| `app/index.html` | Designer (structure); Coder (data integration) | Semantic dashboard shell, page title and heading, summary/overview region, project-list container, accessible status and priority labels, and deterministic hooks including `.dashboard` and `.project-card`. Load `styles.css` and render from `project-data.json`; handle loading and fetch failures visibly. Avoid hard-coding project cards that duplicate the JSON records. |
| `app/styles.css` | Designer | Visual hierarchy, readable typography and spacing, responsive project-card layout, status badges, priority/risk treatment, visible focus states, sufficient contrast, and polished card styling such as rounded corners and shadows. Keep selectors aligned with the agreed HTML structure. |
| `app/project-data.json` | Coder | Valid JSON with a top-level `projects` array. Each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`; include a concise `summary` for the contributor-friendly overview. Use consistent, presentation-ready status and priority values and enough representative records to demonstrate the layout. |
| `.vscode/launch.json` | Coder | Strict JSON with a deterministically named **Run Project Pulse Dashboard** configuration. Serve the static files with the working directory set to `${workspaceFolder}/app`, then open `http://localhost:<port>/index.html` so the browser shows the UI, not a directory listing. Prefer an available built-in/runtime tool (for example, the Python standard-library HTTP server) rather than adding a project dependency. |

## Dependencies and execution phases

1. **Agree on the data/render contract — Planner, Designer, and Coder.** Confirm the required fields, the additional `summary` content, status/priority values, and the DOM hooks the rendering code will target. This prevents mismatched data and presentation.
2. **Build independent foundations — parallel.**
   - Designer creates the semantic shell and responsive visual treatment in `app/index.html` and `app/styles.css`, using sample structure/hooks but not duplicating records.
   - Coder creates representative `app/project-data.json` records against the agreed contract.
   These tasks can proceed in parallel after step 1 because the schema and hooks have been agreed. The Designer owns the shared HTML/CSS files; the Coder does not alter styling.
3. **Integrate data — sequential after steps 1 and 2.** Coder connects the page to the JSON data and adds visible loading/error feedback. Confirm markup hooks, status/priority text, and styles remain aligned with the Designer's work. Use an HTTP server for preview because browser `fetch()` may not load a sibling JSON file from a `file://` page.
4. **Configure preview — after the app paths and runtime are known.** Coder adds `.vscode/launch.json`, serving from `app/` and opening `index.html` at a stable local URL. Do not introduce a separate application package or dependency solely for preview.
5. **Integrated validation and polish — sequential.** Designer and Coder review the launched dashboard together, address any accessibility, responsive, data, or path issues in their assigned files, and report the checks performed.

The Designer's HTML and CSS work is a single coordinated scope because the selectors and layout must agree. JSON authoring can run alongside it once the contract is fixed. Data integration and launch verification depend on the files and paths being established, so they follow the parallel work.

## Validation expectations

- Confirm that all four assigned files exist and that `app/project-data.json` and `.vscode/launch.json` parse as strict JSON; the launch file must contain no comments.
- Check that the data has a top-level `projects` array and each record supplies the required fields (`name`, `owner`, `status`, `recentActivity`, `priority`) plus the concise summary used by the UI.
- Inspect the rendered page for a clear Project Pulse heading, project cards, status and priority indicators, summaries, and recent activity; confirm no project cards are missing or duplicated relative to the data.
- Start **Run Project Pulse Dashboard** and verify that it serves from `app/` and opens `http://localhost:<port>/index.html`. Confirm the response is the dashboard page, not a directory listing, and that the JSON request succeeds.
- Exercise empty or unavailable project data and confirm the page gives contributors a useful empty/error message instead of silently appearing successful or failing with a blank dashboard.
- Check narrow and wide viewport layouts, keyboard focus visibility, semantic headings/labels, and readable contrast for status and priority badges.
- Run the repository exercise validator if available, then report which checks were automated and which were manually reviewed. Do not claim browser/runtime behavior based only on static file checks.

## Risks and edge cases

- A `file://` preview can prevent browser JSON fetching; use the launch configuration's local HTTP server.
- Keep the data values and their labels consistent so unfamiliar status or priority strings do not become ambiguous visual badges.
- The dataset may be empty or unavailable; provide an intentional empty state and visible loading/error states.
- Avoid color-only status communication; include readable text and preserve contrast and keyboard-accessible focus.
- The repository currently has no app package manifest or app dependencies. Keep the solution static and use available runtime tooling unless implementation reveals a genuine missing prerequisite.

## Open decisions

- Confirm the final status and priority vocabulary with the team when representative project records are chosen; keep the values consistent across JSON and presentation.
- If the Codespace does not have the proposed HTTP-server runtime available, choose another already-installed static server and update the launch configuration without adding unrelated dependencies.
