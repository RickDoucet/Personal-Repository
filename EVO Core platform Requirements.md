# IntelligenEVO Platform Spine — Functional and Non-Functional Requirements

## Executive Summary

The IntelligenEVO spine is the shared platform foundation that all EVO microservices (DMS, CCS, GSM, FIN, UMS, RTDP, placcnt, and others) build on. It provides the message and event fabric, service-to-service communication and identity, multi-tenant isolation, configuration, persistence access patterns, audit and logging, real-time push, and observability. These are cross-cutting capabilities: feature services consume them rather than reimplementing them. The requirements are written so a service team can tell whether a spine capability is present and correct.

## Scope and Assumptions

- In scope: messaging/eventing backbone, real-time data push, inter-service communication and API edge, identity and authorization integration, multi-tenancy, configuration, shared persistence and data-access concerns, logging/audit/traceability, service hosting and discovery, and observability.
- Out of scope: business logic owned by individual feature services (device commands, financial reconciliation, player accounts, etc.). The spine carries and secures those flows but does not define them.
- Assumption (A1): message transport is RabbitMQ with Protobuf payloads for device-facing and inter-service events, consistent with CCS and DMS in the glossary.
- Assumption (A2): real-time client push is RTDP over WebSocket (STOMP/SockJS), consistent with the GUI and SIT.
- Assumption (A3): identity is issued by UMS as JWT, including service-to-service (S2S) tokens, consistent with the glossary.
- Assumption (A4): services are Spring Boot microservices deployed on a cloud-native, container-based runtime.

## Functional Requirements

### FR-1 Messaging and Eventing Backbone

- FR-1.1: The spine shall provide a message broker (A1: RabbitMQ) that lets services publish and subscribe to events and commands without point-to-point coupling.
- FR-1.2: The spine shall support a Protobuf-based message contract for device-facing and inter-service payloads, with versioned schemas.
- FR-1.3: The spine shall support both command routing (directed, e.g. CCS to EGM/VSC) and event broadcast (fan-out, e.g. DMS status changes to SIT and RTDP).
- FR-1.4: The spine shall guarantee at-least-once delivery for durable messages and expose acknowledgement semantics to consumers.
- FR-1.5: The spine shall provide dead-letter handling for messages that cannot be delivered or processed after a configurable retry policy.
- FR-1.6: The spine shall preserve message ordering within a partition or routing key where a service declares ordering is required.
- FR-1.7: The spine shall support schema evolution so a new producer version does not break existing consumers (backward-compatible field additions).

### FR-2 Real-Time Data Push (RTDP)

- FR-2.1: The spine shall push real-time updates to subscribed clients (A2: WebSocket via STOMP/SockJS) for monitoring, situations, and live operational data.
- FR-2.2: The spine shall enforce authorization on each subscription so a client only receives data it is permitted to see, based on its UMS token and tenant/domain scope.
- FR-2.3: The spine shall support topic or channel-based subscriptions scoped to tenant, venue, and entity.
- FR-2.4: The spine shall handle client reconnection and resubscription without duplicating or silently dropping updates.
- FR-2.5: The spine shall degrade gracefully under load by applying backpressure or coalescing rather than failing the push channel.

### FR-3 Inter-Service Communication and API Edge

- FR-3.1: The spine shall expose a consistent synchronous API style (REST over HTTPS) for request/response interactions between services and from the GUI.
- FR-3.2: The spine shall provide an edge or gateway layer that terminates client traffic, routes to services, and enforces authentication before requests reach a service.
- FR-3.3: The spine shall propagate caller identity (A3: UMS JWT, including S2S token) on every inter-service call so the receiving service can authorize the request.
- FR-3.4: The spine shall support data aggregation for client consumption (consistent with EIS) so the GUI can request composed shapes without N calls, with authorization enforced on the aggregate.
- FR-3.5: The spine shall provide a standard error contract (status, code, correlation id) across services.

### FR-4 Identity, Authentication, and Authorization

