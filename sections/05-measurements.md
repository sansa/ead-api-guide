---
title: "Workload and Measurement"
nav_order: 5
permalink: /measurement/
---

# Workload and Measurement

This page describes the workload configuration and energy measurement methodology used in the empirical evaluation of the EAD-API framework. The measurement approach is designed to be reproducible, low-overhead, and transparent.

---

## Workload Generator

Client load was generated using **k6**, an open-source load testing framework. k6 was selected for its ability to enforce precise request arrival rates independent of server response time.

### Workload Model

A **constant-arrival-rate** workload model was used to ensure that each API variant was evaluated under identical request pressure.

- **Request type:** HTTP GET
- **Target endpoint:** One endpoint per run
- **Arrival rate:** 10 requests per second
- **Executor:** `constant-arrival-rate`

This configuration ensures that differences in measured energy consumption are attributable to server-side behavior rather than variations in client-side request generation.

---

## Workload Parameters

All experiments used the following fixed parameters:

- **Request rate:** 10 requests per second
- **Warm-up duration:** 30 seconds
- **Measurement duration:** 180 seconds
- **Repetitions:** 3 runs per variant and dataset

The warm-up phase allows the JVM, database, and system caches to stabilize before measurements are collected.

---

## Measurement Metrics

For each experimental run, the following metrics were recorded:

- **Total energy consumption (J):**  
  CPU package energy consumed during the measurement window.
- **Requests completed:**  
  Number of successful HTTP requests.
- **Energy per request (J/request):**  
  Total energy divided by number of completed requests.
- **95th-percentile latency (p95):**  
  Tail latency of HTTP request durations.
- **Average latency:**  
  Mean HTTP request duration.

Energy per request is the primary metric used for cross-variant comparison.

---

## Energy Measurement Method

Energy consumption was measured using **Intel RAPL** via the Linux powercap interface: `/sys/class/powercap/intel-rapl:0/energy_uj`


This interface exposes cumulative CPU package energy in microjoules. Energy consumption for a run is computed as the difference between the counter value at the start and end of the measurement window.

### Measurement Scope

- Energy measurements reflect **host CPU package energy**.
- The REST API runs directly on the host operating system.
- PostgreSQL runs inside a Docker container on the same host.
- Energy consumed by the database is included only insofar as it contributes to host CPU utilization.

Energy consumed by the workload generator itself is not explicitly isolated but remains constant across runs.

---

## Measurement Automation

All measurements were automated using a custom Python script that:

1. Executes a warm-up workload.
2. Reads the initial RAPL energy counter.
3. Runs the measurement workload via k6.
4. Reads the final RAPL energy counter.
5. Parses k6 summary output.
6. Writes all metrics to a CSV file.

This automation minimizes human error and ensures consistent execution across runs and variants.

---

## Noise Reduction and Control

To reduce the impact of measurement noise:

- Experiments were run on a dedicated machine.
- No other user workloads were present during measurement.
- Each configuration was executed three times.
- Results are reported as averages across runs.

These controls follow established best practices for empirical energy measurement.

---

## Reproducibility Notes

The k6 workload definition, measurement script, and example command invocations are provided in the companion repository. Paths, parameters, and environment variables are documented to allow exact reproduction of the workload and measurement process.

---

## Next Steps

The following page presents the collected results and describes how the raw measurement data is organized and interpreted.
