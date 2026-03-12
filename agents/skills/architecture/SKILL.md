# Phase 2: Solution Architecture

## Purpose

Design the high-level technical architecture before any code is written. This phase
produces the component diagram, hosting model decisions, ALM strategy, and security
posture that govern all subsequent phases.

## Deliverable

A **Solution Architecture Document** containing a component diagram, hosting decisions,
ALM plan, and security checklist.

## How to run this phase

Read `UseCase.md` before asking the developer any questions. Extract the environment
URL, DLP policies, solution name, target environments, and ALM preferences. Only ask
about what UseCase.md doesn't answer.

## Architecture Components

A Power Apps code app has three runtime layers:

| Layer | Owned by | Responsibility |
|-------|---------|---------------|
| Your SPA code | Developer | UI, business logic, state management |
| `@microsoft/power-apps` client library | Microsoft | Connector proxying, auth token management, SDK APIs |
| Power Apps host | Microsoft | Authentication, app loading, DLP enforcement, error presentation |

### Component Diagram Template

```mermaid
graph TD
    subgraph Browser
        SPA[React SPA]
        SDK[@microsoft/power-apps SDK]
        SPA --> SDK
    end

    subgraph PowerPlatform["Power Platform (Managed)"]
        HOST[Power Apps Host]
        CONNECTORS[Connector Runtime]
        DLP[DLP Enforcement]
        AUTH[Microsoft Entra Auth]
    end

    subgraph DataSources["Data Sources"]
        DV[Dataverse]
        SQL[Azure SQL]
        O365[Office 365]
    end

    SDK --> HOST
    HOST --> AUTH
    HOST --> CONNECTORS
    HOST --> DLP
    CONNECTORS --> DV
    CONNECTORS --> SQL
    CONNECTORS --> O365
```

## Hosting Model

Code apps are hosted by Power Platform. There is no infrastructure to manage.

| Concern | Platform behaviour |
|---------|-------------------|
| Authentication | Microsoft Entra — handled automatically by the host |
| HTTPS | Enforced by the platform |
| Scaling | Managed by Microsoft |
| Availability | Follows Power Platform SLAs |
| CDN / static assets | Served by Power Platform infrastructure |
| Mobile / Windows app | **Not supported** — desktop browser only |

## ALM Strategy

### Environment topology

Define the environment chain before any code is written:

```
Developer (local) → Dev environment → Test environment → Prod environment
```

Each environment is a separate Power Platform environment. Connections are managed
per-environment via **connection references** — do not hardcode connection IDs.

### ALM decisions checklist

- [ ] Environments provisioned (Dev, Test, Prod)
- [ ] Solution created (code app must be solution-aware for ALM)
- [ ] Connection references defined per connector (not raw connection IDs)
- [ ] Source control repository set up
- [ ] Branching strategy agreed (recommend: feature branches → main via PR)
- [ ] CI/CD pipeline planned (GitHub Actions or Azure Pipelines)
- [ ] `pac solution export` as part of release process

### Connection references

Connection references are solution components that abstract a connection from a
specific user's credential. They are the key to making the app portable across
environments.

```bash
# Discover connection references in your solution
pac code list-connection-references -env <environmentURL> -s <solutionID>

# Bind a data source to a connection reference
pac code add-data-source -a <apiName> -cr <connectionReferenceLogicalName> -s <solutionID>
```

See `agents/skills/pac-reference.md` for full command reference.

## Security Posture

### Authentication & authorisation

- Authentication is handled entirely by the Power Apps host (Microsoft Entra)
- No auth code to write — the host manages tokens and consent dialogs
- Authorisation is enforced at the connector level (Dataverse security roles, SQL permissions)
- Azure B2B (guest users) is supported

### Security checklist

- [ ] No API keys, secrets, or passwords in source code
- [ ] Auth delegated entirely to Power Apps SDK — no custom token handling
- [ ] User permissions enforced server-side at the connector/data source level
- [ ] DLP policies verified — all connectors are in compliant groups
- [ ] Input validation on all form fields (`dangerouslySetInnerHTML` avoided)
- [ ] HTTPS enforced by platform — no additional configuration needed
- [ ] Conditional Access policies reviewed with the Power Platform admin

### DLP compliance

Before finalising the architecture:

1. List DLP policies on the target environment (Admin Center → Data policies)
2. Check each planned connector's group (Business / Non-Business / Blocked)
3. Business group connectors cannot share data with Non-Business group connectors
4. If there is a conflict: request a DLP exception, split into two apps, or route through Power Automate

## Architecture Document Template

```markdown
# Solution Architecture: [App Name]

## Component Diagram
[Mermaid diagram]

## Hosting
- Platform: Power Apps code app (managed hosting)
- Authentication: Microsoft Entra (handled by host)
- Supported clients: Desktop browser only

## ALM
- Environments: Dev / Test / Prod
- Solution name: [name]
- Connection references: [list]
- Source control: [GitHub / Azure DevOps repo URL]
- Branching: feature → main via PR
- CI/CD: [GitHub Actions / Azure Pipelines / manual]

## Security
- Auth: Microsoft Entra (no custom auth)
- Authorisation: [Dataverse security roles / SQL permissions / etc.]
- DLP: [policy name, compliance status]
- Guest users: [Yes / No]

## Risks
| Risk | Mitigation |
|------|-----------|
| | |
```

## GitHub Copilot Prompt

```
@workspace Based on this architecture document: [paste architecture].
Generate a ARCHITECTURE.md file for the repository root summarising:
- Component diagram (Mermaid)
- Hosting model
- Security posture
- ALM strategy
- Key architectural decisions and their rationale
```

## Transition to Phase 3

With the architecture agreed, read `agents/skills/data-structure/SKILL.md` and
design the data layer.
