# 🎬 Movie Booking Platform — System Design & Architecture

> Cloud-native movie ticket booking platform — Microservices, SAGA, CQRS, REST APIs, AWS | Java & Spring Boot

---

## Problem Statement

XYZ wants to build an online movie ticket booking platform serving two types of clients:

- **B2B — Theatre Partners**: Onboard theatres digitally, manage shows & seat inventory, reach a larger customer base
- **B2C — End Customers**: Browse movies across cities, languages & genres, and book tickets in advance with a seamless experience

---

## Technologies Used

| Category | Choice |
|---|---|
| Language & Framework | Java 17, Spring Boot 3 |
| API Gateway | Kong / AWS API Gateway |
| Databases | PostgreSQL (Aurora), Redis, Elasticsearch |
| Messaging | Apache Kafka (AWS MSK) |
| AI | AWS Bedrock, Amazon SageMaker, AWS Lex |
| Cloud | AWS (EKS, Aurora, MSK, ElastiCache, CloudFront, Route53, WAF) |
| DevOps | Docker, Kubernetes, Helm, Jenkins / GitHub Actions |
| Monitoring | ELK Stack, Grafana, AWS X-Ray |
| Auth | Keycloak / AWS Cognito (OAuth2 / JWT) |
| Integration | Apache Camel / MuleSoft |
| Payments | Stripe / Razorpay |
| Notifications | Twilio / AWS SNS / Firebase |

---

## Section 1 — Functional Features & Code Implementation

### Architecture
Microservices-based architecture with 6 core services — User, Theatre, Booking, Payment, Notification, and Search — behind an API Gateway handling auth, rate limiting, routing, and SSL termination.

### AI Integration
- **Smart Search & Recommendations** — natural language search and collaborative filtering via AWS Bedrock
- **Dynamic Pricing Engine** — ML-based demand prediction with SageMaker for surge pricing
- **Fraud Detection** — real-time anomaly detection on booking patterns
- **Chatbot** — AWS Lex for customer support, handling show timings, bookings, and cancellations

### Functional Scenarios Covered
- Browse theatres running a show by city and date
- Platform offers — 50% discount on 3rd ticket, 20% discount on afternoon shows
- Book tickets by selecting theatre, timing, and preferred seats
- Theatre show management — create, update, delete shows
- Seat inventory allocation and updates per show
- Bulk booking and cancellation with all-or-nothing transaction handling

### API Design
RESTful APIs following best practices — URI versioning (`/api/v1/`), pagination, idempotency keys, HATEOAS, OAuth2 + JWT security, and OpenAPI 3.0 documentation via Swagger.

### Design Patterns Applied
Strategy, Builder, Repository, DTO, Adapter (Anti-Corruption Layer), Circuit Breaker, SAGA, and CQRS.

### Data Model
8 core entities — User, Theatre, Movie, Show, Seat, Booking, Payment, and Offer — with relationships designed for ACID compliance, city-based sharding, and monthly partitioning.

---

## Section 2 — Non-Functional Requirements

### Transactions
SAGA choreography pattern for distributed transactions. Optimistic locking for concurrent seat reservation. Idempotency keys for safe payment retries. Full compensation flow on failure.

### Theatre Integration & Localization
- **New theatres** — REST API self-service onboarding with webhook subscriptions
- **Legacy IT systems** — Adapter layer with SOAP-to-REST bridge and SFTP batch pull via Apache Camel
- **Localization** — Multi-language content, multi-currency, timezone-aware timestamps, RTL language support, and region-based tax rules

### Scalability & 99.99% Availability
Kubernetes HPA for auto-scaling, stateless services, Multi-AZ deployment, Aurora Global DB with sub-30-second failover, and CloudFront CDN. DB sharded by city.

### Payment Gateway Integration
Stripe and Razorpay with tokenization (no raw card storage), idempotency on retries, and async refund processing via Kafka.

### Monetization
Convenience fee per ticket, theatre SaaS subscription tiers, ad placements, dynamic pricing commission, anonymized audience data products, and gift card float income.

