# AGENTS.md — SecureDelivery Dashboard

## Purpose of This File

This file defines how AI coding agents must work inside the SecureDelivery Dashboard repository.

The dashboard technology baseline is:

- Next.js
- TypeScript

Next.js is an accepted architectural decision for the MVP.

Agents must not replace Next.js with another frontend framework unless explicitly requested and the architectural decision is updated through an ADR.

Read:

- `../docs/project.md`
- `docs/architecture.md`

before making architectural changes.

---

## Required Project Context

The SecureDelivery Dashboard is the operational and management interface for:

- Super Administrators
- Administrators
- Customers

The server is the source of truth.

The dashboard must not recreate authoritative business rules that belong to the backend.

The MVP monitors delivery quality, SmartBox health and event evidence.

It does not implement full route tracking.

---

## Technology Baseline

The dashboard MVP uses:

- Next.js
- TypeScript

The exact choices for routing conventions, rendering strategy, state management, data fetching, WebSocket integration, component library/design system, testing and deployment remain architectural decisions unless already established in the repository.

Do not introduce alternate frontend frameworks.

---

## Shared Contract Policy

Before implementing or changing communication between repositories, read:

- `../docs/contracts/README.md`
- `../docs/contracts/openapi.yaml`
- `../docs/contracts/asyncapi.yaml` when realtime is involved
- relevant contract documentation under `../docs/contracts/`
- shared ADRs under `../docs/decisions/`

The contracts under `../docs/contracts/` are authoritative.

Do not invent, duplicate or silently modify cross-repository payloads.

When changing a shared contract:

1. update the canonical contract first;
2. evaluate backward compatibility;
3. update the server implementation;
4. regenerate/update typed clients when generation is configured;
5. update affected consumers;
6. update tests;
7. update architecture documentation and ADRs when required.

`Device` is the canonical technical term.

Do not use `SmartBox` in API paths, backend DTOs, persistence entities or cross-repository contract schemas.

`SmartBox` is a product-facing UI label only.

## Agent Workflow

Before implementing changes:

1. Read this `AGENTS.md`.
2. Read `docs/architecture.md`.
3. Inspect the existing Next.js project structure and repository conventions.
4. Follow established Next.js and TypeScript conventions already present in the repository.
5. Do not replace Next.js or introduce a second application framework without an explicit architectural decision.
6. Keep business authorization rules on the server.
7. Reuse existing components and patterns.
8. Avoid unrelated refactors.
9. Add or update meaningful tests.
10. Consider loading, error, empty and disconnected states.
11. Update architecture documentation for architectural changes.
12. Create/update ADRs for meaningful architectural decisions.

---

## Architectural Decision Records

Document meaningful decisions under:

```text
docs/decisions/
```

Create or update ADRs when changes affect:

- replacement or major change of the Next.js framework baseline
- state management
- API client strategy
- WebSocket strategy
- authentication state
- authorization rendering strategy
- routing
- design system
- data-fetching architecture
- realtime chat architecture

Do not create ADRs for routine component changes or minor styling.

Do not silently override accepted architecture.

---

## RBAC Context

Main roles:

```text
SUPER_ADMIN
ADMIN
CUSTOMER
```

The dashboard should present only actions available to the current user.

However:

> UI visibility is not authorization.

The backend must enforce all permissions.

Do not implement security by only hiding buttons.

---

## SUPER_ADMIN UI

Super Administrators should be able to access:

- Super Administrator creation
- Administrator creation
- Customer creation
- role changes for Administrators and Customers
- password reset for Administrators and Customers
- user activation/deactivation where allowed
- all Administrator capabilities

The UI must respect restrictions such as:

- cannot reset another Super Administrator password
- cannot deactivate another Super Administrator
- cannot deactivate self

The backend remains authoritative if UI state and server policy disagree.

---

## ADMIN UI

Administrators should be able to manage:

- Customers
- Administrators
- SmartBoxes
- SmartBox operational status
- Customer-scoped SmartBox views
- global SmartBox views
- support tickets

Administrator operational monitoring includes:

- latest GPS location
- connectivity
- health
- battery
- last communication
- recent events

---

