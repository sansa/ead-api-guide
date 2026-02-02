---
title: "Replication Guide"
nav_order: 7
permalink: /replication/
---

# Replication Guide

This page provides step-by-step instructions to replicate the experiments reported in the paper *“EAD-API: A Multidimensional Framework for Energy-Aware API Design.”* The goal is to enable independent reproduction of the reported measurements under comparable conditions.

---

## Overview of Replication Workflow

Replication consists of the following high-level steps:

1. Set up the execution environment
2. Deploy the subject system (Spring PetClinic REST)
3. Prepare the dataset
4. Execute the workload and energy measurements
5. Inspect and aggregate results

Each step is described below.

---

## System Requirements

Replication was validated on the following configuration:

- **Operating system:** Linux (Ubuntu 22.04 LTS recommended)
- **CPU:** Intel x86-64 processor with RAPL support
- **Java:** JDK 17 or later
- **Docker:** Required for PostgreSQL
- **k6:** Required for workload generation
- **Python:** Version 3.10 or later

> **Note:** Energy measurements rely on Intel RAPL and therefore require execution on bare-metal hardware with access to `/sys/class/powercap`.

---

## Repository Structure

## Repository Structure

The replication package is composed of two primary GitHub repositories:

- **Spring PetClinic REST (fork):**  
  https://github.com/sansa/spring-petclinic-rest  
  This repository contains the subject system used in the study, including all API variants (baseline, payload-efficient, and data-access-optimized). Each variant is maintained in a separate Git branch.

- **Energy measurement and workload scripts:**  
  https://github.com/sansa/vf-rapl  
  This repository contains the Python scripts for orchestrating workloads and collecting energy measurements using Intel RAPL, as well as the k6 workload definitions and result aggregation utilities.

Together, these repositories provide all source code, scripts, and configuration files required to reproduce the experiments reported in the paper.

---

## Step 1: Deploy the Subject System

1. Clone the repository containing the Spring PetClinic REST API.
2. Check out the desired API variant branch:
   - `v0` — Baseline
   - `v1` — Payload-efficient variant
   - `v3` — Data-access-optimized variant
3. Build and start the REST API on the host machine.

The REST API should be accessible at:

```
http://localhost:9966/petclinic/api/
```

---

## Step 2: Start the Database Backend

1. Start the PostgreSQL container using the provided Docker configuration.
2. Ensure that the database schema is initialized before starting the API.

The database runs in a container but shares the host CPU resources, which are included in the energy measurements.

---

## Step 3: Prepare the Dataset

Two dataset configurations are supported:

- **Small dataset:** Default PetClinic dataset (no additional setup required)
- **Medium dataset:** Execute the provided SQL script to populate approximately 1,000 owners

Dataset scripts are located in the dataset directory of the companion repository. Ensure that the database is restarted or reinitialized between dataset changes.

---

## Step 4: Execute the Workload and Measurement

Energy measurements are automated using a Python script that coordinates workload execution and RAPL sampling.

A typical execution command has the following form:

```
sudo python3 measure_rapl.py
--variant <variant_id>
--run-id <run_number>
--base-url http://localhost:9966

--endpoint <endpoint_path>
--rate 10
--warmup 30
--duration 180
```


Each configuration should be executed **three times** to account for measurement variability.

> **Note:** Root privileges are required to access the RAPL energy counters.

---

## Step 5: Inspect and Aggregate Results

Each run produces a row in a CSV file containing:

- Variant identifier
- Dataset size metadata
- Total energy consumption (J)
- Requests completed
- Energy per request
- Latency metrics

Aggregation scripts are provided to compute averages and summary statistics across runs.

---

## Validation Checks

To verify correct execution:

- Confirm that approximately 1,800 requests are completed per run
- Confirm that RAPL counters increase monotonically
- Confirm that workload rate remains stable at 10 requests per second

Example baseline energy-per-request values are reported in the paper for reference.

---

## Reproducibility Notes

Minor numerical differences are expected across machines due to hardware, firmware, and operating system variations. However, relative trends between API variants should remain consistent when following the described procedure.

---

## Contact and Issues

If issues arise during replication, please consult the paper and repository documentation. Questions and corrections can be reported via the issue tracker associated with the companion repositories.
