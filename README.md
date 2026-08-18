# Devam Dholakia

CS student at the **University of Central Florida** (B.S. '28). I like backend systems: queues, delivery guarantees, and the parts of a service that only matter when something is already going wrong.

- 🛠️ Prev. Software Engineering Intern at **USAA** (Summer 2026), working on Spring Boot, GraphQL, and an internal CRM platform
- 🔬 Research Assistant at UCF's **ISUE Lab**, on language-conditioned 3D scene generation
- 🎓 Secretary of **AI@UCF**
- 📫 [LinkedIn](https://linkedin.com/in/devam-dholakia) · devd4312@gmail.com

---

## Selected projects

### 🔁 [Webhookd](https://github.com/devamdholakia/Webhook): at-least-once webhook delivery
`Java 21` `Spring Boot 3` `SQS` `DynamoDB` `Docker`

Accepts events over HTTP, fans them out to subscriber endpoints, retries with full-jitter exponential backoff, and dead-letters after a bounded number of attempts.

The interesting parts are the failure paths:
- **Attempts counted in DynamoDB, not by SQS.** A circuit breaker that parks a message still increments `ApproximateReceiveCount`, so letting SQS own the retry budget means healthy events die without a single HTTP call being made.
- **State written before the enqueue**, with a sweeper over a sparse GSI to re-enqueue orphans. That's what makes the at-least-once claim honest rather than aspirational.
- **Per-subscription circuit breaker**, so one dead endpoint can't occupy the worker pool while healthy subscribers queue behind it.

Load-tested to 200 events/sec against a failure-injection receiver, with zero data loss across simulated 10-minute subscriber outages. Cost modeled at ~$7.10 per million deliveries from on-demand pricing, which turns out to be DynamoDB-write-bound rather than compute-bound.

### 🏎️ [Apex](https://github.com/Akhileshreddym/Apex): F1 pit-wall simulator
`Next.js` `FastAPI` `XGBoost` `NumPy` `WebSockets`

Live race-strategy tool. A two-stage Ridge + XGBoost pipeline predicts remaining-race lap times to **0.91s MAE** at mid-race, a 41% error cut over a mean-of-observed-laps baseline. A vectorized Monte Carlo engine runs 10,000 race simulations at 14ms p95 and streams strategy updates to the client at ~120/sec.

Worth noting how that number was arrived at. The first version scored 0.35s MAE with an R² of 0.996, which was too good. Tracing it back, a base-pace feature was computed from per-race median lap times inside the training fold, and shuffled K-fold splits put laps from the same race on both sides. The honest number, re-evaluated with forward-chaining splits that train only on laps already observed in the current race, is 0.91s.

### 🎥 [uKnight](https://github.com/uKnight-Co/uKnight): anonymous video chat for verified students
`Spring Boot` `WebRTC` `Redis` `GCP Cloud Run` `Docker`

Led a 6-engineer team; reached 72 verified .edu users in 3 weeks. Redis-backed matchmaking queue keeps pairing state out of the app tier so the signaling service scales without sticky sessions. STUN/TURN fallback relays through TURN when symmetric NAT blocks a direct peer path, which is what makes calls connect on locked-down campus networks.

### 📦 SAP Archive Decoder: 🏆 1st place, KnightHacks 2025
`Python` `concurrent.futures` `gzip` `lz4` `zstandard`

Auritas Challenge winner. Decoded 5/5 binary SAP archives to CSV/JSON at ≥95% field accuracy in 36 hours, with schema-driven parsers for 7 field types including PACKED/BCD, and error-tolerant parsing that skips corrupt records instead of dying on them.

---

## Stack

**Languages** Java · Python · C · JavaScript · SQL
**Backend** Spring Boot · FastAPI · GraphQL · Node.js
**Data** PostgreSQL · DynamoDB · Redis · Cassandra
**Infra** AWS (SQS, DynamoDB, ECS) · GCP Cloud Run · Docker · GitHub Actions
**ML** XGBoost · scikit-learn · PyTorch (QLoRA/PEFT) · NumPy · pandas
