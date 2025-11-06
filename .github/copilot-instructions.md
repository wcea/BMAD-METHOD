<!-- .github/copilot-instructions.md for BMAD-METHOD -->

# Copilot / AI assistant instructions — BMAD-METHOD

Purpose: give an AI coding agent the minimal, high-value context needed to be immediately productive in this repository.

Quick contract

- Input: a short user goal (feature, bug, doc edit).
- Output: a concise change (patch or docs) plus references to the files changed and one-line validation steps.
- Success: PR-ready change, passes local sanity checks (lint or build step referenced below).

Start here (high-value files to read)

- `bmad-core/agents/` — agent persona files. Each agent is a Markdown file with a YAML header (role, dependencies, startup instructions). Example: `bmad-core/agents/architect.md`.
- `bmad-core/workflows/`, `bmad-core/templates/`, `bmad-core/tasks/`, `bmad-core/checklists/` — core behavior and reusable instructions.
- `bmad-core/data/technical-preferences.md` — global technical profile that influences agent recommendations.
- `docs/core-architecture.md` and `docs/user-guide.md` — the "why" and system-level workflows.
- `tools/builders/web-builder.js` — how web bundles are created (produces `dist/*.txt` bundles with START/END markers).

Key repository patterns (do not assume a standard code-only project)

- Agents are first-class artifacts: they are Markdown + YAML personas that reference dependencies by name, not file path. Dependencies map to `{root}/{type}/{name}` (e.g. `tasks:create-doc` → `bmad-core/tasks/create-doc.md`).
- Templates embed processing directives (see `bmad-core/utils/template-format.md`) — templates often contain LLM-only instructions and should be treated as executable data, not user-facing raw text.
- Web vs IDE modes:
  - IDE mode: agents are used directly from `bmad-core/agents/` in an IDE with file access.
  - Web mode: run `tools/builders/web-builder.js` (or `npx bmad-method install`) to produce `dist/*.txt` bundles. Bundles include separators like `==================== START: .bmad-core/...` so parse accordingly.

Developer workflows (commands you can run or reference)

- Install / update the BMAD toolset: `npx bmad-method install` or `npm run install:bmad` (requires Node.js >= 20). These commands resolve agents and build bundles.
- Quick checks: README mentions `npm test` used for contribution flow — when editing code, cite or run the project's test/lint scripts if present.

Concrete examples to cite in edits

- When updating an agent persona, modify its YAML header and the Markdown file under `bmad-core/agents/` (e.g., add or change `dependencies:`). Keep startup-instructions intact unless user asks to change behavior.
- To change how templates are processed, update `bmad-core/tasks/create-doc.md` or `bmad-core/utils/template-format.md` — these define template orchestration.

Behavior rules for AI assistance (project-specific)

- Preserve explicit startup and activation steps in agent files. Many agents (e.g., `architect.md`) include strict activation-instructions — do not remove or silently change them.
- When producing web bundles or recommending changes to the web-builder, respect the START/END tag format and the single-file bundle expectation.
- Prefer small, incremental edits: change one agent or one template per PR and include a short validation note (how to verify locally).

What to avoid

- Do not invent agent dependencies or file paths — use the repository mapping convention (`bmad-core/{tasks,templates,checklists,data,agents,workflows}`).
- Do not change `technical-preferences.md` without user approval — it globally alters agent behavior.

If you need more context

- Read `docs/user-guide.md` for workflow-level context and `docs/core-architecture.md` for component responsibilities.
- Ask the human for the intended environment (IDE or Web bundle) and whether the change must be included in a `dist` web bundle.

Short example prompt to the assistant
"Add a new checklist item to the Architect agent to verify database migrations: update `bmad-core/checklists/architect-checklist.md`, reference the change in `bmad-core/agents/architect.md` dependencies, and produce a one-line test (run `npm run install:bmad` and confirm `dist/` includes updated bundle)."

Next steps

- If this file is out-of-date or you'd like different emphasis (more test examples, different command snippets), tell me which sections to expand or trim.
