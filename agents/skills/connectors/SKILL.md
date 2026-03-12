# Phase 5: Connectors & Data Sources

## Purpose

Map every data requirement from Phases 3 and 4 to a specific Power Platform connector,
document connection references, plan the `pac code add-data-source` commands, and
verify DLP compatibility. This phase produces the connector manifest that drives
the scaffolding in Phase 6.

## Deliverable

A **Connector Manifest** listing every connector, its type, the operations used,
and the CLI commands to wire them up.

## How Connectors Work in Code Apps

### The flow:
1. You create a **connection** in the Power Apps portal (or reuse an existing one)
2. You run `pac code add-data-source` with the connector's API name and connection ID
3. The CLI generates typed `*Model.ts` and `*Service.ts` files in `generated/services/`
4. Your code imports and calls these generated services — no raw HTTP, no auth code
5. At runtime, the Power Apps SDK routes calls through Power Platform's connector runtime

### Important constraints:
- You cannot create new connections via the CLI — connections must exist in Power Apps first
- All connectors are supported except those listed in the official limitations
- DLP policies are enforced at app launch: if your app uses connectors in conflicting
  DLP groups, it will fail to start
- Each connector respects the signed-in user's permissions (row-level, column-level)

## Connector Categories

### Standard Connectors (included in base licence)
Common examples:
- **Office 365 Users** — user profiles, photos, manager chain
- **Office 365 Outlook** — send email, calendar events
- **SharePoint** — list items, document libraries
- **Microsoft Teams** — post messages, get channels
- **Excel Online (Business)** — read/write Excel tables (prototyping only)
- **Approvals** — start and monitor approval flows

### Premium Connectors (require Power Apps Premium licence)
Common examples:
- **Dataverse** — full CRUD on Dataverse tables, FetchXML queries
- **SQL Server / Azure SQL** — query tables, execute stored procedures
- **HTTP with Azure AD** — call any Azure AD-protected API
- **Azure Blob Storage** — file upload/download
- **Salesforce**, **SAP**, **ServiceNow** — enterprise integrations

### Custom Connectors
- Wrap any REST API as a Power Platform connector
- Created via the Power Apps portal or using a Swagger/OpenAPI definition
- Premium by default unless certified by Microsoft

## Connector Planning Template

For each connector the app needs:

```markdown
### Connector: [Name]

| Field | Value |
|-------|-------|
| API Name | e.g., `shared_commondataserviceforapps` |
| Type | Standard / Premium / Custom |
| Connection ID | [from `pac connection list` or Power Apps portal] |
| DLP Group | Business / Non-Business / Blocked |
| Operations Used | List: e.g., GetItems, CreateItem, UpdateItem, DeleteItem |
| Tabular? | Yes (requires table ID and dataset) / No (action-based) |
| Tables (if tabular) | e.g., `accounts`, `contacts`, `cr_customtable` |

**pac command**:
```bash
# Action-based connector
pac code add-data-source -a <apiName> -c <connectionId>

# Tabular connector (e.g., Dataverse, SharePoint, SQL)
pac code add-data-source -a <apiName> -c <connectionId> -t <tableId> -d <datasetName>
```

**Generated files**:
- `generated/services/[Name]Model.ts`
- `generated/services/[Name]Service.ts`
```

## Common Connector Patterns

### Dataverse (most common for enterprise apps)

```bash
# List your connections
pac connection list

# Add Dataverse data source for a specific table
pac code add-data-source \
  -a shared_commondataserviceforapps \
  -c <connectionId> \
  -t <logicalTableName> \
  -d <orgUrl>
```

Usage in code:
```typescript
import { DataverseService } from './generated/services/DataverseService';
import type { Account } from './generated/services/DataverseModel';

// The SDK handles auth — just call the service
const accounts: Account[] = await DataverseService.getAccounts({
  $filter: "statecode eq 0",
  $top: 50,
  $orderby: "name asc"
});
```

### Office 365 Users

```bash
pac code add-data-source -a shared_office365users -c <connectionId>
```

Usage in code:
```typescript
import { Office365UsersService } from './generated/services/Office365UsersService';

const me = await Office365UsersService.MyProfile();
const photo = await Office365UsersService.UserPhoto({ userId: me.Id });
```

### SharePoint

```bash
pac code add-data-source \
  -a shared_sharepointonline \
  -c <connectionId> \
  -t <listId> \
  -d <siteUrl>
```

## DLP Compatibility Check

Before finalising the connector list, verify DLP compliance:

1. List the DLP policies on the target environment:
   - Power Platform Admin Center → Policies → Data policies
2. Check which group each connector falls into (Business / Non-Business / Blocked)
3. **Rule**: Connectors in the Business group cannot share data with connectors in
   the Non-Business group within the same app
4. If there's a conflict, options are:
   - Request a DLP policy exception from the admin
   - Separate the data flow into two apps
   - Use Power Automate as a middleware (flow runs in its own context)

## Connector Manifest Template

```markdown
# Connector Manifest: [App Name]

## Environment
- Name: [env name]
- URL: [env url]
- DLP Policy: [policy name]

## Connectors

| # | Connector | API Name | Type | DLP Group | Tables | Operations |
|---|-----------|----------|------|-----------|--------|------------|
| 1 | Dataverse | shared_commondataserviceforapps | Premium | Business | accounts, contacts | CRUD |
| 2 | Office 365 Users | shared_office365users | Standard | Business | — | MyProfile, UserPhoto |
| 3 | SharePoint | shared_sharepointonline | Standard | Business | Documents | GetItems, CreateItem |

## DLP Compatibility
✅ All connectors are in the same DLP group (Business). No conflicts.

## CLI Commands (run in order)

```bash
pac code add-data-source -a shared_commondataserviceforapps -c <id> -t accounts -d <orgUrl>
pac code add-data-source -a shared_office365users -c <id>
pac code add-data-source -a shared_sharepointonline -c <id> -t <listId> -d <siteUrl>
```

## Licence Impact
- Premium connectors used: Dataverse
- All end users require Power Apps Premium licence
```

## GitHub Copilot Prompt

After running the `pac code add-data-source` commands and getting the generated
service files:

```
@workspace Review the generated service files in /generated/services/.
Create a data access layer in src/services/ that:
- Re-exports each generated service with a friendlier name
- Adds error handling wrappers around each operation
- Adds TypeScript return types for each method
- Includes JSDoc documentation for each service method
Show me the implementation for [primary connector] first.
```

## Transition to Phase 6

With connectors planned and documented, read `agents/skills/implement-code/SKILL.md` to
initialise the project, add data sources, and build the app.
