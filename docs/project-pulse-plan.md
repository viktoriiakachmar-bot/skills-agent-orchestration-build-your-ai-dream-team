# Project Pulse Implementation Plan

## Summary

Build a lightweight static **Project Pulse** dashboard for contributors. It will present multiple projects with owner, status, recent activity, priority/risk, and a concise summary in a polished, responsive card layout. The repository has no existing app framework or dashboard implementation, so use dependency-free HTML, CSS, JSON, and browser-native data loading.

## File ownership

| Owner | Files | Responsibility |
|---|---|---|
| Designer | `app/styles.css` | Define visual hierarchy, responsive layout, accessible contrast/focus states, status/priority badges, and polished card styling. |
| Coder | `app/index.html` | Create semantic dashboard markup, load `project-data.json`, render project cards, and expose stable CSS hooks including `.dashboard` and `.project-card`. |
| Coder | `app/project-data.json` | Supply valid contributor-facing sample data under a top-level `projects` array. |
| Coder | `.vscode/launch.json` | Create strict JSON launch configuration named **Run Project Pulse Dashboard** that serves `app/` and opens `index.html`. |
| Orchestrator | All integration boundaries | Confirm contracts, sequence dependent work, review the combined dashboard, and report validation. |

## Ordered implementation steps

1. **Agree on the UI/data contract**
   - Designer specifies the dashboard hierarchy: title/intro, project grid, card fields, status badge, and priority treatment.
   - Coder confirms the stable markup hooks and data fields before implementation.
   - Files: no implementation file changes required in this step.

2. **Create project data**
   - Coder creates `app/project-data.json`.
   - Use a top-level `projects` array with multiple projects.
   - Every project includes `name`, `owner`, `status`, `recentActivity`, and `priority`; add a concise summary if used by the card UI.
   - Files: `app/project-data.json`.

3. **Implement dashboard structure and rendering**
   - Coder creates `app/index.html` with the exact visible title `Project Pulse`.
   - Link `styles.css`, reference/load `project-data.json`, and render one visible `.project-card` per project.
   - Present the required project fields with semantic, accessible markup and a clear empty/error state if data cannot load or is malformed.
   - Files: `app/index.html`.

4. **Implement polished responsive styling**
   - Designer creates `app/styles.css` against the agreed HTML hooks.
   - Include `.dashboard` and `.project-card`, readable spacing, responsive grid behavior, status and priority visual differentiation, `border-radius`, `box-shadow`, and keyboard-visible focus styles.
   - Files: `app/styles.css`.

5. **Add runnable preview configuration**
   - Coder creates `.vscode/launch.json` as strict JSON without comments.
   - Add **Run Project Pulse Dashboard**, run `python3 -m http.server 5500` with `cwd` set to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`.
   - Files: `.vscode/launch.json`.

6. **Integrate and validate**
   - Orchestrator reviews the rendered UI and launch behavior, resolves contract mismatches, and records results.
   - Files: review only unless a defect requires the owning specialist to update its assigned file.

## Dependencies and sequencing

1. The UI/data contract is required before data, markup, and CSS implementation.
2. `app/project-data.json` must define the agreed field names before `app/index.html` can reliably render cards.
3. `app/index.html` must establish final class names and semantic structure before final CSS refinement in `app/styles.css`.
4. `.vscode/launch.json` can be authored once the `app/` directory and `index.html` target are established.
5. Integration validation is sequential and occurs only after all four implementation files exist.

## Parallel work

After the contract is agreed:
- Coder may create `app/project-data.json` while Designer drafts the visual system for `app/styles.css`.
- Coder may create `.vscode/launch.json` in parallel with data and styling because it has no content dependency beyond the known `app/index.html` target.
- Designer can implement CSS in parallel with Coder’s HTML only if the agreed `.dashboard` and `.project-card` hook contract is unchanged.

## Work that must be sequential

- Contract agreement precedes all implementation.
- Data schema precedes completed HTML data rendering.
- Final HTML structure precedes final CSS tuning.
- All implementation precedes end-to-end browser and launch validation.
- Do not let Designer and Coder edit the same file concurrently.

## Edge cases and risks

- A direct `file://` open may block JSON `fetch`; validation must use the configured HTTP server.
- Handle fetch failures, invalid JSON, a missing `projects` array, an empty array, and projects missing required values without leaving a blank or broken page.
- Escape/render data safely with DOM APIs rather than injecting untrusted strings as HTML.
- Ensure status and priority are not conveyed by color alone.
- Verify cards remain readable on narrow screens and long project names, owners, activity text, or priorities do not overflow.
- Avoid a root server URL that shows a directory listing; the launch URL must explicitly target `index.html`.

## Validation expectations

- `app/project-data.json` parses as JSON and has a top-level `projects` array.
- Each project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `app/index.html` contains the exact title `Project Pulse`, links `styles.css`, loads/references `project-data.json`, and creates visible `.project-card` elements showing required fields.
- `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`; the layout is responsive and keyboard focus is visible.
- `.vscode/launch.json` parses as strict JSON, defines **Run Project Pulse Dashboard**, uses `${workspaceFolder}/app`, runs `python3 -m http.server 5500`, and opens `http://localhost:%s/index.html`.
- Run the launch configuration in VS Code and confirm the browser opens the dashboard—not a directory listing—and all project cards are visible.
- Validate a simulated load/data failure produces a concise user-visible error state.
- Run `bash scripts/validate-exercise.sh` after implementation; review any unrelated baseline failures separately.

## Open questions

- No visual brand, exact project count, controlled status vocabulary, or priority scale is defined. Use sensible sample values and document them in the data until stakeholders provide a standard.
- The brief asks for a contributor-friendly summary but does not require a JSON field. Decide during the contract step whether to add a `summary` property or derive static explanatory copy in HTML.
