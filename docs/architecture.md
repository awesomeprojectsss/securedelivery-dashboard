# SecureDelivery Dashboard Architecture

## 1. Purpose

The SecureDelivery Dashboard is the web interface for platform management, SmartBox monitoring and customer support.

The dashboard uses Next.js with TypeScript.

This document defines the architecture and responsibilities of the Next.js application while leaving lower-level frontend choices open where the team has not yet made a decision.

---

## 2. User Roles

The dashboard serves:

```text
SUPER_ADMIN
ADMIN
CUSTOMER
```

The backend is authoritative for authorization.

The dashboard adapts navigation and available actions to the current user but does not replace backend security.

---

## 3. Technology Baseline

The dashboard MVP uses:

- Next.js
- TypeScript

Next.js is the accepted frontend framework.

TypeScript is required for application code.

The following choices remain open until explicitly decided:

- Next.js routing conventions / application structure
- rendering strategy
- state management
- server-state/data-fetching approach
- WebSocket client integration
- component library and design system implementation
- authentication token/session handling
- testing libraries
- deployment strategy

These decisions should be recorded through ADRs when they materially shape the application architecture.

---

## Engineering Practice Boundary

This architecture document defines SecureDelivery-specific boundaries and decisions.

Framework-level implementation guidance is defined in the repository `AGENTS.md`.

Human developers should also follow:

- `docs/development-guide.pt-BR.md`
- `docs/git-workflow.pt-BR.md`

Implementations should remain idiomatic to Next.js and TypeScript and should prefer official/framework-native solutions over unnecessary custom abstractions.


## 4. Main Functional Areas

Potential feature areas include:

```text
Authentication
Users
Roles
Customers
Devices
DeviceActivation
Monitoring
Deliveries
Events
Support
Realtime
Shared UI
```

Exact feature names and folder conventions should follow the established Next.js project structure.

---

## 5. High-Level Architecture

A generic frontend architecture should preserve this direction:

```text
Pages / Screens
      ↓
Feature Components
      ↓
Application State / Use Cases
      ↓
API Client / Realtime Client
      ↓
SecureDelivery Server
```

Do not put backend business rules directly into presentation components.

---

## 6. Source of Truth

The SecureDelivery Server is the source of truth.

The dashboard may cache and render state, but authoritative business data comes from the backend.

Examples:

- roles
- Device status (presented as SmartBox status)
- Customer ownership
- activation state
- ticket ownership
- event records
- health status

---

## Device / SmartBox Terminology

The Dashboard consumes technical `Device` resources.

The product presents those resources as `SmartBox`.

```text
Technical contract          Product UI
Device                      SmartBox
Devices                     SmartBoxes
deviceId                    internal/technical identifier
/api/v1/devices             SmartBox screens
```

Do not alter shared API terminology for presentation convenience.

The frontend may use presentation adapters/view models where needed.

## 7. RBAC Presentation

### SUPER_ADMIN

UI capabilities include:

- create Super Administrators
- create Administrators
- create Customers
- change roles of Administrators and Customers
- reset passwords of Administrators and Customers
- activate/deactivate users where allowed
- access all Administrator functionality

Restrictions should be reflected in the UI, but backend policy is authoritative.

### ADMIN

UI capabilities include:

- manage Customers
- manage Administrators
- manage SmartBoxes
- monitor SmartBoxes by Customer
- monitor all SmartBoxes globally
- inspect operational status
- manage support tickets
- assume support conversations

### CUSTOMER

UI capabilities include:

- monitor owned SmartBoxes
- request SmartBoxes
- activate/validate SmartBoxes
- inspect events
- inspect latest location
- open Google Maps external links
- open support tickets
- use realtime support chat

---

## 8. SmartBox Views

### Customer-Scoped Administrator View

```text
Selected Customer
      ↓
SmartBox List
      ↓
SmartBox Detail
```

SmartBox detail may display:

- status
- battery
- connectivity
- health
- last communication
- last location
- events

### Global Administrator View

Administrators can inspect operational state across all Customers.

The view is intended for platform support and operations.

### Customer View

Customers can inspect only SmartBoxes owned by their tenant.

---

## 9. SmartBox Activation

