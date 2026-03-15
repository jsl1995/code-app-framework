# Phase 9: Governance

Read `agents/skills/governance/SKILL.md` for full guidance, then act directly.

## Steps

### 1. Read context
Read `UseCase.md`, `docs/project-brief.md`, `docs/architecture.md`, and
`docs/connector-manifest.md`. Extract the app name, solution name, environments,
persona names, and go-live date.

Tell the developer:
> "[Phase 9/11: Governance] I'll produce the handover pack: repository documentation,
> app catalogue entry, and ops runbook."

### 2. Write repository documentation

**`README.md`** (project root — for the generated app, not this framework):
Write a user-facing README covering: overview, prerequisites, getting started,
architecture summary, and deployment. Use real app name and capability descriptions.

**`ARCHITECTURE.md`**:
Write a concise architecture document referencing `docs/architecture.md` with the
Mermaid component diagram inlined.

**`CONTRIBUTING.md`**:
Write a contributing guide covering: branching strategy, PR process, coding
standards, and the pre-commit hook setup.

**`CHANGELOG.md`**:
Write an initial CHANGELOG with a `[1.0.0]` entry listing the core capabilities
shipped at go-live.

**`.env.example`**:
Write a `.env.example` documenting all environment variable keys (no values) so
new developers know what to fill in.

### 3. Write the ops runbook
Write `docs/runbook.md` covering:
- Deployment procedure (`npm run build` → `pac code push`)
- Connection reference update workflow (when environment changes)
- Schema change workflow (delete + re-add data source)
- Rollback procedure (re-push previous build)
- Who to contact for Power Platform admin tasks

### 4. Write the app catalogue entry
Write `docs/app-catalogue.md` using the template from the SKILL.md. Include app
name, owner, user group, environment URLs, connector list, and support channel.

### 5. Go-live checklist
Run through the go-live gate checklist from the SKILL.md and report the result.
Flag any incomplete items as blockers.

When done, say:
> "Governance pack complete. All documentation written to `docs/`. Review the
> go-live checklist above before releasing to users."
