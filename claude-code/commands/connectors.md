# Phase 5: Connectors

Read `agents/skills/connectors/SKILL.md` for full guidance, then act directly.

## Steps

### 1. Read context
Read `UseCase.md` and `docs/data-architecture.md`. Extract planned data sources,
DLP policies, solution name, and environment URL.

Tell the developer:
> "[Phase 5/11: Connectors] I'll map your data sources to Power Platform connectors,
> check DLP compatibility, and produce the connector manifest."

### 2. Build the connector manifest
For each data source, determine:
- Exact Power Platform API name (e.g., `shared_commondataserviceforapps`)
- Type: Standard / Premium / Custom
- DLP group: Business / Non-Business
- Whether tabular (requires `-t` and `-d` flags) or action-based
- Connection reference logical name (e.g., `cr_[solutionname]_[connector]`)
- Operations required: getall, get, create, update, delete, etc.

Flag any unsupported connectors (Excel Online Business / OneDrive).

### 3. DLP compatibility check
Compare each connector's DLP group against the policies in UseCase.md. If there are
Business / Non-Business conflicts, recommend: DLP exception request, app split, or
Power Automate bridge. Produce a clear Go / No-Go verdict.

### 4. Write the connector manifest
Write `docs/connector-manifest.md` directly. Include:
- The summary manifest table
- Per-connector detail blocks with the exact `pac code add-data-source` commands
- DLP compliance verdict

### 5. Confirm
Show the manifest and ask:
> "Does this connector plan look right? All DLP constraints satisfied before we
> start building?"

Wait for confirmation, then say:
> "Connector Manifest saved to `docs/connector-manifest.md`. Run `/implement-code`
> to scaffold and deploy the app."
