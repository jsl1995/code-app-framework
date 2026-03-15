# Create Dataverse

Read `agents/skills/create-dataverse/SKILL.md` for full guidance, then act directly.

Run this before Phase 6 when Dataverse is the chosen data source.

## Steps

### 1. Read context
Read `UseCase.md` and `docs/data-architecture.md`. Extract the solution name,
environment URL, table definitions, and persona names (for security roles).

Tell the developer:
> "I'll set up the Dataverse tables, columns, relationships, and security roles
> for [app name] in [environment URL]."

### 2. Verify authentication
Run:
```bash
pac auth list
pac org who
```

If not authenticated to the correct environment:
```bash
pac auth create --environment <environmentUrl>
```

### 3. Create the solution
```bash
pac solution create --name "<SolutionName>" --publisher-name "<PublisherName>" \
  --publisher-prefix "<prefix>"
```

### 4. Create tables and columns
For each table in `docs/data-architecture.md`, generate and run the PAC CLI
commands to create the table and each column. Use the column types from the
data architecture (Text, Lookup, Choice, DateTime, Whole Number, Currency, etc.).

Refer to `agents/skills/pac-reference.md` for exact command syntax.

### 5. Create relationships
For each 1:N relationship in the ER diagram, create the lookup column on the
child table pointing to the parent table.

For N:N relationships, create an intersect table or use Dataverse's built-in
many-to-many relationship builder.

### 6. Create security roles
For each persona in UseCase.md, define a Dataverse security role with appropriate
table-level permissions. Walk the developer through creating these in the Power
Platform Admin Center since security roles cannot yet be fully automated via PAC CLI.

### 7. Load sample data
If the developer wants sample data, generate a CSV import file based on the
table schema and realistic values from the app domain. Provide import instructions
for the Dataverse Import Wizard.

### 8. Confirm
Verify tables exist:
```bash
pac code list-tables -a shared_commondataserviceforapps -c <connectionId> \
  -d <orgUrl>
```

When confirmed, say:
> "Dataverse configured. Tables, relationships, and security roles are in place.
> Run `/connectors` to build the connector manifest, then `/implement-code` to build."
