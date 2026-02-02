---
title: "Experimental Setup"
nav_order: 2
permalink: /experimental-setup/
---

# Experimental Setup

This page documents the experimental environment and execution setup used to evaluate the EAD-API framework. The goal is to enable independent replication of the experiments reported in the paper under controlled and transparent conditions.

---

## Hardware and Operating System

All experiments were conducted on a dedicated bare-metal server to avoid interference from virtualization layers.

- **Hardware:** Hetzner EX44 (Intel x86-64)
- **Operating system:** Ubuntu 22.04 LTS
- **Virtualization:** None (bare-metal execution)

The system provides access to on-chip energy counters via Intel RAPL, which are used for host-level energy measurement.

---

## Deployment Architecture

The experimental deployment separates the API execution environment from the database backend:

- **REST API:**  
  Spring PetClinic REST API executed directly on the host operating system.

- **Database:**  
  PostgreSQL deployed in a Docker container to ensure a consistent and isolated database configuration.

No other application components were containerized. Energy measurements reflect CPU package energy consumed on the host system.

---

## Subject System

The subject system is the **Spring PetClinic REST API**, a widely used open-source reference application. The experiments focus on the following endpoint: `GET /petclinic/api/owners`.

This endpoint returns a list of owners and associated data and is representative of data-driven REST APIs that combine database access, object mapping, and JSON serialization.

---

## API Variants

Multiple API variants were implemented to isolate individual EAD-API design dimensions. Each variant was developed in a separate Git branch to ensure traceability.

- **Baseline (v0):** Original PetClinic REST implementation.
- **Payload-efficient variant (v1):** Reduced response representation via a slim endpoint.
- **Data-access-optimized variant (v3):** Batched database retrieval to eliminate N+1 query patterns.

Only one design dimension was modified per variant to avoid confounding effects.

---

## Workload Generation

Client load was generated using **k6** with a constant arrival rate configuration to ensure a stable and reproducible request pattern.

- **Request rate:** 10 requests per second
- **Warm-up period:** 30 seconds
- **Measurement duration:** 180 seconds
- **Repetitions:** 3 runs per variant

The workload targets a single endpoint per run and uses HTTP GET requests with identical headers across all experiments.

---

## Energy Measurement

Energy consumption was measured using **Intel RAPL** via the Linux powercap interface: `/sys/class/powercap/intel-rapl:0/energy_uj`

Energy measurements capture CPU package energy at the host level. Energy consumed by the workload generator and database container is included only insofar as it contributes to host CPU utilization.

---

## Dataset Configurations

Two dataset sizes were used to evaluate the sensitivity of energy consumption to data volume:

- **Small dataset:** Default PetClinic dataset (approximately 10 owners)
- **Medium dataset:** Scaled dataset containing 1,000 owners

All other experimental parameters were held constant across dataset configurations.

---

## Measurement Procedure

Each experimental run followed the same procedure:

1. Execute a warm-up workload (not recorded).
2. Read the initial RAPL energy counter value.
3. Execute the measurement workload.
4. Read the final RAPL energy counter value.
5. Compute total energy consumption as the difference between the two readings.

In addition to energy consumption, request count and latency metrics were collected for each run.

---

## Reproducibility Notes

All scripts, configuration files, and raw results used in the experiment are publicly available through the companion repository. The exact workload definitions, measurement scripts, and dataset generation steps are documented on subsequent pages.

---

## Next Steps

The following page describes the implemented API variants and how each maps to specific EAD-API design dimensions.



