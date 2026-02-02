---
title: "Results and Data"
nav_order: 6
permalink: /results/
---

# Results and Data

This page summarizes the experimental results obtained from evaluating selected EAD-API design dimensions. Results are reported for multiple API variants under controlled workload conditions and two dataset sizes. Raw measurement data and aggregation scripts are provided to support independent analysis.

---

## Overview of Results

Each API variant was evaluated using a constant-arrival-rate workload of 10 requests per second. For each configuration (variant × dataset size), the experiment was repeated three times.

The primary metric of interest is **energy per request (J/request)**, computed as total measured CPU package energy divided by the number of completed requests. Latency metrics are reported to contextualize energy results.

Unless stated otherwise, all results reported on this page correspond to experiments conducted using the PostgreSQL backend.

---

## Small Dataset Results (≈10 Owners)

The small dataset corresponds to the default Spring PetClinic dataset. Under this configuration, the dataset easily fits in memory and database access costs are low.

### Observations

- Energy per request is similar across all variants.
- Differences between baseline, payload-efficient, and data-access-optimized variants are small.
- Latency values are comparable across variants.

These results indicate that under low data volume and low load, fixed execution overheads dominate energy consumption.

---

## Medium Dataset Results (1,000 Owners)

The medium dataset exposes scaling effects by increasing response size and backend processing effort.

### Payload Efficiency (D1)

When evaluated with the medium dataset, the payload-efficient variant consistently reduced energy per request relative to the baseline.

- Energy per request decreased by approximately **1.9\%** on average.
- Both p95 and average latency were reduced.
- Functional behavior remained unchanged, aside from response representation.

This demonstrates that payload size becomes a significant energy factor as data volume increases.

---

### Data Access Efficiency (D4)

The data-access-optimized variant eliminated an N+1 query pattern while preserving the same API interface and response structure as the baseline.

- Energy per request remained comparable to the baseline.
- Latency metrics showed no substantial improvement under the tested workload.
- No energy savings were observed at 10 requests per second.

These results indicate that data access optimizations may not translate into immediate energy reductions under low to moderate load, even though they improve query structure.

---

## Cross-Dataset Comparison

Comparing results across dataset sizes highlights the **context-dependent nature** of energy-aware API design:

- For small datasets, energy differences between variants are negligible.
- For larger datasets, payload-oriented design choices yield measurable energy benefits.
- Backend data access optimizations may require higher concurrency or larger working sets to produce observable energy savings.

These observations reinforce the EAD-API premise that the effectiveness of design dimensions depends on workload and data characteristics.

---

## Raw Data and Aggregation

All raw measurement results are stored as CSV files and include:

- Variant identifier
- Dataset size
- Energy consumption (J)
- Requests completed
- Energy per request
- Latency metrics (p95 and average)

Aggregation scripts used to compute averages and summary statistics are provided in the companion repository.

---

## Notes on Interpretation

The results reported here reflect **CPU package energy** measured at the host level. They do not isolate energy consumption of individual components (e.g., database vs application logic). As such, results should be interpreted as end-to-end effects of API design choices under controlled conditions.

---

## Next Steps

The final page provides step-by-step instructions for replicating the experiments, including environment setup, dataset generation, and execution of measurement scripts.
