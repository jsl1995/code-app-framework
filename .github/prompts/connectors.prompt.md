---
agent: 'agent'
description: 'Phase 5: Build the connector manifest, check DLP compatibility, and plan connection references'
tools: ['editFiles', 'createFile', 'readFile']
---

You are running Phase 5 of a Power Apps Code App build.

Read the full guidance in [connectors skill](../../agents/skills/connectors/SKILL.md).

## Your task

### Step 1: Read context files

Read `UseCase.md` and `docs/data-architecture.md` (from Phase 3). Extract:
- Planned data sources and their types
- DLP policies on the target environment
- Solution name and environment URL

Summarise:
> "Based on your data architecture, I'll map these data sources to Power Platform connectors: [list]. Let me check DLP compatibility and build the manifest."

### Step 2: Build the connector manifest

For each data source from Phase 3, identify:
- The exact Power Platform API name (e.g., `shared_commondataserviceforapps`)
- Connector type: Standard / Premium / Custom
- DLP group: Business / Non-Business (check against UseCase.md DLP policies)
- Whether it is tabular (requires `-t` and `-d` flags) or action-based
- The connection reference logical name to use for ALM
- The operations the app will use (getall, create, update, delete, etc.)

Flag any unsupported connectors (Excel Online Business / OneDrive).

### Step 3: DLP compatibility check

Review the connector manifest against the DLP policies in UseCase.md:
- Are all connectors in compatible DLP groups?
- Are there any Business / Non-Business conflicts?
- If conflicts exist, recommend: DLP exception request, app split, or Power Automate bridge

### Step 4: Generate the manifest document

Produce the full Connector Manifest following the template in the skill file, including
the exact `pac code add-data-source` commands for each connector.

Save as `docs/connector-manifest.md`.

### Step 5: Confirm

Show the manifest to the developer and ask:
> "Does this connector plan look right? Are all DLP constraints satisfied before we start scaffolding?"

Wait for confirmation before finishing.

---

When confirmed, tell the developer:
> "Connector Manifest saved to `docs/connector-manifest.md`. Type `/implement-code` to scaffold and build the app."
