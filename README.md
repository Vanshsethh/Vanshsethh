## Hey, I'm Vansh

I build distributed backend systems and real-time infrastructure — log pipelines, governance control planes, and the kind of services that sit between an application and its data. I work across Go and Node.js, design for multi-tenancy and fail-closed defaults, and care about the unglamorous parts: rate limiting, audit trails, and trust boundaries. Currently open to learning and building software that holds up under real load.

---

### LogPulse — Real-Time Log Aggregation & Alerting

A self-hosted platform where applications send logs to a single HTTP endpoint and LogPulse handles the rest: validation, durable storage, live streaming, search, and threshold-based alerting.

**Problem it solves:** Centralized log visibility without the cost of Datadog or Grafana Cloud. One ingestion endpoint, multi-tenant isolation, and alerts that fire the moment something breaks.

**Architecture highlights:**
- **Go for the hot path, Node for the I/O path.** The ingestor, processor, and WebSocket service are written in Go for concurrency and memory efficiency. The API and alert services stay in Node.js because they're event-driven, not CPU-bound.
- **RabbitMQ fanout as the backbone.** Every log hits a fanout exchange and fans out to three queues simultaneously — storage, streaming, and alerting. If MongoDB is down, logs queue safely and nothing is lost.
- **Trust boundary at the ingestor.** It's the only service touching untrusted input. It does full sanitization, API-key authentication, and dual-layer rate limiting. Every downstream service validates shape but trusts content.
- **WebSocket auth in the first message, not the URL.** JWT is sent as the first frame after connection — query params leak into server logs. The server validates the token, checks project membership via the API service, and joins the client to a project-scoped room.
- **Redis sliding-window counters for alerting.** Each rule maintains a sorted set in Redis. Logs are added with timestamps, old entries are pruned, and the count is checked against the threshold. Cooldowns are separate Redis TTL keys.

**Core features:** Live tail console (500-log ring buffer), full-text search with pagination, analytics dashboard, alert rule builder (threshold + level + service filter, 1m/5m/15m windows, 5m/15m/30m cooldowns), email + webhook delivery, project-level multi-tenancy with admin/member roles, 30-day TTL auto-retention.

**Tech:** Go · Gin · gorilla/websocket · Node.js · Express · Mongoose · React · Vite · Tailwind · Zustand · Recharts · MongoDB · Redis · RabbitMQ · Nginx · Docker Compose

`github.com/Vanshsethh/logpulse`

---

### Forge — Governance Control Plane for Autonomous Financial Agents

A real-time guardrail system that evaluates every action an AI agent wants to take before it executes — policy check, spend cap enforcement, kill switch, and a tamper-evident audit ledger.

**Problem it solves:** As banks deploy fleets of autonomous agents acting in milliseconds, one mis-scoped or compromised agent could take thousands of harmful actions before anyone notices. Forge is the infrastructure to scope, meter, halt, and audit them.

**Architecture highlights:**
- **Fail-closed by design.** If OPA, Redis, or MySQL is unreachable, the default verdict is deny. A governance layer that fails open is worse than no governance layer.
- **OPA as a sidecar, not embedded.** Policies are evaluated over HTTP by the Open Policy Agent, so rules can be updated without redeploying the gateway — the same pattern Netflix and other companies use in production.
- **Tamper-evident SHA-256 hash chain.** Every decision is appended to a MySQL audit log where each entry's hash includes the previous entry's hash. The application MySQL user has no UPDATE or DELETE privilege on the audit table. A test mutates a row through a root connection and proves the verifier detects the exact altered entry.
- **Gateway/admin split for hot-path isolation.** The gateway evaluates and logs — nothing else. The admin API handles dashboard queries and configuration. Splitting them means a heavy audit-log query never slows down an agent action.
- **HMAC-signed agent requests with replay protection.** The simulator signs requests with the same HMAC format the gateway verifies.

**Core features:** Per-transaction / hourly / daily Redis-backed spend caps, fleet-wide and per-agent kill switches, OPA policy evaluation (payments, servicing, travel), append-only audit ledger with tamper-detection test, React operator dashboard, three-agent fleet simulator with a rogue overspend scenario.

**Tech:** Node.js · Express · MySQL · Redis · Open Policy Agent (Rego) · React · Vite · Tailwind · Zustand · Recharts · Docker Compose

`github.com/Vanshsethh/forge`

---

### Technical Skills

| | |
|---|---|
| **Languages** | Go · JavaScript · SQL · Rego |
| **Backend** | Node.js · Express · Go · Gin · JWT · bcrypt · Helmet · express-validator · Open Policy Agent |
| **Frontend** | React · Vite · Tailwind · Zustand · Recharts · Axios |
| **Databases** | MongoDB · MySQL · Redis |
| **DevOps & Infra** | Docker · Docker Compose · Nginx · RabbitMQ · GitHub |
| **Concepts** | Microservices · Message Queues · Rate Limiting · Multi-tenancy · WebSocket Auth · Hash-chain Audit · Fail-closed Design |

---

### GitHub Stats

<p>
  <img height="160" src="https://github-readme-stats.vercel.app/api?username=Vanshsethh&show_icons=true&theme=dark&hide_border=true&count_private=true" />
  <img height="160" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Vanshsethh&layout=compact&theme=dark&hide_border=true" />
</p>

---

### Contact

`vanshseth0209@gmail.com` · [LinkedIn](https://www.linkedin.com/in/vansh-seth-03bb66324/)