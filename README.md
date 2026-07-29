## Hey, I'm Vansh

Backend / full-stack engineer building distributed systems — log pipelines, governance control planes, and real-time infrastructure. I design for multi-tenancy, fail-closed defaults, and trust boundaries. Open to building software that holds up under real load.

`vanshseth0209@gmail.com` · [LinkedIn](https://www.linkedin.com/in/vansh-seth-03bb66324/)

---

### LogPulse — Real-Time Log Aggregation & Alerting

Self-hosted log platform. Apps send logs to one endpoint; LogPulse validates, stores, streams live, and fires alerts.

```text
app ─POST /ingest─▶ ingestor (Go) ─▶ RabbitMQ fanout
                                        ├─ processor (Go) ─▶ MongoDB
                                        ├─ ws-service (Go) ─▶ browser (WebSocket)
                                        └─ alert-service (Node) ─▶ email / webhook

dashboard ─▶ nginx :80 ─▶ api-service (Node) ─▶ MongoDB + Redis
```

| | |
|---|---|
| **Problem** | Centralized log visibility without Datadog/Grafana Cloud cost |
| **Hot path** | Go for ingestor, processor, ws-service — concurrency + memory efficiency |
| **I/O path** | Node.js for api-service, alert-service — event-driven, not CPU-bound |
| **Trust boundary** | Ingestor is the only service touching untrusted input — full sanitization |
| **Rate limiting** | Dual-layer: Nginx (per IP) + ingestor (per API key, 1000/min, 50/sec burst) |
| **Alerting** | Redis sliding-window counters (ZSET) + cooldown TTL keys |
| **WebSocket auth** | JWT in first message, not query param — project-scoped rooms |
| **Tenancy** | `projectId` on every document and query · admin / member roles |
| **Retention** | 30-day auto-delete via MongoDB TTL index |

**Tech:** `Go` `Gin` `gorilla/websocket` `Node.js` `Express` `Mongoose` `React` `Vite` `Tailwind` `Zustand` `Recharts` `MongoDB` `Redis` `RabbitMQ` `Nginx` `Docker Compose`

→ [github.com/Vanshsethh/logpulse](https://github.com/Vanshsethh/logpulse)

---

### Forge — Governance Control Plane for Autonomous Financial Agents

Real-time guardrail system. Every AI agent action passes through Forge before execution — policy check, spend cap, kill switch, tamper-evident audit.

```text
agent ─HMAC signed─▶ gateway-service ─▶ OPA (policy) ─▶ Redis (spend caps)
                         │                                │
                         ├─ kill switch check             ├─ per-tx / hourly / daily caps
                         ├─ revocation check              │
                         └─ verdict: allow / deny         ▼
                              │                    MySQL audit ledger
                              │                    (SHA-256 hash chain)
                              ▼
                    fail-closed: deny if any dependency is down

dashboard ─▶ admin-service ─▶ Redis / MySQL ─▶ fleet view, spend, kill switches
```

| | |
|---|---|
| **Problem** | Autonomous agents act in milliseconds — one mis-scoped agent = thousands of harmful actions |
| **Fail-closed** | OPA / Redis / MySQL down → automatic deny, never a silent allow |
| **Policy engine** | OPA as HTTP sidecar — update rules without redeploying the gateway |
| **Audit ledger** | Append-only SHA-256 hash chain · app MySQL user has no UPDATE/DELETE privilege |
| **Tamper proof** | Test mutates a row via root connection, verifier detects the exact altered entry |
| **Spend caps** | Redis sliding-window counters — per-transaction, hourly, daily |
| **Kill switches** | Fleet-wide + per-agent, instant revocation via Redis flags |
| **Gateway/admin split** | Hot path (evaluate + log) isolated from dashboard queries |
| **Simulator** | 3 agents (payments, servicing, travel) + rogue overspend scenario |

**Tech:** `Node.js` `Express` `MySQL` `Redis` `Open Policy Agent` `Rego` `React` `Vite` `Tailwind` `Zustand` `Recharts` `Docker Compose`

→ [github.com/Vanshsethh/forge](https://github.com/Vanshsethh/forge)

---

### Technical Skills

| Category | Technologies |
|---|---|
| **Languages** | `C` `C++` `Go` `SQL` `HTML5` `CSS` `JavaScript` `TypeScript` |
| **Frameworks & Databases** | `React.js` `Node.js` `Express.js` `RabbitMQ` `Redis` `REST API` `MySQL` |
| **Tools & Platforms** | `Docker` `Git & GitHub` `Tailwind CSS` `Vercel` |
| **CS Concepts** | OOP · DBMS · OS · Computer Networks · DSA · Complexity Analysis |

---


### GitHub Stats

<p>
  <img height="160" src="https://github-readme-stats.vercel.app/api?username=Vanshsethh&show_icons=true&theme=dark&hide_border=true&count_private=true" />
  <img height="160" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Vanshsethh&layout=compact&theme=dark&hide_border=true" />
</p>
