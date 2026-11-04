# HookRail

Reliable webhook delivery infrastructure for payment platforms.

HookRail accepts events from producers, persists them durably, fans them out to subscribed merchant destinations, and manages delivery attempts, retries, recovery, and operational visibility.

## Product direction

- Primary users: payment platforms; expand to SaaS event producers.
- Delivery semantics: at-least-once; duplicate deliveries are possible and consumers must make business effects idempotent.
- Initial benchmark targets: 100 events/second sustained and bursts of 1,000 events/second. These are targets, not current performance claims.
- Backend: Go. Database: PostgreSQL. Local orchestration: Docker Compose.
- Initial job queue: PostgreSQL-backed; evaluate Kafka later if workload evidence justifies it.
- Payload storage: S3-compatible object storage, using MinIO locally.
- One event may fan out to multiple destinations; each event-destination delivery has independent state.

## Reliability principles

1. Do not return `202 Accepted` until the payload and authoritative event/job records are durably available under the configured storage guarantees.
2. Create event metadata and initial delivery jobs atomically in PostgreSQL.
3. Use stable event IDs and expect duplicate deliveries; consumers must implement idempotent business processing.
4. Recover abandoned work using time-bounded leases.
5. Isolate tenants and destinations.
6. Treat destination URLs as untrusted and enforce SSRF protections on outbound requests.
7. Measure before introducing Kafka, read replicas, or multi-region deployment.

## Architecture and roadmap

See [architecture decisions](docs/architecture/decisions.md) and the [implementation roadmap](docs/roadmap.md).

## Development status

Current milestone: repository and engineering baseline. No production-readiness or throughput claims are made until correctness, recovery, security, and load tests have been implemented and run.
