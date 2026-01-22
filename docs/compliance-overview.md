# Table of Contents

- [Purpose of Compliance Overview](#purpose-of-compliance-overview)  
- [Basic Principle](#basic-principle)  
- [Why Compliance is Built-In](#why-compliance-is-built-in)  
- [Supported Standards](#supported-standards)  
- [CIS Coverage](#cis-coverage)  
- [NIST Coverage](#nist-coverage)  
- [ISO 27001 Coverage](#iso-27001-coverage)  
- [Mapping Approach](#mapping-approach)  
- [Evidence Generation](#evidence-generation)  
- [Compliance in CI](#compliance-in-ci)  
- [Relation to compliance/*](#relation-to-compliance)  
- [Relation to Repository Structure](#relation-to-repository-structure)  
- [Compliance Approach Invariants](#compliance-approach-invariants)  
- [Compliance Non-Goals](#compliance-non-goals)  
- [Summary](#summary)

---

# Purpose of Compliance Overview

`compliance-overview.md` describes the **approach to compliance as an architectural property** of the Sovereign AI Kubernetes platform.

This document answers:  
- Why compliance is built into the platform  
- Which standards are covered  
- How controls are traced  
- How compliance evidence is generated

Compliance here is not a report.  
It is a **byproduct of correct architecture**.

---

# Basic Principle

Compliance is not added afterward.  
It **emerges automatically** if the platform is designed correctly.

Approach:  
- First architecture  
- Then security invariants  
- Then policies and audit  
- Only then reports

Documentation without enforcement is not considered compliance.

---

# Why Compliance is Built-In

Reasons:  
- Sovereign and regulated environments do not allow post-hoc checks  
- Requirements must be enforceable, not just documented  
- Human factor is eliminated

Compliance without automation:  
- Does not scale  
- Is not reproducible  
- Fails audits

---

# Supported Standards

The platform targets:  

- **CIS Kubernetes Benchmark**  
- **NIST (800-53 / 800-190)**  
- **ISO/IEC 27001**

Standards are not implemented directly.  
They are **mapped to architectural controls**.

---

# CIS Coverage

CIS is used as:  
- Technical baseline  
- Source of control requirements  
- Hardening reference

CIS controls:  
- Implemented via policies  
- Verified automatically  
- Do not require manual validation

---

# NIST Coverage

NIST is used as:  
- Risk management model  
- Governance framework  
- Basis for sovereign requirements

NIST controls:  
- Traced to security layers  
- Expressed through policies and audit  
- Used for risk-based analysis

---

# ISO 27001 Coverage

ISO is considered as:  
- A management and process standard  
- A layer on top of technical controls

The platform:  
- Provides technical evidence  
- Reduces manual procedures  
- Simplifies certification

---

# Mapping Approach

Mapping approach:  

- Standard → Control  
- Control → Architectural element  
- Element → Policy / Audit / Runtime

Each control:  
- Has a source  
- Has an implementation  
- Has verifiable evidence

Mapping is stored in code, not in tables.

---

# Evidence Generation

Evidence is generated automatically:  
- Audit logs  
- Policy evaluation results  
- CI compliance checks  
- Runtime alerts

Evidence is:  
- Reproducible  
- Linked to Git version  
- Ready for external audit

---

# Compliance in CI

CI:  
- Validates compliance with policies  
- Checks control coverage  
- Generates reports

If CI is green, the cluster meets the declared compliance level.

---

# Relation to compliance/*

The `compliance/` directory contains:  
- Standard mappings  
- Implemented controls  
- Report templates

Each file:  
- Is linked to an architectural decision  
- Has an enforcement mechanism

---

# Relation to Repository Structure

Compliance is not separate.  
It permeates:  

- Policies  
- Runtime security  
- Audit  
- CI

See details: `docs/repository-structure.md`

---

# Compliance Approach Invariants

Never violated:  
- No manual approvals  
- No declarations without enforcement  
- No environment without audit  
- No compliance without Git

---

# Compliance Non-Goals

The platform is not intended for:  
- Manual checklist completion  
- One-off certifications  
- "Paper" compliance  
- Ad-hoc audits

---

# Summary

Compliance in Sovereign AI:  
- Emerges from the architecture  
- Is automatically verifiable  
- Scales with the platform  
- Withstands external audit

If a system can be broken without trace, it is not compliant.

