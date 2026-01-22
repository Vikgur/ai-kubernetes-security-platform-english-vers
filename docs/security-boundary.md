# Table of Contents

- [Purpose of Security Boundary](#purpose-of-security-boundary)
- [Core Principle](#core-principle)
- [Trust Boundaries](#trust-boundaries)
- [Kubernetes vs Underlying Infrastructure](#kubernetes-vs-underlying-infrastructure)
  - [Trusted](#trusted)
  - [Untrusted](#untrusted)
- [Control Plane Boundary](#control-plane-boundary)
  - [Trusted](#trusted-1)
  - [Security Properties](#security-properties)
- [Data Plane Boundary](#data-plane-boundary)
  - [Untrusted](#untrusted-1)
  - [Mitigations](#mitigations)
- [Human Identities vs Workload Identities](#human-identities-vs-workload-identities)
  - [Human Identities](#human-identities)
  - [Workload Identities](#workload-identities)
- [Namespace Boundary](#namespace-boundary)
- [CI/CD Boundary](#cicd-boundary)
- [Admission Boundary](#admission-boundary)
- [Runtime Boundary](#runtime-boundary)
- [Connection with Threat Model](#connection-with-threat-model)
- [Connection with Policy Strategy](#connection-with-policy-strategy)
- [Security Boundary Invariants](#security-boundary-invariants)
- [Non-Goals of the Document](#non-goals-of-the-document)
- [Conclusion](#conclusion)

---

# Purpose of Security Boundary

`security-boundary.md` formalizes **trust and responsibility boundaries** in the Sovereign AI Kubernetes platform.

The document answers:
- what can be trusted and under which conditions;
- where trust boundaries exist;
- which entities are considered hostile;
- where Kubernetes responsibility ends.

README describes the platform.  
This document defines the **trust model**.

---

# Core Principle

By default, **nothing is trusted**.

Trust must be:
- explicit;
- minimal;
- verifiable;
- revocable.

Lack of formalized boundaries is considered an architectural flaw.

---

# Trust Boundaries

The platform defines the following trust boundaries:

1. Kubernetes ↔ underlying infrastructure  
2. Control plane ↔ data plane  
3. Human identities ↔ workload identities  
4. Namespace ↔ namespace  
5. CI/CD ↔ runtime cluster  

Each boundary is:
- controlled;
- logged;
- protected by policies.

---

# Kubernetes vs Underlying Infrastructure

## Trusted

- physical and virtual infrastructure;
- hardened OS and container runtime;
- bootstrap scripts and Ansible hardening;
- cloud primitives (VPC, firewall, IAM).

## Untrusted

- everything running inside Kubernetes as a workload;
- any pod;
- any container;
- any namespace except system.

Kubernetes **does not extend infrastructure trust**.  
It isolates it from workloads.

---

# Control Plane Boundary

## Trusted

- API server;
- scheduler;
- controller-manager;
- etcd (when properly hardened).

## Security Properties

- strict RBAC;
- audit logging;
- admission enforcement;
- minimal admin access.

Control plane compromise:
- is considered critical;
- requires incident response;
- must not occur silently.

---

# Data Plane Boundary

The data plane is considered **hostile-by-default**.

## Untrusted

- all user workloads;
- AI training and inference containers;
- third-party images;
- transient jobs.

## Mitigations

- namespace isolation;
- network policies;
- runtime enforcement;
- resource limits;
- behavior detection.

The data plane never receives implicit trust.

---

# Human Identities vs Workload Identities

## Human Identities

- SRE;
- DevSecOps;
- platform operators;
- auditors.

Properties:
- minimal RBAC;
- short-lived credentials;
- full audit;
- break-glass as an exception.

## Workload Identities

- ServiceAccounts;
- pod-bound identities;
- automated tokens.

Properties:
- strictly namespace-scoped;
- no cross-access;
- no cluster-admin;
- subject to admission and policy engines.

Human ≠ workload.  
Their privileges never overlap.

---

# Namespace Boundary

Namespace is the basic isolation unit, but **not a security perimeter**.

Properties:
- logical isolation;
- enforcement via policies;
- network isolation is mandatory.

Without NetworkPolicy, a namespace is considered unsafe.

---

# CI/CD Boundary

CI/CD:
- is considered external to the cluster;
- has no persistent access;
- interacts via declarative interfaces.

Git is the single source of truth.  
The cluster is a passive consumer.

---

# Admission Boundary

Admission is the key control point.

Here:
- input is untrusted;
- requirements are formalized;
- the final decision on acceptance is made.

All objects:
- are considered potentially malicious;
- are validated by policies;
- are either accepted or rejected.

---

# Runtime Boundary

Runtime:
- does not trust admission;
- does not trust the image;
- does not trust the process.

Any behavior outside the model:
- is logged;
- triggers alerts;
- can be blocked.

---

# Connection with Threat Model

Trust boundaries serve as the basis for:
- STRIDE model;
- attack scenarios;
- mitigations.

Details: `docs/kubernetes-threat-model.md`

---

# Connection with Policy Strategy

All boundaries:
- are expressed in policy-as-code;
- are enforced automatically;
- are validated in CI.

Details: `docs/policy-strategy.md`

---

# Security Boundary Invariants

Never violated:
- workloads cannot be trusted;
- humans are constrained by RBAC;
- control is always centralized;
- manual exceptions are forbidden;
- audit is mandatory.

---

# Non-Goals of the Document

The document does not describe:
- specific YAML manifests;
- tool-specific configurations;
- operational runbooks.

It defines the **trust model**, not the implementation.

---

# Conclusion

Security boundary:
- defines the security architecture;
- provides the foundation for threat modeling;
- eliminates implicit trust;
- makes Kubernetes a manageable security system.

In Sovereign AI, trust is a resource.  
It is distributed very sparingly.

