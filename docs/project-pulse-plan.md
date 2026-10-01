# Project Pulse Dashboard Implementation Plan

## Goal

Build Mona’s Project Pulse as a lightweight static dashboard that helps contributors scan multiple projects, their owners, statuses, recent activity, priorities or risks, and a short summary. The first view should be a polished, accessible card-based dashboard—not a plain page or server directory listing. Follow `.github/project-pulse-brief.md` and the Step 3 requirements; the repository currently has no existing app implementation or framework to preserve.

## Ownership and file assignments

| File | Owner | Responsibility |
| --- | --- | --- |
| `app/index.html` | Coder | Create the semantic page shell, use the exact title “Project Pulse,” reference `styles.css` and `project-data.json`, and render a `.project-card` for each project with its name, owner, status, `recentActivity`, and priority visible. Include accessible loading, empty, and data-load error feedback. |
| `app/styles.css` | Designer | Define and implement the visual hierarchy, responsive layout, readable spacing, status and priority treatments, accessible contrast and focus states, and polished card styling. Include `.dashboard` and `.project-card` selectors, `border-radius`, and `box-shadow`. Coordinate class names with Coder before implementation. |
| `app/project-data.json` | Coder | Supply deterministic example content under a top-level `projects` array. Every project must include `name`, `owner`, `status`, `recentActivity`, and `priority`. Use concise contributor-friendly values; no real project data is provided in the brief. |
| `.vscode/launch.json` | Coder | Create strict JSON with no comments and a “Run Project Pulse Dashboard” configuration. Serve from `${workspaceFolder}/app` with `python3 -m http.server 5500`; open `http://localhost:%s/index.html` so the browser displays the dashboard rather than a directory listing. |

The **Designer** owns UI/UX decisions and the stylesheet: information hierarchy, layout, accessibility, responsive behavior, and visual polish. The **Coder** owns the HTML/data integration and runnable launch configuration, staying within these assignments. The **Orchestrator** assigns these scopes, agrees on shared interface details, and verifies the integrated result. The **Planner** has completed this plan and has no implementation-file assignment.

## Ordered implementation steps and dependencies

1. **Agree on shared contracts — Orchestrator, Designer, Coder.** Confirm the project data shape from the brief, the HTML-to-CSS class hooks, status/priority presentation, and how loading, empty, and error states will be exposed. This is a short coordination step; it prevents mismatched selectors or assumptions about data.
2. **Build assigned files in parallel — Designer and Coder.** Designer implements `app/styles.css` against the agreed hooks. Coder creates `app/project-data.json`, `app/index.html`, and `.vscode/launch.json` against the brief’s fixed data schema and launch requirements. These file scopes do not overlap. Coder’s HTML must consume the JSON at runtime; the launch setup must serve the `app/` directory.
3. **Integrate and validate sequentially — Orchestrator with Designer and Coder as needed.** Once all four files exist, check that the rendered HTML, CSS hooks, and data fields agree; fix any integration issues within the assigned file scopes. Then perform the launch and browser checks below.

**Parallel work:** After step 1 establishes the interface contracts, Designer’s stylesheet and Coder’s HTML/data/launch files can be developed in parallel because they have separate file ownership. The JSON schema is specified by the brief, so data creation does not need to wait for visual design.

**Sequential work:** Agree on shared hooks before parallel implementation. Integration review and end-to-end launch verification must wait until all assigned files are available. If implementation changes shared class names or data fields, the affected agents must coordinate before final validation.

## Edge cases and assumptions

- The brief defines required data fields but supplies no real project records or allowed status/priority values. Use clearly fictional, deterministic example entries unless Mona provides real data.
- Handle an empty `projects` array and a failed JSON request explicitly; do not silently show a blank dashboard or substitute misleading data.
- Keep all required fields readable if values are long, and ensure cards reflow without horizontal overflow on narrow screens.
- Use semantic structure, keyboard-visible focus, and sufficient contrast; do not rely on badge color alone to communicate status or priority.
- Assumption: this is a static frontend with no backend or package manager. The brief’s `python3 -m http.server` preview is sufficient; do not add dependencies unless a concrete need emerges.
- Confirm the launch configuration’s browser-opening behavior in the target Codespace/VS Code environment, since successful configuration parsing alone does not prove the browser opens.

## Validation expectations

1. Parse both JSON files: `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`.
2. Verify the data has a top-level `projects` array and each entry has `name`, `owner`, `status`, `recentActivity`, and `priority`.
3. Check that `app/index.html` has the exact “Project Pulse” title, references `styles.css` and `project-data.json`, and renders cards showing each required project field. Confirm its selectors match Designer’s CSS.
4. Check `app/styles.css` contains `.dashboard` and `.project-card`, responsive rules, `border-radius`, and `box-shadow`; manually inspect readable spacing, contrast, focus visibility, and narrow-screen layout.
5. Start the launch configuration and verify it serves from `app/`, opens `http://localhost:<port>/index.html`, and displays the dashboard rather than a directory listing. Check browser console and network results for errors, including JSON loading.
6. The Step 3 workflow checks the required files, key HTML/CSS/data phrases, JSON parsing, and launch name/target. Passing those checks is necessary but does not replace the manual rendering, accessibility, responsive, and launch checks above.

Open question: Should the example project names and status/priority vocabulary follow a specific Mona team convention?