## CUSTOMER UI

Customers should be able to:

- view their SmartBoxes
- request a SmartBox
- activate/validate SmartBoxes
- monitor SmartBoxes
- view latest location
- open latest location in Google Maps
- view battery
- view connectivity
- view health
- inspect delivery events
- open support tickets
- chat with support

Do not expose cross-customer data.

---

## SmartBox UI

`Device` is the canonical technical entity returned by the API.

`SmartBox` is the product-facing label.

Examples:

```text
API:       GET /api/v1/devices
Type:      Device
UI label:  SmartBox
```

Do not create `/smartboxes` client calls to the backend.

Do not rename contract fields from `deviceId` to `smartBoxId`.

The Dashboard may map technical Device data into product-facing SmartBox presentation models when useful, but the shared API contract must remain intact.

## Location

The MVP does not show complete routes.

The UI may show:

- latest known coordinates
- event location
- external Google Maps link

Do not implement route reconstruction or embedded route history unless explicitly requested.

---

## Speed and Navigation Presentation

The MVP may present speed/distance KPIs derived from compact Device telemetry.

Relevant metrics include:

- average moving speed;
- maximum speed;
- monitored distance;
- moving/stopped duration;
- events per 100 km;
- event distribution by speed range.

The API uses SI units.

Convert speed from m/s to km/h only at presentation boundaries.

Do not compute average speed by averaging one-minute averages.

Use server-provided/derived aggregates based on distance and moving duration.

When presenting event correlations with speed, use neutral language such as "associated with" or "occurred at".

Do not claim that speed caused an event unless the product later implements a validated causal analysis method.

## Realtime

WebSocket may support:

- SmartBox status changes
- event updates
- ticket updates
- support chat

The dashboard must tolerate disconnections.

Do not assume WebSocket delivery is guaranteed.

After reconnecting, refresh authoritative state when needed.

---

## Support Tickets

Support screens should support:

- ticket list
- ticket detail
- ticket status
- assignment
- Administrator takeover
- persisted message history
- realtime messages

Realtime UI should be reconciled with server-persisted state.

---

## Extensible Measurements and Events

The Dashboard must tolerate data introduced by newer Device versions.

Unknown `eventType` values must not crash rendering.

When no specialized renderer exists, display a generic event view containing:

- eventType
- severity
- timestamp
- SmartBox display name
- location when available
- generic attributes

Unknown observations may be displayed through a generic key/value/unit renderer in technical/audit views.

Specialized UI for known events is allowed, but must not make the shared contract closed.

## API Boundaries

Do not call APIs directly from arbitrary presentation components when the project establishes dedicated clients/services/hooks.

Keep network concerns separate from UI concerns.

Prefer typed contracts.

Do not manually duplicate backend types when a shared or generated contract strategy exists.

---

## Error and Loading States

Every remote view should consider:

- loading
- error
- empty state
- stale state
- disconnected state

Do not silently swallow API errors.

Do not expose internal stack traces to users.

---

## Security

Do not store secrets in source code.

Handle authentication tokens according to the selected frontend architecture.

Do not expose sensitive location data to unauthorized users.

Do not log sensitive authentication information.

---

## Next.js and TypeScript

Use Next.js as the web application framework.

Use TypeScript for application code.

Prefer strict typing.

Avoid:

- `any`
- unsafe casts
- duplicated DTOs
- duplicated enums
- magic strings
- business logic embedded in presentation components
- unnecessary client-side state when server-backed state already exists

Do not replace Next.js or TypeScript by assumption.

Follow the routing, rendering and data-fetching conventions established by the repository and documented architecture.

---

## Testing

Prioritize tests for:

- role-based rendering
- SmartBox monitoring states
- API transformations
- realtime event handling
- ticket chat
- reconnect behavior
- critical user flows

Do not create superficial tests only for coverage.

---

## Development Standards

Use:

- Git
- GitHub
- Pull Requests
- Code Review
- Conventional Commits
- automated tests
- linting
- formatting

Use the team-approved branching strategy.

---

## Conventional Commits Examples

