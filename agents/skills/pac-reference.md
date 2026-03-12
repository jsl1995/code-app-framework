# PAC CLI — Code Apps Command Reference

Quick reference for all `pac code` commands used in the build lifecycle.
Source of truth: [Microsoft Learn — pac code](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/code).

## Authentication

```bash
# Authenticate to a Power Platform environment
pac auth create --environment <environmentUrl>

# List authenticated profiles
pac auth list

# Select active profile
pac auth select --index <n>
```

## Project Initialisation

```bash
# Scaffold a new code app (react-ts | vue-ts | html)
pac code init --name "MyApp" --framework react-ts

# Alternative: npm-based CLI (v1.0.4+, useful for CI/CD without dotnet)
npx @microsoft/power-apps init --name "MyApp" --framework react-ts
```

## Connection Management

```bash
# List all connections and their API names and IDs
pac connection list
```

## Data Source Discovery

Run these before adding data sources to get exact, case-sensitive names.

```bash
# List datasets available for a connection
pac code list-datasets -a <apiName> -c <connectionId>

# List tables within a dataset
pac code list-tables -a <apiName> -c <connectionId> -d <datasetName>

# List SQL stored procedures within a dataset
pac code list-sql-stored-procedures -c <connectionId> -d <datasetName>
```

## Adding Data Sources

### Via connection reference (recommended for ALM)

```bash
# List solutions to find solution ID
pac solution list --json | ConvertFrom-Json | Format-Table

# List connection references in a solution
pac code list-connection-references -env <environmentURL> -s <solutionID>

# Add action-based connector via connection reference
pac code add-data-source -a <apiName> -cr <connectionReferenceLogicalName> -s <solutionID>

# Add tabular connector via connection reference
pac code add-data-source \
  -a <apiName> \
  -cr <connectionReferenceLogicalName> \
  -s <solutionID> \
  -t <tableId> \
  -d <datasetName>
```

### Via direct connection ID (local dev / quick testing only)

```bash
# Action-based connector (e.g. Office 365 Users)
pac code add-data-source -a <apiName> -c <connectionId>

# Tabular connector (e.g. Dataverse, SQL, SharePoint)
pac code add-data-source -a <apiName> -c <connectionId> -t <tableId> -d <datasetName>

# SQL stored procedure
pac code add-data-source -a <apiName> -c <connectionId> -d <datasetName> -sp <storedProcedureName>
```

## Removing Data Sources

```bash
pac code delete-data-source -a <apiName> -ds <dataSourceName>
```

> **Schema changes**: No refresh command exists. If a connector schema changes,
> delete the data source and re-add it.

## Build & Deploy

```bash
# Local dev server
npm run dev

# Production build
npm run build

# Push to Power Platform (pipeline shorthand)
npm run build && pac code push --solution-unique-name <solutionName>

# Push without build (if already built)
pac code push --solution-unique-name <solutionName>
```

## Solution Management

```bash
# List solutions in the environment
pac solution list

# Export a solution (for backup / ALM)
pac solution export --name <solutionName> --path ./solutions/

# Import a solution
pac solution import --path ./solutions/<solution>.zip
```

## Unsupported Connectors

These connectors are **not** supported in code apps (as of initial release):

- Excel Online (Business) — `shared_excelonlinebusiness`
- Excel Online (OneDrive) — `shared_excelonline`

Do not include these in connector manifests.
