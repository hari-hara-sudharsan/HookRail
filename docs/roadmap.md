# HookRail — Implementation Roadmap

The roadmap is milestone-based. Each milestone requires tests and review before the next capability is treated as complete.

## M0 — Engineering foundation

- [x] Create GitHub repository.
- [x] Record product direction and initial architecture decisions.
- [ ] Agree on service boundaries and module ownership.
- [ ] Establish branch, commit, and pull-request conventions.
- [ ] Add contribution and local development instructions.

## M1 — Durable event ingestion

- [ ] Go module and service bootstrap.
- [ ] Configuration loading and structured logging.
- [ ] Docker Compose for PostgreSQL and MinIO.
- [ ] PostgreSQL migrations.
- [ ] Tenant model and authentication foundation.
- [ ] Event ingestion API and request validation.
- [ ] Tenant-scoped idempotency with request fingerprint conflict detection.
- [ ] Payload upload and verification in MinIO.
- [ ] Atomic event metadata and delivery-job creation.
- [ ] Correct success/error responses.
- [ ] Unit and integration tests for duplicate requests and partial failures.

Exit criteria: No 202 Accepted is returned unless the payload is available and the event plus its initial jobs are committed. Repeated same-key/same-request submissions do not create duplicate logical events; same-key/different-request submissions are rejected.

## M2 — Destination management and fan-out

- [ ] Tenant-owned destination CRUD.
- [ ] Event-type subscription rules.
- [ ] Destination URL validation.
- [ ] Snapshot destination selection at event acceptance; policy to be finalized.
- [ ] Create one independent delivery job per matched destination.
- [ ] Tests for fan-out, destination changes, and tenant isolation.

## M3 — Delivery workers

- [ ] PostgreSQL job-claim query and indexes.
- [ ] Short transaction using row locks and SKIP LOCKED.
- [ ] Time-bounded worker leases.
- [ ] HTTP delivery with strict timeouts and response limits.
- [ ] HMAC signing and signature verification example.
- [ ] Append delivery-attempt history.
- [ ] Worker crash and lease-expiry recovery tests.

## M4 — Retry and failure management

- [ ] Retryable versus permanent error classification.
- [ ] Backoff policy with jitter and bounded attempts/age.
- [ ] Scheduler/sweeper.
- [ ] Dead-letter state and controlled replay.
- [ ] Circuit breaking and destination-level concurrency limits.
- [ ] Tests for ambiguous outcomes and repeated delivery.

## M5 — Security and tenant fairness

- [ ] API key hashing, rotation, and revocation.
- [ ] RBAC for dashboard/admin actions.
- [ ] Egress SSRF defense and DNS rebinding protections.
- [ ] Redirect policy and connection-time IP validation.
- [ ] Per-tenant ingestion and delivery quotas.
- [ ] Secret management and redaction.
- [ ] Security-focused integration tests.

## M6 — Dashboard and operations

- [ ] Secured dashboard/admin APIs.
- [ ] Event explorer and delivery history.
- [ ] Destination management UI.
- [ ] Replay workflow with audit trail.
- [ ] Metrics, health/readiness endpoints, and tracing.
- [ ] Alerting on backlog, error rate, and worker/database health.

## M7 — Performance and production validation

- [ ] Reproducible load-test workload.
- [ ] Benchmark 100 events/second sustained and 1,000 events/second bursts.
- [ ] Record p50/p95/p99 latency, delivery throughput, backlog, CPU, memory, and database metrics.
- [ ] Test database outage, object-store outage, worker crash, slow destination, and traffic spike.
- [ ] Backup/restore and disaster-recovery exercise.
- [ ] Deployment, secrets, migrations, rollback, and operational runbooks.
- [ ] Decide whether Kafka, read replicas, or service extraction are justified by evidence.

## Working rule

A checkbox is completed only after implementation, tests, and review. A feature that works only on the happy path is not considered complete.
