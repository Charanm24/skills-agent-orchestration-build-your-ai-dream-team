# Project Pulse Dashboard Implementation Plan

## Goal and scope

Deliver Mona's lightweight, static Project Pulse dashboard so contributors can
see multiple projects, their owners, current statuses, recent activity, and
priority or risk at a glance. The required implementation files are exactly:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

The page must render the dashboard from `app/index.html`, not a web-server
directory listing, when opened through the VS Code launch configuration named
**Run Project Pulse Dashboard**.

## Repository findings and confidence

### Verified in this checkout

- The repository currently has no `app/` directory or dashboard source files.
- `.vscode/tasks.json` exists and defines the exercise's Copilot CLI folder-open
  task; it is not a dashboard launch configuration.
- `.vscode/launch.json` does not currently exist.
- No `package.json`, application framework, frontend dependency manifest, or
  application test suite was found. The dashboard brief explicitly asks for a
  small static app.
- `.github/project-pulse-brief.md` specifies the three app files, launch file,
  top-level `projects` array, and per-project fields `name`, `owner`, `status`,
  `recentActivity`, and `priority`.
- `.github/steps/3-step.md` gives additional concrete implementation
  expectations: visible `.project-card` elements, `.dashboard` and
  `.project-card` CSS selectors, polished responsive styles including
  `border-radius` and `box-shadow`, and a launch using
  `python3 -m http.server 5500` with a browser URL ending in
  `/index.html`.
- The repository's current `.github/agents/planner.agent.md` requires ordered
  steps with exact read/create/modify assignments, dependencies, acceptance
  criteria, validation, explicit ownership, and parallel/sequential decisions.
  The Designer, Coder, and Orchestrator definitions respectively require
  bounded design work, repository-aware bounded implementation, and
  coordination without conflicting file ownership.
- `scripts/validate-exercise.sh` validates exercise files and checks launch
  configuration JSON and expected dashboard phrases. It is not an application
  test suite and does not run or inspect a browser.

### Research limitation and assumptions

Planner-style research was requested as part of the intended workflow, but no
Orchestrator/Planner agent-invocation capability is available in this
environment. This plan is therefore grounded in the repository files listed
above and does not claim to be Planner-generated. No existing application
conventions or real project records were found. The Coder should use clearly
illustrative sample project data unless Mona supplies real records; do not
present invented records as verified team facts. The required data schema has
no separate summary property, so use `recentActivity` for the concise
contributor-facing update unless the brief is amended.

The brief does not identify the Python runtime or VS Code Python debugger
extension availability. Treat those as environment checks for launch
validation; do not add dependencies or change devcontainer configuration as
part of this scope.

## Responsibilities and ownership

- **Orchestrator:** Coordinates the phases and handoff, prevents overlapping
  writes, and reviews final integration. Does not implement application code.
- **Planner:** Intended to research constraints and establish this plan. Planner
  was not invokable here; the repository evidence and uncertainty are recorded
  above instead.
- **Designer:** Decides dashboard information hierarchy, visual direction,
  responsive behavior, accessibility, and empty/error/loading states; hands
  those decisions to Coder before implementation. Designer does not write the
  application files in this plan.
- **Coder:** Implements and validates all four required files, following the
  Designer handoff and keeping the implementation static and dependency-free.

Each implementation file has one writer: Coder. This avoids a Designer/Coder
write collision in `app/styles.css` or a partial-ownership handoff for
`app/index.html`.

## Ordered implementation steps

### Step 1 — Define the dashboard experience and handoff

**Owner:** Designer

**Files to read:**

- `.github/project-pulse-brief.md`
- `.github/agents/designer.agent.md`
- `.github/agents/coder.agent.md`
- `.github/steps/3-step.md`
- `docs/agent-team.md`
- `README.md`
- `.vscode/tasks.json`

**Files to create:** None.

**Files to modify:** None.

**Dependencies:** None.

**Acceptance criteria:**

- Provide the Coder an explicit design handoff covering layout, hierarchy,
  card/status/priority treatments, responsive behavior, semantic markup,
  keyboard/focus visibility, contrast, and loading, empty, and data-load error
  feedback.
- Keep `name`, `owner`, `status`, `recentActivity`, and `priority` visible for
  each project without adding unsupported dependencies or silently changing
  the required JSON schema.
- Identify sample content as illustrative unless Mona provides authoritative
  project records.

**Validation:** Orchestrator checks the handoff against the brief and Designer
agent expectations before authorizing implementation. No browser claim is
appropriate at this design-only stage.

### Step 2 — Implement the static dashboard and launch configuration

**Owner:** Coder

**Files to read:**

- `.github/project-pulse-brief.md`
- `.github/agents/coder.agent.md`
- `.github/steps/3-step.md`
- `.github/agents/designer.agent.md`
- `.vscode/tasks.json`
- `.devcontainer/devcontainer.json`
- `scripts/validate-exercise.sh`
- Designer's Step 1 handoff (coordination output; not a repository file)

