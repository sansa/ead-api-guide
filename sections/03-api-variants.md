---
title: "API Variants"
nav_order: 3
permalink: /api-variants/
---

# API Variants

This page documents the API variants implemented to operationalize selected EAD-API dimensions. Each variant is intentionally **minimal** and aims to modify **only one design dimension** at a time to reduce confounding effects.

All variants are version-controlled in separate Git branches to support traceability and replication.

---

## Baseline Variant (v0)

**Goal:** Provide the unmodified reference behavior used as the comparison point for all variants.

- **Endpoint:** `GET /petclinic/api/owners`
- **Behavior:** Returns the full owner representation, including nested associated data (e.g., pets and visits), consistent with the original PetClinic REST API.
- **Purpose in study:** Establish baseline energy-per-request and latency under controlled workload conditions.

---

## D1 — Payload Efficiency Variant (v1)

**EAD-API dimension:** Payload Efficiency (D1)

**Goal:** Reduce response payload size while preserving functional intent (i.e., returning owners), without changing database access strategy or server-side computation beyond serialization.

- **New endpoint:** `GET /petclinic/api/owners/slim`
- **Behavior:** Returns a reduced owner representation containing only core fields (e.g., id, firstName, lastName, city).
- **Rationale:** Smaller response bodies reduce network transfer and reduce JSON serialization and object mapping work performed by the server.

**What changed (high-level):**
- Introduced a slim response schema (projection) for owners.
- Implemented a controller/service path that returns only the reduced representation.

**What did not change:**
- Workload parameters (rate, duration, repetitions)
- Deployment (API on host, DB in Docker)
- Measurement method (Intel RAPL)
- API semantics at the endpoint level (still returns owners; only representation differs)

**Expected effect:**
- Reduced energy per request as dataset size grows (due to lower serialization and transfer cost).

---

## D4 — Data Access Efficiency Variant (v3)

**EAD-API dimension:** Data Access Efficiency (D4)

**Goal:** Improve database access efficiency by eliminating inefficient retrieval patterns while keeping the API response format equivalent to the baseline.

- **Endpoint:** `GET /petclinic/api/owners` (same as baseline)
- **Behavior:** Returns the same full representation as the baseline.
- **Rationale:** Inefficient persistence access patterns (e.g., N+1 queries) can increase database activity and backend CPU overhead.

**What changed (high-level):**
- Replaced an N+1 retrieval pattern with a batched retrieval strategy.
- Related entities are fetched using fewer queries by batching identifiers (e.g., one query for owners and one query for associated records using `IN (...)`).

**What did not change:**
- Output schema and functional behavior (same endpoint and response structure)
- Workload parameters and measurement workflow
- Serialization approach and representation design (this is *not* a payload optimization)

**Expected effect:**
- Under higher load or larger working sets, reduced query overhead may reduce latency and/or energy; under low load, effects may be negligible and context-dependent.

---

## Notes on Variant Isolation

Each variant is designed to isolate a single dimension:

- **v1 (D1)** changes representation/payload size.
- **v3 (D4)** changes persistence access strategy.
- **v0 (baseline)** is the reference.

This isolation supports fair comparison across variants and reduces the risk that observed differences are caused by unrelated changes.

---

## Next Steps

The next pages describe how datasets were generated (default vs 1,000 owners), followed by the workload and measurement scripts used to collect energy and latency metrics.
