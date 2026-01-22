# Table of Contents

- [Purpose of Runtime Security](#purpose-of-runtime-security)
- [Core Principle](#core-principle)
- [Classes of Runtime Threats](#classes-of-runtime-threats)
  - [Unexpected Process Execution](#unexpected-process-execution)
  - [Privilege Escalation](#privilege-escalation)
  - [Container Escape](#container-escape)
  - [Network Abuse](#network-abuse)
  - [Resource Abuse](#resource-abuse)
- [Role of Seccomp](#role-of-seccomp)
  - [Purpose](#purpose)
  - [Rationale](#rationale)
- [Role of AppArmor](#role-of-apparmor)
  - [Purpose](#purpose-1)
  - [Rationale](#rationale-1)
- [Falco-like Detection Model](#falco-like-detection-model)
  - [Approach](#approach)
  - [Detectable Events](#detectable-events)
- [Alert vs Response](#alert-vs-response)
  - [Alerting](#alerting)
  - [Response](#response)
- [Runtime and Audit](#runtime-and-audit)
- [Connection with Threat Model](#connection-with-threat-model)
- [Connection with Observability Security](#connection-with-observability-security)
- [Runtime Protection Invariants](#runtime-protection-invariants)
- [Non-Goals of Runtime Security](#non-goals-of-runtime-security)
- [Conclusion](#conclusion)

---

# Purpose of Runtime Security

`runtime-security.md` describes **the Kubernetes protection layer after workload deployment**.

The document answers:
- which threats exist at runtime;
- why admission alone is insufficient;
- how process behavior is constrained;
- how attacks and anomalies are detected.

Runtime security is the **last line of defense**.

---

# Core Principle

After deployment:
- objects are considered potentially compromised;
- configuration alone no longer guarantees security;
- control must be behavioral.

Runtime protection does not trust:
- admission;
- the image;
- the developer;
- past checks.

---

# Classes of Runtime Threats

## Unexpected Process Execution

- launching shells;
- debugging tools;
- third-party binaries.

---

## Privilege Escalation

- attempts to gain root;
- modifying capabilities;
- interaction with the host.

---

## Container Escape

- kernel system calls;
- access to host filesystem;
- interaction with the container runtime.

---

## Network Abuse

- unexpected outbound connections;
- lateral movement;
- data exfiltration.

---

## Resource Abuse

- cryptomining;
- fork bombs;
- resource-based DoS.

---

# Role of Seccomp

## Purpose

Seccomp:
- restricts system calls;
- reduces kernel attack surface;
- applied before process start.

## Rationale

- most applications use a small subset of syscalls;
- default denial reduces exploit impact;
- resilient to misconfiguration.

Seccomp is **predictive protection**, not detection.

---

# Role of AppArmor

## Purpose

AppArmor:
- restricts filesystem access;
- controls network operations;
- defines allowed behavior models.

## Rationale

- syscall filtering alone is insufficient;
- resource and path control is needed;
- profiles can be workload-specific.

AppArmor is **policy enforcement at the kernel level**.

---

# Falco-like Detection Model

## Approach

The model uses:
- monitoring system events;
- comparing with expected behavior;
- generating alerts on deviations.

## Detectable Events

- exec inside the container;
- binary modification;
- access to sensitive paths;
- network anomalies.

Detection focuses on **behavior**, not signatures.

---

# Alert vs Response

## Alerting

- signals a violation;
- does not stop the system;
- used for investigations.

Applied for:
- early detection;
- behavior model training;
- forensic analysis.

---

## Response

- active intervention;
- restriction or termination of the workload;
- namespace isolation.

Used only:
- with high confidence;
- following strict runbooks;
- without false positives.

Response is always secondary to alerting.

---

# Runtime and Audit

All runtime events are:
- logged;
- correlated;
- available as compliance evidence.

Runtime without audit is not considered protection.

---

# Connection with Threat Model

Runtime addresses threats:
- post-deploy abuse;
- unknown attack vectors;
- zero-day behavior.

Details: `docs/kubernetes-threat-model.md`

---

# Connection with Observability Security

Runtime security:
- provides signals;
- leverages metrics and logs;
- integrates with alerting.

Observability is the transport.  
Runtime security is the meaning.

---

# Runtime Protection Invariants

Never violated:
- workloads are not trusted;
- behavior is constrained;
- events are logged;
- manual intervention is minimized.

---

# Non-Goals of Runtime Security

Runtime does not fix:
- architectural mistakes;
- weak policies;
- missing admission controls;
- poor application design.

It compensates, but does not replace.

---

# Conclusion

Runtime security:
- assumes potential compromise;
- minimizes impact;
- ensures detection;
- closes the security lifecycle.

In Sovereign AI, security continues  
after deployment.
