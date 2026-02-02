---
title: "Overview and Framework"
nav_order: 1
permalink: /overview/
---

# Overview and Framework

This page provides a concise overview of the **EAD-API (Energy-Aware Design for APIs)** framework and its role in the accompanying empirical study. For a full conceptual discussion and evaluation, readers are referred to the corresponding research paper.

---

## Motivation

Application Programming Interfaces (APIs) define the interaction boundaries of modern software systems. In service-oriented and microservice-based architectures, APIs are invoked at large scale and high frequency, making their design choices critical for performance, scalability, and long-term sustainability.

While prior work has demonstrated that software design decisions can influence energy consumption, energy costs often remain invisible during API design. As a result, energy efficiency is frequently treated as an operational or implementation-level concern rather than a first-class design consideration.

---

## EAD-API Framework

EAD-API is a design framework that structures **API-level design decisions** into explicit dimensions based on how they influence resource utilization and energy consumption. Rather than prescribing concrete optimizations, the framework supports systematic reasoning about energy-related trade-offs during API design.

Each dimension captures a class of design choices that affect different parts of the system stack, such as data transfer, computation, or backend access patterns. The framework is intentionally measurement-tool agnostic and can be applied independently of specific profiling or monitoring solutions.

---

## Evaluated Design Dimensions

The empirical study focuses on a subset of EAD-API dimensions that are commonly encountered in data-driven REST APIs:

- **Payload Efficiency (D1):**  
  Design choices that influence response size and structure, including the selection of returned fields, representation granularity, and use of alternative or reduced views.

- **Computation and Serialization Cost (D3):**  
  API design decisions that affect server-side processing and serialization overhead, such as object graph complexity and data transformation strategies.

- **Data Access Efficiency (D4):**  
  Choices related to database access patterns, including query structure, batching strategies, and avoidance of inefficient access patterns (e.g., N+1 queries).

These dimensions are not mutually exclusive and may interact in practice. EAD-API is intended to support reasoning across multiple dimensions simultaneously.

---

## Role in the Empirical Study

In the accompanying experiment, selected EAD-API dimensions are operationalized through minimal API variants of the Spring PetClinic REST application. Each variant isolates a single design dimension while preserving functional behavior.

Energy consumption and performance metrics are then measured under controlled workload conditions to assess how different API design choices affect energy usage in practice. The results demonstrate that the impact of EAD-API dimensions is context-dependent and influenced by workload characteristics and dataset size.

---

## Scope and Limitations

This framework is not intended as a comprehensive catalog of all energy-related API design decisions. Instead, it serves as a conceptual lens to make energy implications explicit and to guide empirical evaluation and design reasoning.

Additional dimensions and application contexts can be incorporated in future studies using the same framework.

---

## Next Steps

The following pages describe the experimental setup, API variants, datasets, and measurement workflow in detail to support replication of the study.
