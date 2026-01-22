# Table of Contents

- [Purpose of Admission Control](#purpose-of-admission-control)  
- [Role of Admission in Architecture](#role-of-admission-in-architecture)  
- [Types of Admission Controls](#types-of-admission-controls)  
- [Validating Admission](#validating-admission)  
  - [Purpose](#purpose)  
  - [Characteristics](#characteristics)  
  - [Blocking Examples](#blocking-examples)  
- [Mutating Admission](#mutating-admission)  
  - [Purpose](#purpose-1)  
  - [Characteristics](#characteristics-1)  
  - [Mutation Examples](#mutation-examples)  
- [Always Blocked](#always-blocked)  
- [Conditionally Allowed](#conditionally-allowed)  
- [Relation to Pod Security Standards](#relation-to-pod-security-standards)  
- [Admission and Policy Engines](#admission-and-policy-engines)  
  - [OPA Gatekeeper](#opa-gatekeeper)  
  - [Kyverno](#kyverno)  
- [Admission and Environments](#admission-and-environments)  
- [Relation to Runtime Security](#relation-to-runtime-security)  
- [Admission Control Invariants](#admission-control-invariants)  
- [Admission Non-Goals](#admission-non-goals)  
- [Summary](#summary)

---

# Purpose of Admission Control

`admission-control.md` describes the **enforcement contour at the entry point of the Kubernetes cluster**.

This document answers:  
- Which objects are allowed into the cluster  
- Which are unconditionally denied  
- Which can be modified  
- Where the boundary lies between admission and runtime protection

Admission is the **first and mandatory line of defense**.

---

# Role of Admission in Architecture

Admission sits between:  
- The API request  
- The actual creation of the object

Here:  
- Input is considered untrusted  
- Policies are enforced strictly  
- Decisions are deterministic

Admission does not log intentions.  
It either allows or denies.

---

# Types of Admission Controls

Two classes are used:

- Validating Admission  
- Mutating Admission  

They complement each other and do not overlap in responsibility.

---

# Validating Admission

## Purpose

- Strict compliance checks  
- Block unsafe objects  
- Protect architectural invariants

## Characteristics

- Does not modify the object  
- Returns allow or deny  
- Used for non-negotiable rules

## Blocking Examples

Always blocked:  
- Privileged containers  
- hostPath and hostNetwork without explicit permission  
- Running as root  
- Missing required labels  
- Missing resource limits  
- Attempts to bypass the baseline

Validating admission is the **hard-fail point**.

---

# Mutating Admission

## Purpose

- Automatically bring objects to a compliant state  
- Reduce likelihood of human error  
- Standardize configurations

## Characteristics

- Modifies the object before validation  
- Operates within allowed boundaries  
- Does not weaken security

## Mutation Examples

Conditionally allowed:  
- Adding a seccomp profile  
- Setting readOnlyRootFilesystem  
- Normalizing securityContext  
- Adding standard labels

Mutating admission **never turns deny into allow**.

---

# Always Blocked

Without exceptions:  
- Privileged pods  
- host PID / IPC / Network  
- Cluster-admin for workloads  
- Bypassing Pod Security Standards  
- Images from unapproved registries  
- Unsigned images

These rules have no overrides.

---

# Conditionally Allowed

Allowed when conditions are met:  
- Non-root with user override  
- Additional capabilities with explicit allow  
- Network egress with policy in place  
- Elevated permissions in system namespaces

Conditions are always expressed in code.

---

# Relation to Pod Security Standards

Pod Security Standards (PSS) are used as:  
- Minimal baseline  
- Entry filter  
- Upstream-compatible mechanism

Admission:  
- Extends PSS  
- Closes gaps  
- Enforces stricter rules

PSS is necessary but not sufficient.

---

# Admission and Policy Engines

## OPA Gatekeeper

Used for:  
- Strict validation  
- Compliance-critical denials  
- Cluster-wide rules

## Kyverno

Used for:  
- Mutation  
- Guardrails  
- Developer-facing defaults

Roles are clearly separated.

---

# Admission and Environments

Admission:  
- Is identical across all environments  
- Has no dev-mode  
- Is not relaxed in stage

If an object fails admission in dev, it is not production-ready.

---

# Relation to Runtime Security

Admission:  
- Prevents known risks  
- Does not control behavior

Runtime:  
- Covers execution-time threats  
- Responds to post-deploy attacks

See details: `docs/runtime-security.md`

---

# Admission Control Invariants

Never violated:  
- Deny-by-default  
- No manual overrides  
- Deterministic decisions  
- Audit of every denial  
- Git as the source of policies

---

# Admission Non-Goals

Admission does not address:  
- Runtime exploits  
- Kernel zero-days  
- Behavioral anomalies  
- Incident response

It secures the **entry point**, not execution.

---

# Summary

Admission control:  
- Turns architecture into enforceable rules  
- Blocks unsafe deployments  
- Standardizes configurations  
- Serves as the foundation of platform security

In Sovereign AI, nothing enters the cluster  
without passing admission.