Before writing, check whether any assigned output file has appeared or has
uncommitted user work since this plan was prepared; preserve it and coordinate
with the Orchestrator rather than overwriting it.

**Files to create:**

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

**Files to modify:** None (the four listed paths are new in the inspected
checkout).

**Dependencies:** Step 1 must complete first. The four outputs are a single
Coder-owned implementation scope: the HTML must agree with the CSS hooks and
JSON schema, and the launch configuration must serve the app directory and
open the HTML entry point.

**Acceptance criteria:**

- `app/index.html` has the exact title **Project Pulse**, references
  `styles.css`, loads `project-data.json`, and renders visible project cards
  with the class `project-card`.
- The dashboard presents each project's name, owner, status, recent activity,
  and priority/risk. It includes a concise contributor-facing update using the
  required `recentActivity` field.
- Markup is semantic and accessible, with meaningful labels/heading structure,
  readable contrast, keyboard-visible focus, and clear loading, empty, and
  fetch-error feedback. Do not assume JSON fetch works when the page is opened
  directly with a `file:` URL; the supported preview is served over HTTP.
- `app/project-data.json` is valid JSON with a top-level `projects` array; each
  record has `name`, `owner`, `status`, `recentActivity`, and `priority`. Any
  unprovided sample records are visibly understood to be illustrative, not
  verified Mona team data.
- `app/styles.css` includes `.dashboard` and `.project-card`, polished readable
  card styling including `border-radius` and `box-shadow`, and responsive
  behavior for narrow screens without horizontal overflow.
- `.vscode/launch.json` is strict JSON with a **Run Project Pulse Dashboard**
  configuration that runs `python3 -m http.server 5500` with the `app/`
  directory as its working directory. Its server-ready action opens
  `http://localhost:%s/index.html`, not the directory root.
- Do not add a package manifest, framework, standalone JavaScript file, or
  other out-of-scope files unless a concrete blocker is raised to the
  Orchestrator for reassignment.

**Validation:**

1. Parse `app/project-data.json` and `.vscode/launch.json` with a JSON parser;
   reject comments or trailing syntax in the launch file.
2. Inspect the HTML references and ensure its selectors/data fields match the
   CSS and JSON, including the exact `.dashboard` and `.project-card` hooks.
3. Run `scripts/validate-exercise.sh` in the repository's supported
   environment, if available. Treat its exercise-level results separately
   from runtime behavior.
4. In VS Code, start **Run Project Pulse Dashboard** and verify the browser
   reaches `http://localhost:5500/index.html` (or the corresponding `%s` port)
   and displays the Project Pulse UI rather than a directory listing. Check a
   narrow viewport and keyboard focus where browser access is available.
   Confirm the Python runtime and debugger support needed by the chosen launch
   configuration; report limitations rather than claiming an unrun check.

### Step 3 — Integrate, review, and report validation

**Owner:** Orchestrator

**Files to read:**

- `.github/project-pulse-brief.md`
- `.github/agents/orchestrator.agent.md`
- `.github/agents/planner.agent.md`
- `.github/agents/designer.agent.md`
- `.github/agents/coder.agent.md`
- `docs/project-pulse-plan.md`
- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`
- `scripts/validate-exercise.sh`

**Files to create:** None.

**Files to modify:** None.

**Dependencies:** Step 2 must complete; Step 1's design handoff must be
resolved.

**Acceptance criteria:**

- The integrated result meets the brief and all Step 2 criteria, with no
  conflicting or unrelated file changes.
- Report what Designer decided, what Coder implemented, which checks actually
  ran, any launch/runtime limitations, and any open question about replacing
  illustrative records with Mona-approved data.
- Do not claim runtime/browser validation unless it was performed.

**Validation:** Review the final diff and worktree scope; verify JSON parsing,
the launch name/server directory/index URL, page-to-data-to-style references,
and exercise-validator output. Record browser/runtime results only if actually
observed.

## Parallel work decisions

- Steps 1 and 2 are **sequential**: Coder depends on Designer's design and
  accessibility handoff.
- Step 3 is **sequential** after implementation because integration review
  depends on all four output files.
- No implementation tasks should run in parallel. Coder owns all four output
  files and must keep their contracts aligned; splitting CSS/HTML/data or the
  launch setup across simultaneous writers would add handoff and integration
  risk for this small static app.
- Orchestrator coordination may prepare validation commands while design is
  underway, but it must not edit the assigned implementation files.

## Open questions / risks

- Are real Mona team project records available, or should the first version
  retain clearly illustrative sample projects?
- Is Python 3 and a compatible VS Code launch/debug environment available to
  the learner? Verify before promising the launch experience; no such
  availability is established by the inspected files.
- There is no existing app test harness. JSON/static checks and the specified
  VS Code/browser smoke test are the appropriate minimum; exercise validation
  does not replace runtime verification.
