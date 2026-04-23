
# Insurance Payment & Refund Lifecycle Prototype

## Problem
Insurance systems often delay excess refunds (T+30 days) due to manual validation, reconciliation gaps, and lack of account verification controls.

---

## What this project demonstrates
An interactive simulation of an end-to-end insurance payment lifecycle:

NEFT Collection → UW Tagging → GL Mapping → Penny Drop Verification → Refund Control

This prototype focuses on financial integrity, operational efficiency, and risk control in BFSI systems.

---

## Key Product Logic

- Detects excess collection and holds it in GL (4404 - Suspense)
- Blocks refund until account is verified via penny drop
- Prevents failed payouts and unnecessary GL reversals
- Handles partial payments and blocks policy conversion
- Simulates bank failures and retry scenarios
- Maintains full transaction traceability via logs and trace IDs

---

## Try it
(Live link will be added after deployment)

Steps:
1. Submit NEFT transaction  
2. Observe excess / shortfall calculation  
3. View GL mapping entries  
4. Run penny drop verification  
5. Try refund (blocked/unlocked based on verification status)  

---

## Tech Stack

- Node.js (Express)
- JavaScript (Frontend)
- In-memory data store (simulation)

---

## Project Structure

```

insurance-lifecycle/
├── backend/
│   ├── server.js          # Express app, all routes
│   ├── db.js              # In-memory store
│   ├── package.json
│   └── services/
│       ├── logger.js      # Structured trace logging
│       ├── payment.js     # NEFT ingestion + validation
│       ├── gl.js          # Dynamic GL entry generation
│       ├── verification.js # Penny drop + failure simulation
│       └── refund.js      # Refund logic with dependency checks
└── public/
└── index.html         # Integrated frontend

````

---

## Run Locally

```bash
cd backend
npm install
node server.js
````

Open:
[http://localhost:3000/index.html](http://localhost:3000/index.html)

---

## API Reference

| Method | Endpoint                   | Description                |
| ------ | -------------------------- | -------------------------- |
| POST   | /api/transactions          | Submit NEFT transaction    |
| GET    | /api/transactions          | List transactions          |
| GET    | /api/transactions/:id      | Get transaction            |
| POST   | /api/convert-policy        | Convert proposal to policy |
| POST   | /api/verify-account        | Run penny drop             |
| POST   | /api/refund                | Initiate refund            |
| GET    | /api/gl/:policy_id         | Fetch GL entries           |
| GET    | /api/logs                  | Fetch audit logs           |
| POST   | /api/simulate/bank-failure | Toggle bank failure        |
| POST   | /api/simulate/reset        | Reset simulation           |
| GET    | /api/health                | Health check               |

---

## Edge Cases Handled

* Duplicate UTR → rejected
* Invalid IFSC → validation error
* Partial payment → blocks policy conversion
* Excess payment → mapped to GL-4404 (refund hold)
* Penny drop failure → retry logic + refund lock
* Refund without verification → blocked
* Duplicate refund → prevented
* Refund failure simulation → supported

---

## GL Mapping Logic

| Code | Description                     | Ledger            |
| ---- | ------------------------------- | ----------------- |
| 4401 | First Year Premium              | Revenue A/c       |
| 4402 | Risk Rider Premium              | Revenue A/c       |
| 4403 | GST Collected (18%)             | Tax Liability A/c |
| 4404 | Excess Collection (Refund Hold) | Suspense A/c      |
| 4405 | Partial Payment Shortfall       | Suspense A/c      |

---

## Traceability

Each transaction generates:

* Unique Transaction ID
* UTR Reference
* Trace ID for system-wide tracking

Example log:

```json
{
  "trace_id": "TRC-A1B2C3D4",
  "event": "TRANSACTION_RECEIVED",
  "service": "payment-service",
  "timestamp": "2026-04-23T09:14:32.000Z"
}
```

---

## Why this matters

This prototype demonstrates how real-world BFSI systems ensure:

* Financial accuracy
* Risk control before payout
* Proper accounting via GL mapping
* Reduced operational overhead
* End-to-end system visibility


You’re very close now.
```
