# Payments Platform Sandbox

A **backend + infra sandbox** for payments systems, focused on orchestration, compliance, ACH, and operational visibility.  
Built with **Java + Spring Boot**, **Moov ACH**, **OPA**, and **Orkes Conductor**.

## Quick Start
Run the platform locally with Docker Compose:

```bash
git clone https://github.com/payments-platform-sandbox/dev-env
cd dev-env && cp .env.example .env
docker compose up -d
# Gateway available at http://localhost:8000
```

## Functional Requirements

### This platform was designed around five core Functional Requirements (FRs):

**1. FR1 – Payment Workflow Execution** <br> Orchestrate payment lifecycles across services with retries, timeouts, and compensation.

**2. FR2 – Webhook Handling** <br>Resume workflows based on async events from Moov or external systems.

**3. FR3 – Reconciliation** <br>Periodically reconcile ledger entries against Moov ACH state to ensure consistency.

**4. FR4 – Refunds & Cancellations** <br>Support reversals of payments, subject to policy checks and orchestration logic.

**5. FR5 – Ops Visibility & Recovery** <br>Provide observability, metrics, and workflow inspection for debugging and operations.

## Architecture (at a glance)
```
Client -> Gateway -> payment-service -> orchestration (Orkes)
                                    \-> opa-compliance (OPA)
                                    \-> moov-ach-ledger (Moov + Postgres)
```

## Repositories

| Repo | Purpose | Functional Requirements |
|------|---------|--------------------------|
| [dev-env](https://github.com/payments-platform-sandbox/dev-env) | Local stack: gateway, Redis, Postgres (+ optional Kafka) | FR1, FR2, FR3, FR4, FR5 |
| [payment-service](https://github.com/payments-platform-sandbox/payment-service) | Public API: create/refund/cancel with idempotency + OPA checks | FR1, FR2, FR4 |
| [orchestration](https://github.com/payments-platform-sandbox/orchestration) | Orkes workflows: retries, timeouts, compensation, reconciliation | FR1, FR2, FR3, FR4, FR5 |
| [opa-compliance](https://github.com/payments-platform-sandbox/opa-compliance) | Policy-as-code service using OPA for transaction rules | FR1, FR4 |
| [moov-ach-ledger](https://github.com/payments-platform-sandbox/moov-ach-ledger) | ACH integration + immutable ledger with reconciliation jobs | FR1, FR2, FR3, FR4 |
| [contracts](https://github.com/payments-platform-sandbox/contracts) | Shared OpenAPI + event schemas (source of truth for APIs/events) | FR1–FR5 |
| [observability](https://github.com/payments-platform-sandbox/observability) | Prometheus + Grafana dashboards *(optional)* | FR5 |
| [platform-infra](https://github.com/payments-platform-sandbox/platform-infra) | Terraform/Helm/Kubernetes for optional cloud deployments | *(Infra only, not tied to FRs)* |

## Notes
- **Local-first:** Everything runs via `dev-env` with Docker Compose.  
- **Optional extras:** Observability and platform-infra are not required to run FR1–FR5.  
- **Context:** [Transaction at Scale](https://transactionatscale.substack.com/)  
