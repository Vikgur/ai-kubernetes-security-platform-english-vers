# Table of Contents

- [Purpose of Kubernetes Threat Model](#purpose-of-kubernetes-threat-model)  
- [Threat Modeling Approach](#threat-modeling-approach)  
- [Threat Actors](#threat-actors)  
  - [External Adversary](#external-adversary)  
  - [Malicious Workload](#malicious-workload)  
  - [Insider](#insider)  
  - [Supply Chain Attacker](#supply-chain-attacker)  
- [Attack Surfaces](#attack-surfaces)  
- [Control Plane Threats](#control-plane-threats)  
  - [API Server Abuse](#api-server-abuse)  
  - [etcd Compromise](#etcd-compromise)  
  - [Admission Bypass](#admission-bypass)  
- [Node and Workload Threats](#node-and-workload-threats)  
  - [Container Escape](#container-escape)  
  - [Privilege Escalation Within Cluster](#privilege-escalation-within-cluster)  
  - [Lateral Movement](#lateral-movement)  
- [Supply Chain Threats](#supply-chain-threats)  
  - [Malicious Images](#malicious-images)  
  - [Dependency Poisoning](#dependency-poisoning)  
- [CI/CD Threats](#cicd-threats)  
- [Runtime Threats](#runtime-threats)  
  - [Post-Deployment Abuse](#post-deployment-abuse)  
- [Cross-Cutting Threats](#cross-cutting-threats)  
  - [Misconfiguration](#misconfiguration)  
- [Relation to Architecture](#relation-to-architecture)  
- [Relation to Other Documents](#relation-to-other-documents)  
- [Threat Model Non-Goals](#threat-model-non-goals)  
- [Summary](#summary)

---

# Purpose of Kubernetes Threat Model

`kubernetes-threat-model.md` describes the **Kubernetes threat model for a Sovereign AI-level platform**.

This document answers:  
- Who are the potential attackers  
- What attack surfaces exist  
- Which threats are considered systemic  
- Which controls cover them

This is a **platform threat model**, not an application-specific one.

---

# Threat Modeling Approach

An architecture-oriented approach is used:  
- From trust boundaries  
- From system roles  
- From workload lifecycle

STRIDE is applied as a supporting model,  
but the key is **platform-level thinking**.

---

# Threat Actors

## External Adversary

- External attacker  
- Access via exposed services  
- Exploitation of application vulnerabilities

## Malicious Workload

- Compromised container  
- Malicious image  
- Poisoned AI workload

## Insider

- Developer  
- Operator  
- CI/CD actor

## Supply Chain Attacker

- Compromised registry  
- Substituted image  
- Poisoned dependency

---

# Attack Surfaces

Key attack surfaces:  
- Kubernetes API  
- Admission pipeline  
- Container runtime  
- Network between pods  
- CI/CD integrations  
- Image registry and metadata

Each surface is considered potentially hostile.

---

# Control Plane Threats

## API Server Abuse

Threats:  
- RBAC escalation  
- Unauthorized modifications  
- Admission bypass

Mitigated by:  
- Strict RBAC  
- Audit logging  
- Admission policies  
- Minimal admin access

---

## etcd Compromise

Threats:  
- Secret leakage  
- State modification  
- Full cluster control

Mitigated by:  
- Hardening  
- Access restriction  
- Encryption  
- No direct access from the data plane

---

## Admission Bypass

Threats:  
- Deployment of unsafe objects  
- Bypassing policy engines

Mitigated by:  
- Deny-by-default  
- Mandatory admission controllers  
- Prohibition of direct-to-node deployment

See: `docs/admission-control.md`

---

# Node and Workload Threats

## Container Escape

Threats:  
- Escaping the container  
- Access to host OS  
- Lateral movement

Mitigated by:  
- Seccomp  
- AppArmor  
- Dropping capabilities  
- Non-root execution

---

## Privilege Escalation Within Cluster

Threats:  
- ServiceAccount misuse  
- Token exfiltration  
- Cross-namespace access

Mitigated by:  
- Namespace isolation  
- Minimal RBAC  
- Admission validation  
- Runtime detection

---

## Lateral Movement

Threats:  
- Movement between pods  
- Attacks via DNS or services

Mitigated by:  
- Default deny NetworkPolicy  
- Namespace isolation  
- Egress control

---

# Supply Chain Threats

## Malicious Images

Threats:  
- Backdoored containers  
- Poisoned ML frameworks  
- Hidden miners

Mitigated by:  
- Allowlist registry  
- Signed images only  
- Provenance checks

See: `docs/supply-chain-security.md`

---

## Dependency Poisoning

Threats:  
- Malicious dependencies  
- Substitution of build artifacts

Mitigated by:  
- SBOM  
- Provenance  
- Immutable images

---

# CI/CD Threats

Threats:  
- Pipeline compromise  
- Injection of malicious manifests  
- Policy substitution

Mitigated by:  
- Policy validation in CI  
- Separation of duties  
- GitOps model

---

# Runtime Threats

## Post-Deployment Abuse

Threats:  
- Execution of unexpected processes  
- Cryptomining  
- Data exfiltration

Mitigated by:  
- Runtime detection  
- Behavior-based alerts  
- Audit trails

See: `docs/runtime-security.md`

---

# Cross-Cutting Threats

## Misconfiguration

Threats:  
- Permissive policies  
- Open namespaces  
- Lack of restrictions

Mitigated by:  
- Deny-by-default  
- Automated validation  
- Compliance enforcement

---

# Relation to Architecture

All threats:  
- Are mapped to security layers  
- Are covered by policy and runtime controls  
- Are validated in CI

Architecture defines the mitigation, not vice versa.

---

# Relation to Other Documents

- `README.md` — scope and positioning  
- `security-boundary.md` — trust model  
- `admission-control.md` — enforcement  
- `runtime-security.md` — execution-time protection  
- `supply-chain-security.md` — supply chain

---

# Threat Model Non-Goals

The model does not cover:  
- Specific CVEs  
- Tool-specific exploits  
- Pentest reports

This is a **structural threat model**, not a checklist.

---

# Summary

The threat model:  
- Defines architectural decisions  
- Shapes policy strategy  
- Specifies runtime protection requirements  
- Eliminates blind spots

In Sovereign AI, attacks are considered inevitable.  
The architecture ensures they remain controllable.
