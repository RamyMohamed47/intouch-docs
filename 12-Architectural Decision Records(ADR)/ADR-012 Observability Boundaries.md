# ADR-012: Observability Boundaries

- **Status:** Accepted

## Decision

Use structured Pino logs, OpenTelemetry OTLP metrics and sampled traces, Grafana dashboards, and separate Sentry projects for API, web, and mobile. Telemetry is optional and never controls liveness/readiness.

Metrics use bounded labels. Logs/errors/traces exclude tokens, cookies, message and Echo content, emails, filenames, provider credentials, presigned URLs, room names, and user/conversation/organization IDs unless an explicitly safe operational identifier is required.


## Decision Trade-offs

### Chosen: Optional OpenTelemetry/Grafana plus separate Sentry projects

**Pros**

- Metrics, traces, logs, and errors cover complementary operational questions.
- Separate API/web/mobile Sentry projects preserve release and runtime context.
- Telemetry can be disabled or unavailable without changing application readiness.

**Cons**

- Adds SDK configuration, dashboards, sampling, source-map uploads, alert tuning, and cost.
- Instrumentation must be reviewed continuously to prevent high-cardinality or sensitive data.
- Multiple tools require correlation conventions and operational ownership.

### Alternative: Structured logs only

**Pros**

- Lowest setup cost and one operational surface.

**Cons**

- Weak aggregate latency/rate visibility and slower exception triage.
- Cross-service traces, mobile crashes, and release symbolication are missing.

### Alternative: Make telemetry a readiness dependency

**Pros**

- Guarantees every ready instance can export telemetry.

**Cons**

- A monitoring outage can unnecessarily take a healthy product offline.
- Creates a circular operational dependency during incidents.

### Alternative: Capture unrestricted request and user payloads

**Pros**

- Maximum debugging context.

**Cons**

- Leaks messages, Echo content, credentials, URLs, and personal data.
- Creates unacceptable privacy, security, and retention risk.

## Consequences

Operators can inspect latency, rates, dependencies, queues, calls, AI, push, and failures without turning observability into a privacy leak or availability dependency.