### Security — OWASP Top 10
Parameterized queries, JWT with short expiry, output encoding, CSRF protection, rate limiting at gateway, TLS 1.3, and OWASP Dependency Check in CI/CD.

### Compliance
PCI-DSS (tokenization, quarterly audits), GDPR/PDPA (right to erasure, data minimization, data residency), CERT-In India (incident reporting within 6 hours, 180-day log retention), and WCAG 2.1 accessibility.

---

## Section 3 — Platform Provisioning, Sizing & Release

### Technology Decisions
Each technology chosen based on clear drivers — PostgreSQL for ACID guarantees, Kafka for event replay and SAGA, Redis for sub-millisecond caching, and Elasticsearch for full-text geo-aware search.

### Hosting & Sizing
Primary on AWS ap-south-1 (Mumbai) for India data residency. Singapore as DR region. GCP in Phase 2 for ML workloads to avoid vendor lock-in. Initial launch sized across EKS pods, Aurora, Redis cluster, Kafka, and Elasticsearch.

### COTS Enterprise Systems
Keycloak/Cognito for identity, Stripe/Razorpay for payments, Twilio/SNS for notifications, Salesforce/HubSpot for B2B CRM, Apache Camel/MuleSoft for integration, and Datadog/New Relic for APM — all chosen to avoid rebuilding solved problems.

### CI/CD Pipeline
GitHub (GitFlow) → Maven build → JUnit + Mockito → SonarQube (80% coverage gate) → OWASP scan → Docker image → Helm deploy to EKS via Blue/Green. Zero-downtime releases.

### Release Management & Internationalization
Feature flags (LaunchDarkly) for region-specific rollout. Phased city launch — Tier-1 first, then Tier-2, then international. Per-region AWS deployments with Route53 latency routing. DB migrations via Flyway.

### Monitoring & Log Analysis
ELK Stack for log aggregation, Grafana for infrastructure metrics, AWS X-Ray for distributed tracing, and CloudWatch alarms for latency and error rate thresholds.

### Platform KPIs
99.99% availability, p95 API latency under 300ms, booking success rate above 98%, payment success rate above 99.2%, and theatre onboarding under 2 business days.

### Project Plan
4 phases over 24 weeks — Foundation (6 wks), Core Features (8 wks), Payments & NFR (6 wks), Launch Readiness (4 wks) — with a team of 9 across architecture, backend, DevOps, and QA.

---

## Section 4 — Product & Stakeholder Management

### Stakeholder Management
Key stakeholders — theatre partners (B2B), end customers (B2C), product owner, and engineering lead — engaged through regular cadences with decisions driven by data and documented as Architecture Decision Records (ADRs).

Notable decisions resolved: Microservices vs Monolith (phased approach signed off by CTO), theatre integration standards (standard contract with adapter for legacy), and dynamic pricing rollout (opt-in pilot showing 18% revenue uplift before wider adoption).

### Technology Management
ADRs for every major tech choice, a quarterly tech radar, SonarQube quality gates enforced in CI/CD, Dependabot for dependency hygiene, and a dedicated tech debt allocation of 20% per sprint.

### Team Enablement
Shared inner-source libraries, Maven archetype scaffolding for new services, weekly recorded tech talks, pair programming for complex modules, and GitHub Copilot adoption for productivity gains.

### Delivery Planning
SAFe Agile with 2-week sprints and quarterly PI Planning. Fibonacci story point estimation, RAID log for risk management, and a clear Definition of Done covering review, testing, quality gates, and staging deployment.

---

## Presentation

Full solution presented in `Sunil_Reddy_Booking_Platform_Solution.pdf` — 23 slides covering all four case study sections in order.

| Slides | Coverage |
|---|---|
| 1–4 | Problem Statement, Agenda, AI & Technologies |
| 5–10 | Architecture, Data Model, APIs, Code Scenarios, SAGA Flow |
| 11–14 | Integration, NFRs, Monetization, Compliance |
| 15–20 | Tech Choices, COTS, Hosting, DevOps, Release Mgmt, Project Plan |
| 21–23 | Stakeholder Mgmt, Delivery Mgmt, Closing |