The dashboard participates in the Customer activation flow after the Customer scans the QR Code.

Possible UI concerns include:

- activation token validation state
- activation confirmation
- invalid token
- expired token
- already activated
- successful association

The exact routing/deep-link implementation is not yet defined.

---

## 10. Monitoring

Monitoring screens should prioritize operational clarity.

Important states include:

- active
- inactive
- offline
- low battery
- degraded health
- recent critical event

Avoid overwhelming users with raw sensor data.

Prefer product-level information.

---

## 11. Location

The MVP does not implement route visualization.

Location UI should focus on:

- latest known location
- event location
- external Google Maps link

Conceptually:

```text
latitude + longitude
        ↓
External Google Maps URL
```

Do not introduce full route reconstruction without a new product/architecture decision.

---

## Speed and Distance KPIs

The Dashboard may expose operational navigation KPIs derived by the Server from compact Device summaries.

MVP candidates:

- average moving speed;
- maximum speed;
- monitored distance;
- moving time;
- stopped time;
- events per 100 km;
- events grouped by speed range.

Contract values use canonical SI units.

Presentation may convert:

```text
m/s -> km/h
m   -> km
```

The Dashboard must not calculate average speed as a simple average of one-minute averages.

Event views may display speed-at-event and nearby speed context.

Correlation wording must remain neutral and must not claim speed caused an event without a later validated causal-analysis capability.

Full route tracking remains outside the MVP.

## 12. Events

Event views should expose:

- event type
- occurrence timestamp
- associated SmartBox
- associated delivery where relevant
- event location
- available audit evidence or derived summary

The exact depth of raw evidence displayed to Customers versus Administrators may evolve.

---

## Extensible Device Data

The Dashboard must remain forward-compatible with unknown valid Device observations and events.

Known event types may have specialized presentation.

Unknown event types use a generic fallback renderer.

The Dashboard must never assume that the event-type catalog is a compile-time closed enum.

The canonical payload definitions live under `../../docs/contracts/`.

## 13. Realtime

Realtime updates may use WebSocket.

Potential realtime use cases:

- SmartBox connectivity
- SmartBox health
- battery changes
- new events
- ticket messages
- ticket status

WebSocket messages are not authoritative persistence.

On reconnect, the frontend should refresh data where needed.

---

## 14. Support Tickets

Ticket UI should include:

- ticket list
- ticket status
- ticket detail
- message history
- realtime conversation
- assignment state
- Administrator takeover
- close/resolve actions

Persistent server history remains authoritative.

---

## 15. API Client Boundary

Network communication should be isolated from presentation components.

The final mechanism may use:

- services
- repositories
- hooks
- query clients
- generated clients

depending on the selected frontend stack.

The key architectural rule is separation of concerns.

---

## 16. State Management

State-management technology is undecided.

The selected approach should distinguish:

- server state
- local UI state
- realtime updates
- authentication state

Do not introduce heavyweight global state without a demonstrated need.

---

## 17. Error Handling

Important remote views need:

- loading
- error
- empty
- disconnected
- retry/recovery states

The dashboard should not silently hide failures.

---

## 18. Security

The dashboard is not a security boundary.

Backend authorization remains mandatory.

The frontend should:

- avoid exposing restricted navigation
- avoid storing secrets in source
- protect authentication/session material according to the chosen Next.js authentication strategy
- avoid exposing unauthorized location data

---

## 19. Testing Strategy

Important test targets include:

- RBAC-based rendering
- critical management flows
- SmartBox activation
- monitoring states
- realtime reconciliation
- ticket assignment
- support chat behavior
- API error states

---

## 20. Open Architectural Decisions

Still to be defined:

- Next.js routing/application conventions
- state management
- data-fetching library
- WebSocket client strategy
- component library/design system
- authentication token strategy
- testing libraries / test strategy
- deployment strategy

## Shared Integration References

Cross-repository behavior is defined in:

- `../../docs/contracts/domain-model.md`
- `../../docs/contracts/integration-flows.md`
- `../../docs/contracts/kpis.md`
- `../../docs/contracts/openapi.yaml`
- `../../docs/contracts/asyncapi.yaml`
