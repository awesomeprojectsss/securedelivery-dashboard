# SecureDelivery Dashboard Architecture

## 1. Purpose

The SecureDelivery Dashboard is the web interface for platform management, SmartBox monitoring and customer support.

The frontend technology stack has not yet been selected.

This document defines responsibilities and boundaries without prematurely choosing a framework.

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

## 3. Main Functional Areas

Potential feature areas include:

```text
Authentication
Users
Roles
Customers
SmartBoxes
SmartBoxActivation
Monitoring
Deliveries
Events
Support
Realtime
Shared UI
```

Exact feature names depend on the selected frontend framework.

---

## 4. High-Level Architecture

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

## 5. Source of Truth

The SecureDelivery Server is the source of truth.

The dashboard may cache and render state, but authoritative business data comes from the backend.

Examples:

- roles
- SmartBox status
- Customer ownership
- activation state
- ticket ownership
- event records
- health status

---

## 6. RBAC Presentation

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

## 7. SmartBox Views

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

## 8. SmartBox Activation

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

## 9. Monitoring

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

## 10. Location

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

## 11. Events

Event views should expose:

- event type
- occurrence timestamp
- associated SmartBox
- associated delivery where relevant
- event location
- available audit evidence or derived summary

The exact depth of raw evidence displayed to Customers versus Administrators may evolve.

---

## 12. Realtime

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

## 13. Support Tickets

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

## 14. API Client Boundary

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

## 15. State Management

State-management technology is undecided.

The selected approach should distinguish:

- server state
- local UI state
- realtime updates
- authentication state

Do not introduce heavyweight global state without a demonstrated need.

---

## 16. Error Handling

Important remote views need:

- loading
- error
- empty
- disconnected
- retry/recovery states

The dashboard should not silently hide failures.

---

## 17. Security

The dashboard is not a security boundary.

Backend authorization remains mandatory.

The frontend should:

- avoid exposing restricted navigation
- avoid storing secrets in source
- protect authentication material according to the chosen framework
- avoid exposing unauthorized location data

---

## 18. Testing Strategy

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

## 19. Open Architectural Decisions

Still to be defined:

- frontend framework
- routing
- state management
- data-fetching library
- WebSocket client strategy
- component library/design system
- authentication token strategy
- testing framework
- deployment strategy