```text
feat(smartboxes): add customer smartbox monitoring
feat(support): add realtime ticket chat
feat(users): add admin role management
fix(rbac): hide forbidden self-deactivation action
fix(realtime): refresh state after reconnect
test(support): cover ticket assignment flow
docs: document dashboard authorization boundaries
```

---

## Technology Best Practices

Follow official Next.js, React and TypeScript conventions and established ecosystem best practices.

Prefer idiomatic Next.js solutions rather than building the dashboard as a traditional client-only React SPA by default.

Before introducing a custom abstraction, verify whether Next.js or the selected application libraries already provide an established solution.

Do not move business authority from the NestJS backend into Next.js.

---

## Next.js Engineering Guidelines

- Prefer Server Components by default when using the App Router.
- Use Client Components only when browser APIs, local interactive state, effects or client-only libraries require them.
- Keep `"use client"` boundaries as small as practical.
- Do not add `"use client"` at page or layout level without a concrete reason.
- Keep SecureDelivery business rules authoritative in the NestJS API.
- Do not duplicate backend RBAC or tenant authorization as authoritative frontend logic.
- Keep data-access logic separate from presentation components.
- Prefer reusable feature-level components over large page components.
- Avoid unnecessary global state.
- Distinguish server state from local UI state.
- Avoid copying server state into global client stores without a real need.
- Never expose secrets through `NEXT_PUBLIC_*`.
- Validate untrusted/external API data at application boundaries when appropriate.
- Handle loading, empty, error, stale and disconnected states explicitly.
- Use Next.js routing, rendering and caching behavior intentionally.
- Avoid accidental request waterfalls and duplicated requests.
- Prefer server-side data access where it improves security, performance or simplicity, while respecting the NestJS API boundary.
- Keep route-level authorization UX separate from actual backend authorization.
- Use accessible semantic HTML and keyboard-friendly interactions.
- Optimize images, fonts and bundles using framework-native features where appropriate.
- Avoid premature memoization or performance abstractions without evidence.

---

## Realtime UI Guidelines

- Treat WebSocket events as realtime signals, not persistent truth.
- Do not make application correctness depend on receiving every WebSocket event.
- On reconnect, reconcile relevant state with the authoritative API.
- Avoid duplicating the same realtime subscription across unrelated components.
- Keep subscription ownership explicit.
- Clean up client subscriptions correctly.
- Prevent stale realtime updates from overwriting newer authoritative state.
- Keep realtime event payloads typed.

---

## RBAC UI Guidelines

- Render navigation and actions according to the current user's role.
- Never treat hidden controls as authorization.
- Keep `SUPER_ADMIN`, `ADMIN` and `CUSTOMER` experiences intentionally distinct.
- Avoid generic "one dashboard with hidden buttons" architecture where role-specific information architecture is required.
- The NestJS backend remains authoritative for every protected operation.

---

## TypeScript Guidelines

- Enable and preserve strict typing.
- Avoid `any`.
- Avoid unsafe casts used only to silence compiler errors.
- Prefer discriminated unions for explicit state where useful.
- Keep API contracts typed.
- Avoid duplicating DTO definitions when a generated/shared contract strategy becomes available.
- Represent loading/error/data states explicitly rather than with ambiguous nullable objects.

---

## Testing Strategy

Prefer:

- unit tests for transformations and isolated application logic;
- component tests for critical role-specific UI and operational states;
- integration tests for API/realtime orchestration;
- e2e tests for RBAC-sensitive and business-critical user journeys.

High-value tests include:

- role-specific navigation;
- Super Admin restrictions;
- Customer tenant-scoped screens;
- SmartBox operational states;
- WebSocket reconnect reconciliation;
- ticket assignment and realtime chat behavior;
- loading/error/empty states.

---

## Human Developer Documentation

Human developers should also read:

- `docs/development-guide.pt-BR.md`
- `docs/git-workflow.pt-BR.md`

These files define practical development conventions and the Git/GitHub workflow for the repository.

## Out of Scope for MVP

Do not implement unless explicitly requested:

- temperature monitoring
- full route tracking
- route reconstruction
- embedded Google Maps route UI
- Iridium
- satellite features
- iFood
- Rappi
- ERP integrations
- machine learning
- advanced fleet management
