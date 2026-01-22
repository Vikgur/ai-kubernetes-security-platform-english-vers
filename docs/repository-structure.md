# Table of Contents

- [Purpose of Repository Structure](#purpose-of-repository-structure)
- [Core Idea](#core-idea)
- [How to Read the Repository](#how-to-read-the-repository)
- [Where Enforcement vs Description](#where-enforcement-vs-description)
- [Role of Root Files](#role-of-root-files)
  - [README.md](#readmemd)
  - [Linters and Policy Config](#linters-and-policy-config)
- [Role of Key Directories](#role-of-key-directories)
  - [`cluster/`](#cluster)
  - [`rbac/`](#rbac)
  - [`network-security/`](#network-security)
  - [`runtime-security/`](#runtime-security)
  - [`supply-chain-security/`](#supply-chain-security)
  - [`policy-engine/`](#policy-engine)
  - [`observability-security/`](#observability-security)
  - [`multi-tenancy/`](#multi-tenancy)
  - [`compliance/`](#compliance)
  - [`threat-model/`](#threat-model)
  - [`environments/`](#environments)
  - [`ci/`](#ci)
  - [`scripts/`](#scripts)
  - [`docs/`](#docs)
- [Purpose of Directories and Files](#purpose-of-directories-and-files)
  - [README.md](#readmemd-1)  
  - [.gitignore](#gitignore)  
  - [.editorconfig](#editorconfig)  
  - [.yamllint](#yamllint)  
  - [.kube-linter.yaml](#kube-linteryaml)  
  - [.opa.yaml](#opa-yaml)  
  - [.trivyignore](#trivyignore)  
  - [.checkov.yaml](#checkovyaml)  
  - [cluster/](#cluster)  
    - [bootstrap/](#bootstrap)  
    - [baseline/](#baseline)  
    - [hardening/](#hardening)  
    - [README.md](#readmemd-2)  
  - [rbac/](#rbac)  
    - [base/](#base)  
    - [break-glass/](#break-glass)  
    - [README.md](#readmemd-3)  
  - [network-security/](#network-security)  
    - [cni/](#cni)  
    - [network-policies/](#network-policies)  
    - [dns-security.yaml](#dns-securityyaml)  
    - [README.md](#readmemd-4)
  - [runtime-security/](#runtime-security)  
    - [admission/](#admission)  
    - [pod-security/](#pod-security)  
    - [seccomp/](#seccomp)  
    - [apparmor/](#apparmor)  
    - [README.md](#readmemd-5)  
  - [supply-chain-security/](#supply-chain-security)  
    - [image-policy/](#image-policy)  
    - [sbom/](#sbom)  
    - [provenance/](#provenance)  
    - [README.md](#readmemd-6)  
  - [policy-engine/](#policy-engine)  
    - [opa-gatekeeper/](#opa-gatekeeper)  
      - [constraints/](#constraints)  
      - [templates/](#templates)  
    - [kyverno/](#kyverno)  
    - [rego/](#rego)  
    - [README.md](#readmemd-7)  
  - [observability-security/](#observability-security)  
    - [audit-logs/](#audit-logs)  
    - [falco/](#falco)  
    - [prometheus-rules/](#prometheus-rules)  
    - [README.md](#readmemd-8)
  - [multi-tenancy/](#multi-tenancy)  
  - [compliance/](#compliance)  
    - [cis/](#cis)  
    - [nist/](#nist)  
    - [iso27001/](#iso27001)  
    - [reports/](#reports)  
    - [README.md](#readmemd-9)  
  - [threat-model/](#threat-model)  
    - [STRIDE.md](#stride-md)  
    - [attack-scenarios.md](#attack-scenarios-md)  
    - [mitigations.md](#mitigations-md)  
    - [README.md](#readmemd-10)  
  - [environments/](#environments)  
    - [dev/](#dev)  
    - [stage/](#stage)  
    - [prod/](#prod)  
    - [base/](#base-1)  
    - [kustomization.yaml](#kustomization-yaml)  
    - [README.md](#readmemd-11)
  - [ci/](#ci)  
    - [kube-linter.yml](#kube-linteryml)  
    - [opa-policy-check.yml](#opa-policy-checkyml)  
    - [trivy-k8s.yml](#trivy-k8syml)  
    - [manifest-validate.yml](#manifest-validateyml)  
    - [compliance-report.yml](#compliance-reportyml)  
    - [README.md](#readmemd-12)  
  - [scripts/](#scripts)  
    - [apply-baseline.sh](#apply-baselinesh)  
    - [verify-cluster.sh](#verify-clustersh)  
    - [audit-dump.sh](#audit-dumpsh)  
    - [architecture_bootstrap.sh](#architecture_bootstrapsh)  
    - [README.md](#readmemd-13)  
  - [docs/](#docs)  
    - [architecture.md](#architecture-md)  
    - [security-boundary.md](#security-boundary-md)  
    - [kubernetes-threat-model.md](#kubernetes-threat-model-md)  
    - [policy-strategy.md](#policy-strategy-md)  
    - [admission-control.md](#admission-control-md)  
    - [runtime-security.md](#runtime-security-md)  
    - [supply-chain-security.md](#supply-chain-security-md)  
    - [compliance-overview.md](#compliance-overview-md)  
    - [repository-structure.md](#repository-structure-md)

---

# Purpose of Repository Structure

`repository-structure.md` explains **how to read, understand, and use** the Kubernetes platform security repository.

The repository is:
- not a set of YAML files;
- not a collection of best practices;
- not a demo.

It is an **operational security platform**, implemented as the minimally required codebase.

The repository:
- is read as architecture;
- is applied as a platform;
- is tested as a product.

---

# Core Idea

The repository reflects architecture, not environment.

Principles:
- immutable infrastructure;
- policy as code;
- deny by default;
- separation of concerns;
- enforcement > documentation.

If something is not in the repository — it does not exist in the platform.

---

# How to Read the Repository

Read **top-down by control levels**:

1. Architecture and trust boundaries (`docs/`)
2. Platform-level controls (`cluster/`, `rbac/`, `network-security/`)
3. Enforcement and policy engines (`policy-engine/`, `runtime-security/`)
4. Supply chain (`supply-chain-security/`)
5. Observability and detection (`observability-security/`)
6. Compliance as derivative (`compliance/`)
7. Environment overlays (`environments/`)
8. Automation (`ci/`, `scripts/`)

---

# Where Enforcement vs Description

- **Enforcement**: everything applied to the cluster
  - YAMLs, policies, webhooks, profiles, quotas
- **Documentation**: explains *why*, not *how*
  - `docs/`, `README.md` in directories

Documentation does not duplicate enforcement.  
Enforcement does not explain itself.

---

# Role of Root Files

## README.md

Entry point.  
Captures:
- philosophy;
- boundaries;
- target security model.

Read first.

---

## Linters and Policy Config

Files such as:
- `.yamllint`
- `.kube-linter.yaml`
- `.opa.yaml`
- `.checkov.yaml`
- `.trivyignore`

Define **quality gates**.  
No manifest is considered valid without passing these checks.

---

# Role of Key Directories

## `cluster/`

Core security of the control plane and core namespaces.

Handles:
- baseline hardening;
- Pod Security Standards;
- resource isolation;
- API-level hardening.

DevSecOps purpose:  
protect the cluster as a platform, not just an application environment.

---

## `rbac/`

Access model.

Handles:
- least privilege;
- separation of duties;
- break-glass scenarios.

DevSecOps purpose:  
access is part of the attack surface.

---

## `network-security/`

Network boundaries.

Handles:
- namespace isolation;
- default deny;
- DNS security.

DevSecOps purpose:  
every pod is isolated until proven otherwise.

---

## `runtime-security/`

Post-admission control.

Handles:
- admission enforcement;
- pod security context;
- seccomp / AppArmor.

DevSecOps purpose:  
even an allowed workload is not considered safe by default.

---

## `supply-chain-security/`

Artifact trust.

Handles:
- image trust;
- SBOM;
- provenance;
- CI → cluster linkage.

DevSecOps purpose:  
attacks start long before deployment.

---

## `policy-engine/`

Policy hub.

Handles:
- validation;
- mutation;
- enforcement.

Uses:
- OPA Gatekeeper;
- Kyverno.

DevSecOps purpose:  
policy is code, not a convention.

---

## `observability-security/`

Visibility and detection.

Handles:
- audit logs;
- runtime detection;
- security alerts.

DevSecOps purpose:  
an unseen attack is considered successful.

---

## `multi-tenancy/`

Team and tenant isolation.

Handles:
- tenant boundaries;
- quota enforcement;
- blast radius reduction.

DevSecOps purpose:  
an error from one team should not become a platform incident.

---

## `compliance/`

Compliance as a consequence.

Handles:
- mapping to CIS / NIST / ISO;
- evidence collection;
- audit readiness.

DevSecOps purpose:  
compliance is not added — it emerges.

---

## `threat-model/`

Threat model.

Handles:
- formalizing attacks;
- linking mitigations;
- architectural thinking.

DevSecOps purpose:  
without a threat model, security is declarative only.

---

## `environments/`

Environment overlays.

Handles:
- dev / stage / prod differences;
- controlled drift;
- reproducibility.

DevSecOps purpose:  
environments differ by configuration, not architecture.

---

## `ci/`

Security gates.

Handles:
- policy validation;
- static analysis;
- compliance reporting.

DevSecOps purpose:  
security is neither optional nor manual.

---

## `scripts/`

Operational actions.

Handles:
- bootstrap;
- verification;
- audit dump.

DevSecOps purpose:  
manual actions are formalized or forbidden.

---

## `docs/`

Architectural description.

Handles:
- explaining decisions;
- layer interconnections;
- onboarding senior-level engineers.

DevSecOps purpose:  
without architecture, control cannot scale.

---

# Purpose of Directories and Files

## README.md
Main entry point of the repository.  
Contains positioning, scope, non-goals, and links to `docs/`.  
DevSecOps purpose: rapid understanding of architecture and security policy.

## .gitignore
Git exclusion file.  
Defines which temporary, local, or sensitive files are excluded from the repository.  
DevSecOps purpose: prevent secret leaks and garbage in the codebase.

## .editorconfig
Code editor configuration.  
Enforces a consistent formatting style for YAML and other files.  
DevSecOps purpose: standardized codebase for the team and CI.

## .yamllint
YAML linter configuration.  
Checks syntax correctness and manifest style.  
DevSecOps purpose: prevent deployment errors due to invalid YAML.

## .kube-linter.yaml
Kube-linter configuration.  
Performs static analysis of Kubernetes manifests for best practices and security.  
DevSecOps purpose: detect misconfigurations and security anti-patterns before deployment.

## .opa.yaml
OPA (Gatekeeper) configuration for local checks.  
Allows policies as code to be validated during CI.  
DevSecOps purpose: enforce policy-as-code before reaching the cluster.

## .trivyignore
Trivy exclusion file.  
Specifies packages and vulnerabilities to ignore during container scans.  
DevSecOps purpose: focus on critical vulnerabilities, minimize false positives.

## .checkov.yaml
Checkov configuration for IaC and Kubernetes.  
Automatically checks security and compliance rules in manifests.  
DevSecOps purpose: early detection of infrastructure security violations.

## cluster/
Main directory for **cluster hardening and baseline security**.  
DevSecOps purpose: centralized management of platform security controls.

### bootstrap/
Initial cluster configuration stage.  
The `placeholder.yaml` file serves as a template for future bootstrap manifests.  
DevSecOps purpose: formalizes cluster initialization order and minimizes manual actions.

### baseline/
Baseline policies and settings for all namespaces and pods.  
- `pod-security-standards.yaml` — basic Pod Security Standards for cluster-wide enforcement.  
- `namespaces.yaml` — predefined namespaces with assigned labels and annotations.  
- `resource-quotas.yaml` — resource limits per namespace, preventing DoS via CPU/Memory consumption.  
DevSecOps purpose: ensure consistency and security at startup; enforceable baseline.

### hardening/
Hardening of control plane components.  
- `api-server.yaml` — API Server security settings (RBAC, auditing, TLS).  
- `controller-manager.yaml` — privilege restrictions and secure flags for controller-manager.  
- `scheduler.yaml` — secure scheduler configuration (priority, binding restrictions).  
- `etcd.yaml` — encryption, access control, and audit of storage.  
DevSecOps purpose: minimize control plane attack surface and ensure immutable security.

### README.md
Documentation for the `cluster/` directory.  
Explains the application order of bootstrap, baseline, and hardening manifests.  
DevSecOps purpose: rapid understanding of cluster control level and enforcement.

## rbac/
Directory for **managing access and permissions in the cluster**.  
DevSecOps purpose: centralized attack surface management via least privilege and separation of duties.

### base/
Baseline roles and bindings for all namespaces.  
- `roles.yaml` — defines roles with restricted permissions.  
- `rolebindings.yaml` — binds roles to users or groups.  
- `serviceaccounts.yaml` — configures service accounts for workloads.  
DevSecOps purpose: enforceable least privilege across the platform.

### break-glass/
Emergency access scenarios.  
- `break-glass-admin.yaml` — temporary elevated access for critical operations.  
DevSecOps purpose: minimize misuse risk, controlled via audit and limited duration.

### README.md
Documentation for the `rbac/` directory.  
Explains the structure of roles, bindings, and break-glass scenarios.  
DevSecOps purpose: quick overview of access policies and their enforcement.

## network-security/
Directory for **network isolation and cluster security**.  
DevSecOps purpose: control traffic between pods, namespaces, and external services.

### cni/
Network plugin (CNI) configuration.  
- `placeholder.yaml` — template for future CNI manifests.  
DevSecOps purpose: standardize network layer and prepare for enforcement integration.

### network-policies/
Policy manifests for network segmentation.  
- `default-deny.yaml` — blocks all unauthorized traffic by default.  
- `namespace-isolation.yaml` — isolates namespaces, preventing lateral movement.  
DevSecOps purpose: enforceable deny-by-default network perimeter, minimize blast radius.

### dns-security.yaml
Policies and settings for secure DNS resolution within the cluster.  
DevSecOps purpose: prevent DNS spoofing attacks and exfiltration.

### README.md
Documentation for the `network-security/` directory.  
Explains which policies are applied and how they interact with other security layers.  
DevSecOps purpose: understand network model and enforceable isolation.

## runtime-security/
Directory for **runtime protection of workloads after deployment**.  
DevSecOps purpose: enforce security at Pod and container level, minimize post-deploy attack risk.

### admission/
Admission webhooks for controlling incoming objects.  
- `validating-webhook.yaml` — checks object compliance with policies before creation.  
- `mutating-webhook.yaml` — automatically adds required security contexts and annotations.  
DevSecOps purpose: prevent policy violations before workloads start.

### pod-security/
Pod Security Standards enforcement.  
- `restricted.yaml` — strict restrictions for critical namespaces.  
- `baseline.yaml` — baseline restrictions for other workloads.  
DevSecOps purpose: immutable security context for pods, enforceable via code.

### seccomp/
Seccomp profiles for containers.  
- `default.json` — blocks unsafe system calls.  
DevSecOps purpose: minimize kernel-level attack surface.

### apparmor/
AppArmor profiles for containers.  
- `.gitkeep` — placeholder, structure ready for future profiles.  
DevSecOps purpose: prevent unauthorized actions inside containers.

### README.md
Documentation for `runtime-security/`.  
Explains the application order of admission, Pod Security, and kernel profiles.  
DevSecOps purpose: understand runtime enforcement and its role in overall platform security.

## supply-chain-security/
Директория для **обеспечения доверия к артефактам** перед деплоем в кластер.  
DevSecOps-смысл: гарантировать, что все образы и зависимости проверены, подписаны и имеют provenance.

### image-policy/
Политики допуска контейнерных образов.  
- `allowed-registries.yaml` — список доверенных registry.  
- `signed-images-only.yaml` — enforcement использования только подписанных образов.  
DevSecOps-смысл: prevent poisoned images и enforce immutable supply chain.

### sbom/
Хранение SBOM (Software Bill of Materials).  
- `.gitkeep` — placeholder, структура для автоматической генерации и хранения SBOM.  
DevSecOps-смысл: traceability компонентов и vulnerability management.

### provenance/
Хранение информации о происхождении артефактов.  
- `.gitkeep` — placeholder для future provenance metadata.  
DevSecOps-смысл: подтверждение источника и CI pipeline для каждого артефакта.

### README.md
Документация к supply-chain-security/.  
Объясняет модель доверия, применение политики образов, SBOM и provenance.  
DevSecOps-смысл: понимание и enforceable цепочки доверия CI → Deployment.

## policy-engine/
Директория для **централизованного управления политиками Kubernetes**.  
DevSecOps-смысл: все enforcement механизмы собраны как code, versioned и auditable.

### opa-gatekeeper/
OPA Gatekeeper для валидации ресурсов.  

#### constraints/
- `required-labels.yaml` — проверка обязательных label’ов на ресурсах.  
- `deny-privileged.yaml` — запрет запуска привилегированных контейнеров.  
DevSecOps-смысл: enforceable security policies на уровне cluster-wide admission.

#### templates/
- `required-labels.yaml` — шаблон constraint для новых namespace/teams.  
- `deny-privileged.yaml` — шаблон constraint для future workloads.  
DevSecOps-смысл: ускоряет rollout новых правил без ручного копирования.

### kyverno/
Kyverno для mutation и дополнительной валидации.  
- `enforce-security-context.yaml` — добавляет обязательные security context к pod’ам.  
- `mutate-seccomp.yaml` — автоматически назначает seccomp профили.  
DevSecOps-смысл: автоматизация и стандартизация security context, enforcement на этапе deploy.

### rego/
Папка для дополнительных Rego policy.  
- `.gitkeep` — placeholder для future custom rules.  
DevSecOps-смысл: возможность расширения OPA enforcement без изменения основного pipeline.

### README.md
Документация к policy-engine/.  
Объясняет, как применять, версионировать и комбинировать политики OPA и Kyverno.  
DevSecOps-смысл: единый источник истины по policy-as-code.

## observability-security/
Директория для **видимости и мониторинга безопасности кластера**.  
DevSecOps-смысл: обнаружение инцидентов и контроль compliance в реальном времени.

### audit-logs/
Настройка audit логирования Kubernetes.  
- `audit-policy.yaml` — определяет, какие события логируются и как.  
DevSecOps-смысл: полный audit trail для forensic и compliance целей.

### falco/
Runtime detection через Falco-подобные правила.  
- `placeholder.yaml` — шаблон для будущих security detection правил.  
DevSecOps-смысл: обнаружение подозрительных действий на уровне pod/host.

### prometheus-rules/
Alerting и метрики для безопасности.  
- `security-alerts.yaml` — правила Prometheus для оповещений о нарушениях security.  
DevSecOps-смысл: оперативное оповещение и интеграция с SIEM/incident response.

### README.md
Документация к observability-security/.  
Объясняет порядок настройки audit, detection и alerting.  
DevSecOps-смысл: быстрый обзор системы runtime visibility.

---

## multi-tenancy/
Директория для **изоляции арендаторов и команд внутри кластера**.  
DevSecOps-смысл: ограничение blast radius и защита ресурсов разных tenants.

- `tenant-isolation.yaml` — сетевые и namespace boundary политики для отдельных арендаторов.  
- `quota-enforcement.yaml` — ограничения ресурсов для каждого tenant.  
- `README.md` — документация по multi-tenancy.  
DevSecOps-смысл: enforceable tenant separation и ресурсные лимиты как код.

## compliance/
Директория для **встроенного соответствия стандартам безопасности и регуляторным требованиям**.  
DevSecOps-смысл: интегрированное compliance как архитектурное свойство, а не отдельная «бумажка».

### cis/
CIS Kubernetes Benchmark.  
- `mapping.md` — сопоставление controls с кластерной архитектурой.  
- `controls.yaml` — enforceable CIS controls manifests.  
DevSecOps-смысл: автоматизированная проверка и enforcement стандартов CIS.

### nist/
NIST controls для Kubernetes.  
- `mapping.md` — отображение NIST controls на реализацию в кластере.  
- `controls.yaml` — manifests для автоматического соответствия.  
DevSecOps-смысл: поддержка государственных и enterprise compliance требований.

### iso27001/
ISO/IEC 27001 mapping.  
- `mapping.md` — связь стандартов ISO с существующими security controls.  
DevSecOps-смысл: доказательство соответствия и интеграция в audit pipeline.

### reports/
Хранение результатов compliance сканирования.  
- `.gitkeep` — placeholder для future automated reports.  
DevSecOps-смысл: traceable evidence для auditors и CI/CD pipeline.

### README.md
Документация к compliance/.  
Объясняет, какие стандарты покрыты и как применять controls.  
DevSecOps-смысл: единый источник информации по встроенному compliance.

## threat-model/
Директория для **формализации угроз и mitigation стратегии на уровне платформы**.  
DevSecOps-смысл: понимание attack surface, приоритизация mitigations и интеграция с policy enforcement.

### STRIDE.md
Описание угроз по модели STRIDE (Spoofing, Tampering, Repudiation, Information disclosure, Denial of Service, Elevation of privilege).  
DevSecOps-смысл: систематизация возможных атак для последующего контроля и автоматизации защит.

### attack-scenarios.md
Конкретные сценарии атак на кластер и workloads.  
DevSecOps-смысл: тестирование и validation security controls против реальных угроз.

### mitigations.md
Рекомендации и controls для снижения рисков из attack scenarios и STRIDE.  
DevSecOps-смысл: roadmap для enforcement через policy-as-code и runtime-security.

### README.md
Документация к threat-model/.  
Объясняет как использовать threat model для планирования политик и runtime защит.  
DevSecOps-смысл: единый reference по угрозам и их mitigation для всей платформы.

## environments/
Директория для **environment-specific конфигураций Kubernetes**.  
DevSecOps-смысл: управление различными стадиями жизненного цикла кластера через declarative manifests и kustomize.

### dev/
Development окружение.  
- `kustomization.yaml` — overlay для dev namespace, настройки ресурсов, debug flags.  
DevSecOps-смысл: безопасное тестирование и validation политик перед stage/prod.

### stage/
Staging окружение.  
- `kustomization.yaml` — overlay для stage с production-like настройками.  
DevSecOps-смысл: pre-prod проверка и финальный контроль policy enforcement.

### prod/
Production окружение.  
- `kustomization.yaml` — overlay для prod с строгими security и resource constraints.  
DevSecOps-смысл: immutable, enforceable и auditable production deployment.

### base/
Базовые manifests для всех окружений.  
- `kustomization.yaml` — общие ресурсы, namespace, configmaps, secrets placeholders.  
DevSecOps-смысл: single source of truth для всех environments.

### kustomization.yaml
Главный kustomization файл, объединяющий base и overlays.  
DevSecOps-смысл: централизованный control over all environments, reproducible deployments.

### README.md
Документация к environments/.  
Объясняет структуру overlay, overlay inheritance и принципы управления конфигурациями.  
DevSecOps-смысл: быстрое понимание как environment-aware deployments интегрируются с security.

## ci/
Директория для **CI/CD пайплайнов безопасности Kubernetes**.  
DevSecOps-смысл: автоматизация проверки, enforcement и compliance на этапе CI, до деплоя.

### kube-linter.yml
Конфигурация для проверки Kubernetes manifests через kube-linter.  
DevSecOps-смысл: выявление misconfigurations и enforceable best practices на раннем этапе.

### opa-policy-check.yml
CI job для проверки ресурсов на соответствие OPA/Gatekeeper policy.  
DevSecOps-смысл: гарантирует, что все manifests соответствуют централизованным политикам.

### trivy-k8s.yml
Сканирование container images и Kubernetes manifests на уязвимости через Trivy.  
DevSecOps-смысл: раннее обнаружение vulnerabilities и compliance violations.

### manifest-validate.yml
Validation pipeline для всех Kubernetes manifests (schema, duplication, best practices).  
DevSecOps-смысл: prevent misconfig и enforce consistent manifests across environments.

### compliance-report.yml
Автоматическая генерация отчетов по соответствию CIS/NIST/ISO стандартам.  
DevSecOps-смысл: traceable evidence для auditors и CI/CD reporting.

### README.md
Документация к ci/.  
Объясняет порядок работы всех CI jobs, их цели и интеграцию с GitOps.  
DevSecOps-смысл: единый reference для всех automated security checks.

## scripts/
Директория для **утилитарных скриптов управления и проверки кластера**.  
DevSecOps-смысл: автоматизация рутинных операций, enforceable baseline и проверка security posture.

### apply-baseline.sh
Применяет baseline manifests (namespaces, pod-security, resource quotas) в кластер.  
DevSecOps-смысл: reproducible baseline deployment для всех окружений.

### verify-cluster.sh
Скрипт для проверки состояния security controls и compliance после деплоя.  
DevSecOps-смысл: быстрый аудит и validation enforcement.

### audit-dump.sh
Сохраняет audit logs и cluster events для анализа.  
DevSecOps-смысл: traceable evidence и forensic-ready данные для compliance и incident response.

### architecture_bootstrap.sh
Инициализация структуры cluster, overlay environments и policy-engine.  
DevSecOps-смысл: reproducible cluster setup с enforceable security layers.

### README.md
Документация к scripts/.  
Объясняет назначение каждого скрипта и порядок их применения.  
DevSecOps-смысл: единый reference для автоматизации cluster management и security enforcement.

## docs/
Директория для **документации и описания архитектуры безопасности платформы**.  
DevSecOps-смысл: единый источник знаний для разработчиков, DevOps и аудиторов, обеспечивающий понимание, enforcement и traceability.

### architecture.md
Описание security-архитектуры Kubernetes платформы.  
DevSecOps-смысл: понимание слоёв защиты, control plane vs data plane и места runtime enforcement.

### security-boundary.md
Формализация trust boundaries и границ ответственности.  
DevSecOps-смысл: clear definition trusted vs untrusted, human vs workload identities.

### kubernetes-threat-model.md
Threat model уровня платформы.  
DevSecOps-смысл: систематизация attack surfaces, control plane и workload угроз.

### policy-strategy.md
Документ по стратегии применения политик.  
DevSecOps-смысл: lifecycle, versioning, OPA vs Kyverno, enforceable policy-as-code.

### admission-control.md
Enforcement на входе в кластер.  
DevSecOps-смысл: validating/mutating admission webhooks, Pod Security Standards enforcement.

### runtime-security.md
Защита после деплоя.  
DevSecOps-смысл: runtime detection, seccomp/AppArmor, alerting и response.

### supply-chain-security.md
Доверие к артефактам и образам.  
DevSecOps-смысл: image trust, SBOM, provenance, CI → Deployment traceability.

### compliance-overview.md
Встроенный compliance как свойство платформы.  
DevSecOps-смысл: CIS/NIST/ISO coverage, mapping, evidence generation.

### repository-structure.md
Руководство по структуре репозитория.  
DevSecOps-смысл: быстрое понимание назначения директорий, enforceable anti-pattern avoidance и принципы immutable infra & policy-as-code.

