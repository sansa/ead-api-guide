---
title: "EAD-API Replication Package"
nav_order: 0
permalink: /
---

# EAD-API: Energy-Aware Design for APIs  
### Replication and Companion Website

This website accompanies the research paper:

> **EAD-API: A Multidimensional Framework for Energy-Aware API Design**

The purpose of this site is to support **replication, transparency, and reuse** of the experimental artifacts used in the paper. It documents the experimental setup, dataset generation process, API variants, and measurement workflow required to reproduce the reported results.

---

## 📄 Paper Overview

The paper introduces **EAD-API**, a design framework that makes the energy implications of API design decisions explicit. Rather than focusing on implementation-level optimizations, EAD-API highlights how common API design choices—such as payload structure and data access patterns—affect energy consumption at runtime.

The framework is empirically evaluated through a controlled experiment using the **Spring PetClinic REST API**, host-level energy measurements via **Intel RAPL**, and a constant-arrival-rate workload.

---

## 🔬 What This Repository Contains

This companion site provides access to:

- Instructions for setting up the experimental environment
- Source code for API variants used in the study
- Dataset generation scripts (small and medium datasets)
- Workload definitions and measurement scripts
- Raw and aggregated experimental results
- Links to all associated GitHub repositories

All artifacts are provided to enable independent replication of the experiments.

---

## 📦 Associated Repositories

The experimental artifacts described in the paper are distributed across the following GitHub repositories:

- **Subject system (API variants):**  
  Fork of Spring PetClinic REST containing all API variants used in the study  
  https://github.com/sansa/spring-petclinic-rest

- **Measurement and workload scripts:**  
  Repository containing the RAPL-based energy measurement scripts and k6 workloads  
  https://github.com/sansa/vf-rapl

These repositories are referenced throughout this site and together constitute the complete replication package.

--- 
## 🧪 Experimental Setup (At a Glance)

- **Subject system:** Spring PetClinic REST API
- **API variants:** Baseline, payload-efficient variant, data-access-optimized variant
- **Deployment:** REST API on bare-metal Ubuntu; PostgreSQL in Docker
- **Workload:** k6 constant-arrival-rate (10 requests/sec)
- **Energy measurement:** Intel RAPL (CPU package energy)
- **Datasets:** Default PetClinic dataset and scaled dataset (1,000 owners)

Full details are provided in the sections linked below.

---

## 📂 Contents

1. [Overview and Framework]({{ "/overview/" | relative_url }})
2. [Experimental Setup]({{ "/experimental-setup/" | relative_url }})
3. [API Variants]({{ "/api-variants/" | relative_url }})
4. [Dataset Generation]({{ "/datasets/" | relative_url }})
5. [Workload and Measurement]({{ "/measurement/" | relative_url }})
6. [Results and Data]({{ "/results/" | relative_url }})
7. [Replication Guide]({{ "/replication/" | relative_url }})

---

## ♻️ Reproducibility and Licensing

All materials are released for academic and research use. The authors welcome replication studies, extensions, and reuse of the framework and experimental artifacts. Please cite the corresponding paper when using this material in academic work.

---

## 📬 Contact

For questions regarding the experimental setup or replication process, please refer to the paper or open an issue in the corresponding repository.