- FR-4.1: The spine shall integrate with UMS for user authentication and issue/validate JWTs for authenticated sessions.
- FR-4.2: The spine shall support service-to-service authentication using S2S tokens that services cache and auto-refresh.
- FR-4.3: The spine shall enforce role-based access control (RBAC) and domain scoping on protected resources, consistent with UMS domains.
- FR-4.4: The spine shall reject expired, malformed, or unauthorized tokens and surface a consistent authorization error.
- FR-4.5: The spine shall make the authenticated principal, roles, and tenant/domain available to downstream services and to RTDP subscription filtering.

### FR-5 Multi-Tenancy and Venue Isolation

- FR-5.1: The spine shall carry tenant and venue context on every message, event, and API call.
- FR-5.2: The spine shall isolate data and events so one tenant or venue cannot access another's data through the shared backbone.
- FR-5.3: The spine shall support cross-venue operations only where an ownership or cross-validation relationship explicitly permits it.
- FR-5.4: The spine shall allow per-tenant configuration of platform behaviors (for example retention, feature flags) without code changes.

### FR-6 Configuration Management

- FR-6.1: The spine shall provide centralized, environment-aware configuration for services (per environment, per tenant where applicable).
- FR-6.2: The spine shall support configuration changes without requiring a full redeploy of consuming services where the setting is runtime-refreshable.
- FR-6.3: The spine shall validate configuration values against expected types and ranges and reject invalid configuration at startup.
- FR-6.4: The spine shall record who changed a configuration value and when, for audit.

### FR-7 Persistence and Data Access

- FR-7.1: The spine shall provide standard patterns for service data persistence (per-service datastore ownership, no cross-service direct table access).
- FR-7.2: The spine shall support delivery of platform data to the IntelligenEVO Data Lake for reporting and analytics consumers.
- FR-7.3: The spine shall provide a consistent approach to entity versioning and soft-delete flags consistent with existing EVO entities (for example venue and EGM revision and deleted flags).
- FR-7.4: The spine shall expose caching patterns for read-heavy aggregates (consistent with EIS caching) with explicit invalidation.

### FR-8 Logging, Audit, and Traceability

- FR-8.1: The spine shall assign a correlation id to each request and propagate it across all messages, service calls, and log entries for that flow.
- FR-8.2: The spine shall provide structured, centralized logging for all services.
- FR-8.3: The spine shall capture an immutable audit trail for security-relevant and regulated actions (authentication, authorization changes, configuration changes, command dispatch).
- FR-8.4: The spine shall support audit record retention and read-only access for regulators and auditors.

### FR-9 Service Hosting, Discovery, and Lifecycle

- FR-9.1: The spine shall provide shared hosting for common platform capabilities (consistent with GHS) that dependent services rely on.
- FR-9.2: The spine shall provide service discovery so services locate dependencies without hard-coded endpoints.
- FR-9.3: The spine shall expose health and readiness endpoints for every service so the runtime can manage lifecycle (start, restart, drain).
- FR-9.4: The spine shall support rolling deployment and rollback of services without a full-platform outage.

### FR-10 Observability and Health Monitoring

- FR-10.1: The spine shall collect metrics (throughput, latency, error rate, queue depth) for messaging, APIs, and RTDP.
- FR-10.2: The spine shall surface service availability so administrators can detect and respond to failures (consistent with GHS reporting availability).
- FR-10.3: The spine shall support distributed tracing across service hops using the propagated correlation id.
- FR-10.4: The spine shall raise alerts when a platform health threshold is breached (for example broker unavailable, dead-letter growth, token service failure).

## Non-Functional Requirements

### NFR-1 Performance and Scalability

- NFR-1.1: The spine shall scale horizontally so additional service instances increase capacity without re-architecture.
- NFR-1.2: The messaging backbone shall sustain the peak device event volume for the largest supported venue footprint without message loss. Target: define per deployment tier (open: confirm peak events/second).
- NFR-1.3: RTDP shall deliver real-time updates to subscribed clients with low latency under normal load. Target: p95 update delivery within 2 seconds (open: confirm SLA).
- NFR-1.4: Synchronous inter-service calls shall meet a documented latency budget per call tier (open: confirm p95 targets).

