# ADR-001: Next.js as the Dashboard Framework

## Status

Accepted

## Context

SecureDelivery requires a web dashboard for Super Administrators, Administrators and Customers.

The dashboard must support RBAC-aware navigation, SmartBox monitoring, events, user/customer management, support tickets, realtime updates and profile/settings flows.

A frontend framework decision is required so implementation can begin with a stable technology baseline.

## Decision

Use **Next.js** as the SecureDelivery dashboard framework.

Use **TypeScript** for application code.

The backend remains the authoritative source of business state and authorization.

The dashboard must not duplicate server business rules simply because Next.js provides server-side capabilities.

## Consequences

- Dashboard application code is written in TypeScript.
- Next.js becomes the baseline web framework.
- Routing/application conventions, rendering strategy, state management, data fetching, WebSocket integration, UI component strategy, authentication/session handling and deployment remain explicit architectural decisions.
- Replacing Next.js requires a new ADR that supersedes this decision.
