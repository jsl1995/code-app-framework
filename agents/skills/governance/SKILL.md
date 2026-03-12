# Phase 9: Governance & Handover

## Purpose

Ensure the app is documented, supportable, and transferable before it goes live.
Governance is the bridge between build and long-term operation.

## Deliverable

A **Handover Pack** containing app documentation, an operations runbook, naming
conventions reference, and a changelog entry for v1.

## Documentation Checklist

### Repository documentation
- [ ] `README.md` — overview, prerequisites, getting started, architecture summary
- [ ] `ARCHITECTURE.md` — component diagram, hosting model, ALM strategy
- [ ] `CONTRIBUTING.md` — branching strategy, PR process, coding standards reference
- [ ] `CHANGELOG.md` — v1 release entry with features and known issues
- [ ] `.env.example` — all environment variables documented (no real values)

### App catalogue entry

Register the app in the organisation's app catalogue:

```markdown
## App: [Name]

| Field | Value |
|-------|-------|
| Owner | [Name / team] |
| Environment | [Prod environment URL] |
| Solution | [Solution unique name] |
| Data sources | [List of connectors] |
| Premium connectors | [Yes / No — affects licensing requirements] |
| End-user licence | Power Apps Premium |
| Support contact | [email / channel] |
| Source code | [repo URL] |
| Last deployed | [date] |
```

## Operations Runbook

```markdown
# Runbook: [App Name]

## Deployment

1. Build: `npm run build`
2. Push: `pac code push --solution-unique-name <name>`
3. Verify: open app in Power Apps portal with a test user

## Connection reference updates (environment promotion)

When importing solution into a new environment:
1. `pac solution import --path ./solutions/<solution>.zip`
2. Update connection references in Power Apps Admin Center
3. Re-test all connector operations

## Schema changes on a connector

If connector schema changes:
1. `pac code delete-data-source -a <apiName> -ds <dataSourceName>`
2. Re-add: `pac code add-data-source ...`
3. Review and update consuming components
4. Rebuild and redeploy

## Troubleshooting

| Symptom | Likely Cause | Action |
|---------|-------------|--------|
| App fails to load | DLP policy conflict | Check connector grouping in Admin Center |
| Connector calls return 401 | Token expired or connection invalid | Re-authenticate or recreate connection |
| App not visible to users | Not shared | Share via Power Apps portal |
| Premium licence error | User missing licence | Assign Power Apps Premium in M365 Admin |
| Generated service missing | Data source not added | Run `pac code add-data-source` |

## Rollback

1. Export previous solution version from source control
2. `pac solution import --path ./solutions/<previous>.zip --force-overwrite`
```

## Naming Conventions Reference

Publish this for the team so the next developer can navigate the codebase:

| Item | Convention | Example |
|------|-----------|---------|
| Solution | PascalCase | `MyApp` |
| Tables (Dataverse) | PascalCase, singular | `BookingRequest` |
| Columns (Dataverse) | snake_case with prefix | `cr_booking_status` |
| React components | PascalCase | `BookingList.tsx` |
| Custom hooks | camelCase with `use` | `useBookings.ts` |
| Generated services | PascalCase (auto) | `BookingRequestsService.ts` |
| Connection references | snake_case with prefix | `cr_myapp_dataverse` |
| Branches | `feature/[ticket]-description` | `feature/42-add-filters` |
| Commits | Conventional Commits | `feat:`, `fix:`, `docs:` |

## Changelog Template

```markdown
# Changelog

## [1.0.0] — YYYY-MM-DD

### Added
- [List new capabilities from v1 project brief]

### Known Issues
- [Any accepted issues with workaround notes]

### Dependencies
- @microsoft/power-apps: [version]
- Node.js: [LTS version]
- Power Apps CLI: [version]
```

## GitHub Copilot Prompt

```
@workspace Generate a README.md for this Power Apps Code App project.
Include sections: Overview, Prerequisites, Getting Started (local dev),
Architecture, Deployment, Troubleshooting, and Contributing.
Base the content on the project brief and technical plan in [paste docs].
```

## Transition

With the handover pack complete, the app is ready for production release.
Ensure the following gates are passed before go-live:

- [ ] Accessibility audit signed off (Phase 8)
- [ ] UAT signed off (Phase 7)
- [ ] Operations runbook reviewed by support team
- [ ] App shared with the correct security groups in the Prod environment
- [ ] Monitoring / alerting configured (Application Insights recommended)