### NFR-2 Availability and Resilience

- NFR-2.1: The spine shall have no single point of failure in the message broker, gateway, identity integration, or RTDP layers.
- NFR-2.2: The spine shall meet a defined platform availability target. Target: 99.9 percent (open: confirm against customer SLAs).
- NFR-2.3: The spine shall tolerate the loss of a single service instance or broker node without data loss and with automatic recovery.
- NFR-2.4: The spine shall apply retry with backoff and circuit breaking so a failing dependency does not cascade across services.

### NFR-3 Security

- NFR-3.1: All spine traffic shall be encrypted in transit (TLS) for both client-facing and inter-service communication.
- NFR-3.2: Sensitive data at rest shall be encrypted, including audit and player-related data.
- NFR-3.3: The spine shall enforce least-privilege authorization on every protected resource and reject unauthenticated or unauthorized access by default.
- NFR-3.4: The spine shall protect against the OWASP Top 10 at the API edge (injection, broken auth, broken access control, and related).
- NFR-3.5: Secrets (tokens, keys, credentials) shall be managed through a secrets store and never logged.

### NFR-4 Compliance and Regulatory

- NFR-4.1: The spine shall preserve a complete, tamper-evident audit trail sufficient for gaming regulatory review (for example GLI and jurisdictional requirements).
- NFR-4.2: The spine shall support configurable data retention and purge aligned with jurisdictional rules and existing EVO retention behavior (for example SLD retention).
- NFR-4.3: The spine shall enforce tenant and jurisdiction data isolation so regulated data does not cross jurisdictional boundaries improperly.

Note: validate specific regulatory obligations through the Regulatory Risk Reviewer agent before finalizing NFR-4.

### NFR-5 Maintainability and Deployability

- NFR-5.1: The spine shall support independent build, test, and deploy of each service (A4: containerized, cloud-native).
- NFR-5.2: Message and API contracts shall be versioned so producers and consumers can evolve independently.
- NFR-5.3: The spine shall provide consistent, documented patterns (logging, config, auth, messaging) so new services onboard without inventing their own.

### NFR-6 Observability

- NFR-6.1: Every flow shall be traceable end to end via a single correlation id across logs, metrics, and traces.
- NFR-6.2: Platform health metrics and alerts shall be available to operators in near real time.

### NFR-7 Data Integrity and Consistency

- NFR-7.1: The spine shall guarantee that acknowledged durable messages are not lost across broker restarts.
- NFR-7.2: The spine shall provide idempotency support so a redelivered message does not cause duplicate side effects.
- NFR-7.3: Entity versioning and soft-delete semantics shall be consistent across services consuming spine data patterns.

### NFR-8 Interoperability

- NFR-8.1: The spine shall support integration with third-party systems (retailer management, financial, regulatory reporting) through standard import/export patterns consistent with the EVO integration service.
- NFR-8.2: The spine shall expose stable, documented contracts for the GUI and external consumers.

### NFR-9 Localization

- NFR-9.1: The spine shall carry locale and timezone context so downstream services and reports render correctly per venue (consistent with existing EVO venue timezone handling).

## Open Questions

1. Confirm the canonical definition of "spine" with architecture, so the artifact name matches internal usage.
2. Confirm concrete performance targets: peak events/second per tier, RTDP latency SLA, inter-service latency budgets.
3. Confirm the platform availability SLA against customer contracts.
4. Confirm regulatory retention and isolation obligations via the Regulatory Risk Reviewer.
5. Confirm whether configuration and secrets use a specific platform (for example Spring Cloud Config, Vault) to make FR-6 and NFR-3.5 concrete.

## Traceability

- Service and messaging facts: Business Glossary, EVO Platform service catalog (CCS, DMS, UMS, EIS, GHS, SIT, RTDP).
- Real-time push and GUI consumption: [70-gui.md](70-gui.md), [EVO User Stories.md](EVO%20User%20Stories.md).
- Retention and Data Lake pattern: [SLD Product Brief.md](SLD%20Product%20Brief.md), [Spin Level Data Req.md](Spin%20Level%20Data%20Req.md).
