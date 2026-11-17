# Project Pulse implementation plan

## Goal

Build Project Pulse as a polished, responsive, read-only static dashboard that gives contributors a quick view of active projects, their owners, status, recent activity, priority, and risk. It should render its cards from `app/project-data.json`, open directly at `app/index.html` through the VS Code launch configuration, and require no framework or server-side build step.

## Implementation phases

### 1. Agree on the dashboard and data contract
- **Designer:** define the dashboard's visual direction, information hierarchy, card content order, responsive behavior, and accessible status/priority treatments.
- **Coder:** define the JSON fields and corresponding DOM hooks; confirm the data can be loaded by the static preview setup.
- Agree on the project fields (`name`, `owner`, `status`, `recentActivity`, and `priority`) and stable class names before parallel implementation.

### 2. Establish the static app and preview path
- **Coder:** create the `app/` structure and `.vscode/launch.json`; configure the launch target to open `app/index.html` with `app/` as its working directory.
- Confirm the chosen preview path supports loading the JSON data (for example, opening a local file versus serving the app), and document/configure the expected path rather than relying on a directory listing.

### 3. Prepare the content and dashboard structure
- **Coder — `app/project-data.json`:** create a readable top-level `projects` array with realistic project examples and the agreed fields.
- **Coder — `app/index.html`:** implement the semantic page shell, header, summary area, project-card container, stylesheet reference, and simple deterministic rendering of the JSON data. Include accessible labels and an empty/error state.
- Ensure the markup and rendering logic use the agreed data contract and class names.

### 4. Apply the visual design
- **Designer — `app/styles.css`:** implement the visual system, responsive layout, typography, spacing, card treatment, and status/priority styling; check contrast and narrow-screen readability.
- **Coder:** integrate the stylesheet with the HTML and adjust DOM hooks or layout details as needed to match the agreed design, without changing the data contract unilaterally.
- Designer reviews the integrated page and provides any final visual/accessibility adjustments.

### 5. Integrate and validate
- **Coder:** verify data loading/rendering, edge cases, and the VS Code launch configuration; fix implementation issues.
- **Designer:** review hierarchy, responsive behavior, and visual/accessibility quality.
- **Orchestrator:** coordinate completion and ensure all acceptance checks below pass.

## File assignments and dependencies

| File | Primary responsibility | Assignment | Depends on |
| --- | --- | --- | --- |
| `app/project-data.json` | Project content and agreed `projects` schema | Coder | Phase 1 data contract |
| `app/index.html` | Semantic shell, data loading/rendering, empty/error states, CSS link | Coder; Designer reviews structure and hierarchy | Data contract; JSON schema for rendering |
| `app/styles.css` | Visual system, responsive layout, accessible state treatments | Designer authors; Coder integrates and makes implementation adjustments | Agreed HTML structure/class hooks and status/priority labels |
| `.vscode/launch.json` | Launch `app/index.html` in the expected preview context | Coder | App location and selected preview mechanism |

Dependencies are file-level as well as phase-level: the HTML renderer must match the JSON schema; CSS selectors must match the HTML hooks; and the launch configuration must target the actual app entry point and support its data-loading method. The launch configuration can be authored early, but its final behavior can only be verified once the app and preview mechanism are in place.

## Responsibilities

- **Designer:** own visual direction and `app/styles.css`; define card hierarchy, responsive behavior, and accessible contrast/state cues; review the integrated page and request focused visual fixes.
- **Coder:** own `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`; implement the agreed data contract, semantic markup, rendering and graceful failure states; integrate Designer's styles and verify the preview flow.
- **Orchestrator:** keep the schema and markup/style contracts aligned, resolve integration blockers, and coordinate final validation. It does not replace the file owners.

## Parallel work decisions

- **Can run in parallel after Phase 1:** Coder can create the JSON content and launch configuration while the Designer starts the CSS foundation against the agreed card structure and class hooks.
- **Can overlap with constraints:** Coder can build the HTML shell while the Designer develops styles, once the DOM contract is stable. Both should coordinate any changes to hooks or content order.
- **Must remain sequential:** agree on the data/DOM contract before rendering and CSS integration; finalize the JSON schema before completing data binding; complete integration before end-to-end preview and final QA.
- **Decision:** parallelize independent setup and design work, but do not parallelize competing changes to the JSON/DOM/CSS contract. Integrate and review as one dashboard before validation.

## Validation expectations

- `app/project-data.json` is valid JSON and follows the agreed schema.
- The configured preview opens `app/index.html` directly (not a directory listing), and the chosen preview path can load `project-data.json`.
- The dashboard renders multiple project cards with each project's name, owner, status, recent activity, and priority.
- Empty/missing data and data-load failures show a useful fallback instead of a broken page; long names and activity text wrap without breaking the layout.
- Status and priority remain understandable without color alone, and the page has semantic headings, descriptive labels, adequate contrast, and a readable narrow-screen layout.
- `.vscode/launch.json` is valid JSON and launches the dashboard without manual edits.
- Review at narrow and wide viewport widths and check the browser console for loading or rendering errors.
