# Table of Contents

- [Purpose of Policy Strategy](#purpose-of-policy-strategy)
- [Core Principle](#core-principle)
- [Types of Policies](#types-of-policies)
  - [Validation Policies](#validation-policies)
  - [Mutation Policies](#mutation-policies)
  - [Enforcement Policies](#enforcement-policies)
- [Where Policies Apply](#where-policies-apply)
- [OPA Gatekeeper vs Kyverno](#opa-gatekeeper-vs-kyverno)
  - [OPA Gatekeeper](#opa-gatekeeper)
  - [Kyverno](#kyverno)
  - [Why Not Just One Tool](#why-not-just-one-tool)
- [Policy Lifecycle](#policy-lifecycle)
- [Policy Versioning](#policy-versioning)
- [Rollout Strategy](#rollout-strategy)
- [Policy Drift and Its Prevention](#policy-drift-and-its-prevention)
- [Connection with Admission Control](#connection-with-admission-control)
- [Connection with Repository Structure](#connection-with-repository-structure)
- [Policy Strategy Invariants](#policy-strategy-invariants)
- [Non-Goals of the Strategy](#non-goals-of-the-strategy)
- [Conclusion](#conclusion)

---

# Purpose of Policy Strategy

`policy-strategy.md` describes the **strategy for applying security policies** in the Sovereign AI Kubernetes platform.

The document answers:
- what types of policies are used;
- where exactly they are applied;
- why multiple policy engines are used;
- how policies evolve without violating invariants.

The architecture sets the requirements.  
This document describes the **enforcement mechanism**.

---

# Core Principle

Policies are:
- mandatory;
- automatically enforceable;
- versioned;
- applied before workloads start.

Advisory policies do not exist.  
If a rule is not enforced — it does not exist.

---

# Types of Policies

## Validation Policies

Purpose:
- ensure compliance with requirements;
- prohibit unsafe configurations.

Examples:
- privileged containers;
- missing securityContext;
- absence of labels or annotations.

Outcome:
- object is either accepted or rejected.

---

## Mutation Policies

Purpose:
- automatically adjust objects to an acceptable state;
- reduce cognitive load on developers.

Examples:
- adding a seccomp profile;
- setting readOnlyRootFilesystem;
- normalizing labels.

Mutation never weakens security.

---

## Enforcement Policies

Purpose:
- enforce architectural invariants;
- prevent policy drift.

Examples:
- prohibiting baseline bypass;
- mandatory network policies;
- forbidding manual exceptions.

Enforcement is the final arbiter.

---

# Where Policies Apply

Policies are applied:
- at the admission stage;
- on every state change;
- consistently across all environments.

There is no “soft mode” for production.  
Dev and stage environments have the same strict requirements.

---

# OPA Gatekeeper vs Kyverno

Both tools are intentionally used.

## OPA Gatekeeper

Role:
- strict deny;
- formalization of non-negotiable rules;
- compliance enforcement.

Used for:
- architectural prohibitions;
- compliance-critical controls;
- cluster-wide invariants.

---

## Kyverno

Role:
- mutation;
- guardrails;
- developer-friendly policies.

Used for:
- auto-configuring securityContext;
- safe defaults;
- gradual tightening of requirements.

---

## Why Not Just One Tool

- OPA — strict, declarative, formal;
- Kyverno — flexible, user-friendly, Kubernetes-native.

Separation reduces:
- policy complexity;
- risk of errors;
- operational overhead.

---

# Policy Lifecycle

Policy lifecycle:

1. Design  
2. Review  
3. CI validation  
4. Dry-run (optional)  
5. Enforce  
6. Audit  
7. Iteration  

Each step is tracked in Git.

---

# Policy Versioning

Policies:
- are versioned as code;
- have a change history;
- are linked to compliance controls.

Breaking changes:
- are submitted as separate PRs;
- go through stage validation;
- are rolled out in a controlled manner.

---

# Rollout Strategy

Approach:
- baseline first;
- then strict enforcement;
- no environment-specific relaxations.

Rollback:
- only via Git;
- always preserves an audit trail.

---

# Policy Drift and Its Prevention

Drift is considered an incident.

Mitigated by:
- deny-by-default;
- GitOps model;
- prohibition of out-of-band changes;
- CI policy checks.

---

# Connection with Admission Control

The policy strategy is implemented via the admission pipeline.

Admission:
- enforcement point;
- single entry to the cluster;
- last chance to prevent violations.

Details: `docs/admission-control.md`

---

# Connection with Repository Structure

Policies are organized by domain:
- policy-engine/opa-gatekeeper;
- policy-engine/kyverno;
- environments/ for rollout.

The structure reflects **architecture**, not tools.

Details: `docs/repository-structure.md`

---

# Policy Strategy Invariants

Never violated:
- policies are the same across all environments;
- no manual exceptions;
- enforcement takes precedence over convenience;
- audit is mandatory.

---

# Non-Goals of the Strategy

The strategy does not allow:
- per-team custom rules;
- temporary relaxations;
- manual disabling of policies;
- security waivers outside of code.

---

# Conclusion

Policy strategy:
- turns security requirements into code;
- eliminates human error;
- ensures reproducibility;
- makes security scalable.

In Sovereign AI, policy is law.  
And it is enforced automatically.
