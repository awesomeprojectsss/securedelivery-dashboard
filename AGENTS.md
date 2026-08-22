# AGENTS.md — SecureDelivery Dashboard

## Purpose of This File

This file defines how AI coding agents must work inside the SecureDelivery Dashboard repository.

The frontend technology stack has not been finalized.

Agents must not select or introduce a major frontend framework unless explicitly requested.

Read:

- the shared SecureDelivery `project.md`
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

## Agent Workflow

Before implementing changes:

1. Read this `AGENTS.md`.
2. Read `docs/architecture.md`.
3. Inspect the existing framework and project conventions.
4. Do not choose a frontend framework unless explicitly requested.
5. Keep business authorization rules on the server.
6. Reuse existing components and patterns.
7. Avoid unrelated refactors.
8. Add or update meaningful tests.
9. Consider loading, error, empty and disconnected states.
10. Update architecture documentation for architectural changes.
11. Create/update ADRs for meaningful architectural decisions.

---

## Architectural Decision Records

Document meaningful decisions under:

```text
docs/decisions/
```

Create or update ADRs when changes affect:

- frontend framework
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

`SmartBox` is a logical product entity.

Do not label it as "phone" in core product flows just because the MVP device is a smartphone.

The dashboard should remain compatible with future dedicated IoT hardware.

---

## Location

The MVP does not show complete routes.

The UI may show:

- latest known coordinates
- event location
- external Google Maps link

Do not implement route reconstruction or embedded route history unless explicitly requested.

---

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

## TypeScript

Use TypeScript if the selected frontend framework supports it and the team confirms that decision.

When TypeScript is used:

- prefer strict typing
- avoid `any`
- avoid unsafe casts
- avoid duplicated DTOs
- avoid magic strings

Do not choose a framework by assumption.

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
