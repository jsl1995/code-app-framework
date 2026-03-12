# Create Dataverse

## Purpose

Set up the Dataverse environment, create tables, define columns, configure
relationships, security roles, and business rules needed to support the code app.
This skill turns the data architecture design (from `data-structure`) into a real,
working Dataverse schema that the app's connectors will talk to.

## Deliverable

A **configured Dataverse environment** with all tables, columns, relationships,
views, and security roles created — ready for the code app to connect to.

## Prerequisites

- Power Platform environment with Dataverse provisioned
- Power Apps Premium licence (Dataverse is a premium connector)
- Code apps feature enabled on the environment
- System Administrator or System Customizer security role

## Step-by-Step Process

### Step 1: Create or Select a Solution

All Dataverse customisations must live in a solution for ALM (transport across
environments).

```
Power Apps portal → Solutions → + New solution
- Display name: [Client]_[AppName] (e.g., ACME_AssetTracker)
- Publisher: Use your organisation's publisher (or create one)
- Version: 1.0.0.0
```

**Naming convention**:
- Publisher prefix: consistent per client, e.g., `acme`
- Solution name: `[ClientCode]_[AppName]`

### Step 2: Create Tables

For each entity in the data-structure design, create a Dataverse table.

**Copilot prompt — generate table definitions:**
```
@workspace Using the ER diagram and table definitions from
#file:agents/skills/data-structure/SKILL.md, list the pac CLI commands
to create each Dataverse table with all columns, data types, and
required flags. Use the publisher prefix [prefix]_.
```

**Manual creation via Power Apps portal:**
```
Solution → + New → Table
- Display name: [Entity Name]
- Plural name: [auto-generated, verify]
- Schema name: [prefix]_[entityname] (lowercase, no spaces)
- Primary column: Name (or appropriate primary field)
- Type: Standard
- Enable attachments: Yes/No (based on requirements)
```

**Table settings to configure:**
- Ownership: User or Team (affects security model)
- Enable change tracking: Yes (needed for sync scenarios)
- Enable auditing: Yes (for compliance)
- Enable activities: Yes/No (if the entity needs timeline)

### Step 3: Create Columns

For each table, add all columns from the data-structure design.

**Column type mapping:**

| Design Type | Dataverse Type | Notes |
|-------------|---------------|-------|
| string | Single Line of Text | Set max length |
| text | Multiple Lines of Text | Set max length |
| int | Whole Number | Set min/max |
| decimal | Decimal Number | Set precision |
| currency | Currency | Inherits org currency settings |
| boolean | Yes/No | Set default value |
| datetime | Date and Time | Date Only or Date and Time |
| guid (FK) | Lookup | Creates relationship |
| optionset | Choice | Define option values |
| email | Single Line (Email format) | Built-in validation |
| phone | Single Line (Phone format) | Built-in formatting |
| url | Single Line (URL format) | Clickable in model-driven apps |
| file | File | Max size configurable |
| image | Image | Stores in Azure Blob |

**Column naming convention:**
- Schema name: `[prefix]_[columnname]` (lowercase, no spaces)
- Display name: Human-readable with spaces
- Always set description — it documents the schema

### Step 4: Create Relationships

For each relationship in the ER diagram:

**One-to-Many (1:N):**
```
Parent table → Relationships → + New → One-to-many
- Related table: [child table]
- Lookup column name: [prefix]_[parent]id
- Relationship behaviour: Referential (or Parental for cascade delete)
```

**Many-to-Many (N:N):**
```
Table → Relationships → + New → Many-to-many
- Related table: [other table]
- Relationship name: [prefix]_[table1]_[table2]
```

### Step 5: Create Views

Create views for common data access patterns the code app will use:

- **Active [Entities]**: Default view filtering by statecode = Active
- **My [Entities]**: Filtered by owning user = current user
- **Recently Created**: Sorted by createdon desc, top 50
- **[Status] [Entities]**: One view per status value for dashboard metrics

### Step 6: Configure Security Roles

Create custom security roles for each user persona from the brainstorming phase:

```markdown
| Role | Tables | Create | Read | Write | Delete | Append | AppendTo |
|------|--------|--------|------|-------|--------|--------|----------|
| App User | [main entity] | User | Business Unit | User | None | User | User |
| App Manager | [main entity] | User | Organisation | BU | User | User | User |
| App Admin | All app tables | Organisation | Organisation | Organisation | Organisation | Organisation | Organisation |
```

**Security model options:**
- **User-owned tables**: Row-level security via ownership + business units
- **Organisation-owned tables**: All users see all rows (simpler, less secure)
- **Column-level security**: For sensitive fields (salary, PII)

### Step 7: Create Business Rules (Optional)

For server-side validation that should run regardless of how data is accessed
(code app, model-driven app, API, Power Automate):

```
Table → Business rules → + New
- Condition: [field] is required when [other field] = [value]
- Action: Set field required / Show error message / Set default value
```

### Step 8: Verify with PAC CLI

After manual setup, verify everything is correct via CLI:

```bash
# List tables in the solution
pac solution list

# Export the solution for backup
pac solution export --name [SolutionName] --path ./solution-backup.zip

# List connections (needed for Phase 5 — connectors)
pac connection list
```

## Sample Data

After creating the schema, populate with test data for development:

**Copilot prompt — generate sample data script:**
```
@workspace Generate a Power Automate flow (or a TypeScript script using the
Dataverse Web API) that creates 25 sample records in the [Entity] table
with realistic UK data: company names, dates within the last 6 months,
varied statuses, and plausible related records. Include edge cases like
very long names and empty optional fields.
```

## Dataverse Checklist

Before moving to the next phase, verify:

- [ ] All tables created in the solution
- [ ] All columns with correct types, lengths, and required flags
- [ ] All relationships configured with correct behaviour (referential vs parental)
- [ ] Default views created for common access patterns
- [ ] Security roles created and assigned to test users
- [ ] Business rules active (if applicable)
- [ ] Sample data loaded for development
- [ ] Solution exported as backup
- [ ] Connection IDs noted for connector setup

## GitHub Copilot Prompt

```
@workspace I've created the following Dataverse tables: [list tables].
Generate the pac code add-data-source commands I need to connect my
code app to each table. Use the Dataverse connector
(shared_commondataserviceforapps) with connection ID [id] and
org URL [url].
```

## Transition

With Dataverse configured, move to `agents/skills/plan-code/SKILL.md` to plan
the implementation approach, then `agents/skills/implement-code/SKILL.md` to
build the app.
