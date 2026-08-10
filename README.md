# OCI ENS High — One-OE Blueprint Design Elements

> **Document status:** Draft for team review and subsequent validation with CCN
>
> **Target:** Esquema Nacional de Seguridad (ENS), **categoría ALTA**
>
> **Scope:** OCI Operating Entities Landing Zone (One-OE blueprint), its selected security options, and the landing-zone foundation it provisions. This is not an ENS certification statement.
>
> **Language:** English is the working version. A Spanish review summary follows the English content.

## 1. Purpose

This document defines the ENS High design elements that an OCI One-OE landing zone can establish, configure, or support through Infrastructure as Code (IaC).

The document has four goals:

1. State which controls are implemented or configurable in landing-zone IaC.
2. Identify the customer’s manual and operational responsibilities from IaC.
3. Identify workload, organisational, and service-contract controls that a landing zone cannot satisfy.
4. Provide a traceable, proposed mapping to ENS measures and OCI One-OE landing-zone design elements.

OCI Operating Entities Landing Zone (OCI Open LZ) is the appropriate technical baseline: it provides One-OE, Multi-OE, and Multi-Tenancy blueprints. Oracle describes the One-OE blueprint as the option for onboarding one operating entity, its environments, platforms, and projects into one tenancy. [OCI Operating Entities Landing Zone repository](https://github.com/oci-landing-zones/oci-landing-zone-operating-entities)

## 2. Important compliance statement

This guide **does not claim ENS conformity or certification** for OCI, Oracle, a tenant, a workload, or the One-OE blueprint.

ENS category is determined from the security impact of each information and service dimension, and a system is **ALTA** when at least one dimension is rated High. The ENS measures must be selected according to the actual assets, relevant dimensions, category, and risk analysis. Systems of category Media or Alta require a certification audit. [Royal Decree 311/2022, Annexes I–III](https://www.boe.es/buscar/act.php?id=BOE-A-2022-7191)

Accordingly:

- A control marked **IaC** in this document means that the design can be declared and deployed by a reviewed One-OE configuration. It does **not** mean it is enabled in every default configuration or that the deployed configuration is effective.
- A proposed ENS relationship is a design hypothesis for validation. It is not a CCN-approved control matrix.
- The customer remains accountable for the system risk analysis, system categorisation, security policy, operational procedures, evidence, residual-risk acceptance, and certification engagement.
- OCI resiliency, and cloud security more broadly, follows a shared-responsibility model. OCI provides resilient cloud infrastructure; customers design, deploy, test, and operate the workload HA/DR and continuity arrangements. [OCI shared responsibility model](https://docs.oracle.com/en-us/iaas/Content/cloud-adoption-framework/oci-shared-responsibility.htm)

## 3. Scope and design assumptions

### 3.1 In scope

The technical foundation that can be addressed by One-OE configuration and optional landing-zone modules:

- tenancy and compartment structure;
- IAM groups, dynamic groups, policies, and segregation of administrative roles;
- VCN topology, routing, security lists/NSGs, private connectivity, and optional hub-and-spoke controls;
- Cloud Guard targets and recipes;
- Security Zones, where selected;
- logging, Audit integration, flow logs, Connector Hub, events, notifications, and monitoring integration;
- Vault and customer-managed key design, where selected;
- Vulnerability Scanning Service (VSS), where selected;
- resource tagging, inventory support, and budgets, where selected;
- optional Bastion, firewall, SIEM, web protection, and connectivity extensions.

Oracle’s OCI Landing Zones documentation describes landing zones as hardened Terraform templates for identity, network, security, monitoring, and governance foundations. It also documents Cloud Guard, Flow Logs, Connector Hub, Vault, VSS, Bastion, and Security Zones as OCI-native security capabilities used by the Core Landing Zone. [OCI Landing Zones overview](https://docs.oracle.com/en-us/iaas/Content/cloud-adoption-framework/oci-landing-zones-overview.htm) · [OCI Core Landing Zone](https://docs.oracle.com/en-us/iaas/Content/cloud-adoption-framework/oci-core-landing-zone.htm)

### 3.2 Explicitly out of scope for the landing-zone baseline

- Classification and categorisation of the information system.
- Formal ENS risk analysis and formal acceptance of residual risk.
- Organisational security policy, governance, personnel training, disciplinary processes, and security roles.
- Application design, secure development, source-code security, application secrets, application logging, and application patching.
- Workload data classification, retention, deletion, backup/restore configuration, DR design, and continuity testing.
- Operational incident response, monitoring coverage, alert triage, forensic procedures, and reporting to the competent authorities.
- Evidence review, audit, and the ENS conformity/certification decision.

These exclusions are not gaps in the blueprint: they are controls that need customer-specific information, operational ownership, or workload-specific implementation.

### 3.3 Optional components

Optional components are not assumed to exist. They must be selected in configuration, version-controlled, deployed, and verified:

- Security Zones for preventive guardrails in designated compartments;
- Cloud Guard custom detector/responder recipes and responder automation;
- VSS targets and scan recipes;
- Vault keys and customer-managed-key adoption by in-scope services;
- central log export to a customer SIEM;
- OCI Network Firewall or a third-party firewall;
- Bastion;
- WAF, DDoS protection, DNS controls, and other workload-edge services;
- multi-AD/region resilience, backups, and replication.

## 4. Responsibility and implementation labels

| Label | Meaning | Evidence expected before closure |
|---|---|---|
| **IaC — baseline** | Declared in the reviewed One-OE configuration and applied successfully. | Versioned input, Terraform plan/apply output, OCI resource inventory, and configuration export. |
| **IaC — optional** | Available through a selected blueprint/module option or supported extension; not assumed enabled. | The baseline evidence plus the selected option and its effective OCI configuration. |
| **Manual — customer** | Cannot be completed by the blueprint alone; customer action is required. | Approved procedure, ticket/change record, operational record, or test result. |
| **Shared — customer/OCI** | OCI provides the service capability; customer configures, operates, and evidences its use. | Service configuration, OCI evidence, and customer operational evidence. |
| **Out of scope** | A workload, organisational, contractual, or physical control beyond this landing-zone design. | Separate system/workload control record. |

## 5. Architecture principles for ENS High

1. **Treat the landing zone as a common security foundation, not the system boundary.** Each in-scope workload must extend the baseline according to its data, interfaces, threat model, and availability requirement.
2. **Use least privilege and separation of duties.** Place network, security, IAM, and workload administration in separate roles/compartments and use narrowly scoped policies.
3. **Prefer prevention and detection together.** Security Zones can deny non-compliant resource operations; Cloud Guard detects configuration and activity concerns. Neither replaces review and response.
4. **Keep configuration reviewable.** Configuration inputs, plans, approvals, applies, and post-deployment verification are evidence. Changes must follow a controlled change process.
5. **Design logging as an evidence pipeline.** Capture the defined logs, protect access, set retention/export requirements, integrate alerts, and test retrieval. Logging enabled without review, retention, or response is insufficient.
6. **Make every exception explicit.** Security-Zone exemptions, broad IAM policies, public endpoints, disabled detector rules, or unencrypted exceptions require documented approval, owner, expiry, and compensating controls.
7. **Validate in OCI after deployment.** IaC state is evidence of intent. OCI configuration, Cloud Guard status, logs, and access tests are evidence of effectiveness.

## 6. Proposed ENS High control-design matrix

The ENS measure IDs below are taken from Annex II of Royal Decree 311/2022. They show areas that the design can support; applicability, required reinforcements, and sufficiency must be determined by the system risk analysis and validated with CCN. For example, ENS High includes formal risk analysis for `op.pl.1`, extensive security-architecture reinforcements for `op.pl.2`, and stronger requirements for access, logging, monitoring, cryptography, communications, continuity, and service protection. [ENS Annex II measure table](https://www.boe.es/buscar/act.php?id=BOE-A-2022-7191)

| ENS measure area (proposed) | One-OE / OCI design element | IaC contribution | Manual customer responsibility and limitation | Status |
|---|---|---|---|---|
| `op.pl.2` Security architecture | Parent/child compartments; segregated admin groups; VCN topology; shared security services | Declare the tenancy foundation, network boundaries, and separation of administration duties. | Produce the system architecture, interfaces, asset model, threat model, and formally approve the architecture. | IaC — baseline + Manual — customer |
| `op.acc.1`–`op.acc.4` Access control | IAM groups, dynamic groups, policies, compartments, federation option | Implement RBAC and compartment-scoped policy structure. | Define joiner/mover/leaver processes, recertify access, configure identity-provider/MFA requirements, and review policies for each workload. | IaC — baseline + Manual — customer |
| `op.acc.5`–`op.acc.6` Authentication | Identity-domain/federation integration where selected | Provide IAM integration points and policies. | Select and enforce the ENS-appropriate authentication mechanisms, MFA, lifecycle, external-user assurance, and privileged access process. | Manual — customer / Shared |
| `op.exp.1` Asset inventory | Compartments, tags, resource search/inventory support | Establish managed structure and, where configured, tag namespaces/keys. | Define the authoritative CMDB/asset process; ensure coverage of workload, SaaS, endpoints, and non-OCI assets; reconcile regularly. | IaC — optional + Manual — customer |
| `op.exp.2`–`op.exp.5` Secure configuration and change management | Git-managed configuration, Resource Manager/Terraform plan and apply | Make desired configuration reproducible and reviewable. | Establish approvals, change windows, break-glass procedure, drift review, release management, and security-baseline review. | IaC — baseline + Manual — customer |
| `op.exp.7`, `op.mon.1`–`op.mon.3` Incident management, intrusion detection, monitoring, and vigilance | Cloud Guard targets, detector recipes, responder recipes, events, notifications | Configure compartment scope and selected detection/response integrations. Cloud Guard targets define the monitored compartment scope and associated detector/responder recipes. | Tune recipes, set owners and severity handling, triage findings, investigate, preserve evidence, exercise response, and document reporting/escalation. Automatic responders require risk review before activation. | IaC — optional + Manual — customer |
| `op.exp.8`–`op.exp.9` Activity and incident logging | Audit, service logs, VCN flow logs, Logging, Connector Hub, SIEM export | Enable/configure selected log sources, routes, and notifications. | Define log source coverage, retention, integrity, time synchronisation, access restrictions, monitoring use cases, review cadence, and incident-case records. | IaC — optional + Manual — customer |
| `op.exp.10`, `mp.si.2` Cryptographic-key and media protection | Vault and customer-managed-key option | Create vault/key resources and support their use by selected services. | Select approved cryptography, configure each workload/service to use the required keys, control key lifecycle and recovery, and retain cryptographic evidence. A vault alone does not encrypt every workload automatically. | IaC — optional + Manual — customer |
| `mp.com.1`–`mp.com.4`, `op.ext.4` Network perimeter, confidentiality/integrity, segmentation, and interconnection | VCNs, subnets, route tables, NSGs/security lists, gateways, hub-and-spoke, firewall option, private connectivity | Declare network segmentation and controlled routing. | Approve flows and interconnections, configure application TLS/mTLS where required, validate exposure, manage certificates, and test segmentation. | IaC — baseline + IaC — optional + Manual — customer |
| `op.exp.4`, `op.exp.6` Security maintenance and malicious-code protection | VSS host/image target options; Cloud Guard | Configure selected scan targets and visibility. OCI VSS routinely checks supported hosts and container images for vulnerabilities. | Patch/remediate, maintain exceptions, cover unsupported assets, establish malware protection for endpoints/workloads, and verify remediation. | IaC — optional + Manual — customer |
| `op.nub.1`, `op.ext.1`–`op.ext.3` Cloud services, agreements, daily management, and supply chain | Version-pinned IaC sources and deployment pipeline design | Provide a controlled technical baseline and source traceability. | Assess service suitability, contracts/SLA, suppliers, support process, regional/data-residency requirements, SBOM/dependency controls, and daily operations. | Manual — customer / Shared |
| `op.cont.1`–`op.cont.4`, `mp.info.6` Continuity, alternative means, tests, and backups | Optional multi-AD/region patterns; workload modules may provide service-specific options | Provide a foundation on which resilient workloads can be built. | Define RTO/RPO, implement workload backups/replication/failover, ensure capacity in recovery location, conduct periodic tests, and retain evidence. OCI does not automatically deploy, replicate, or fail over customer workloads. | Out of scope for baseline + Manual — customer |
| `mp.s.2`–`mp.s.4` Web-service and DDoS protection | Optional WAF, firewall, and edge-service extensions | Provide optional architecture integration points. | Select, configure, operate, and test workload web protections, DDoS strategy, certificates, and application controls. | IaC — optional / Out of scope for baseline |
| `org.*`, `mp.per.*`, `mp.if.*`, `mp.eq.*`, `mp.sw.*`, `mp.info.*` | N/A | No landing-zone implementation claim. | Implement governance, policies, procedures, personnel controls, physical controls, secure development, data handling, e-signature/time-stamping where applicable, and workload protections. | Out of scope |

### 6.1 Why Cloud Guard is important, but not sufficient

Cloud Guard is a central baseline service because it can monitor compartment targets using configuration and activity detectors and can attach responder recipes. Oracle documents that targets set the monitored scope and detector recipes determine the monitoring rules; responder recipes may take corrective action automatically or with administrator intervention. [About OCI targets](https://docs.oracle.com/en-us/iaas/cloud-guard/using/targets-about.htm) · [About detector recipes](https://docs.oracle.com/en-us/iaas/Content/cloud-guard/using/detect-recipes-about.htm)

For ENS High, deployers must still define and evidence:

- the target-compartment hierarchy and exclusions;
- enabled detector rules, severity, and justified suppressions;
- responder actions, approval gates, rollback and exception handling;
- alert routing, on-call ownership, triage SLA, and incident procedures;
- periodic review of coverage and effectiveness.

Cloud Guard findings are evidence inputs, not proof of ENS conformity.

### 6.2 Security Zones

Security Zones are a recommended optional preventive guardrail for compartments requiring strict resource-creation constraints. Oracle states that operations in a Security Zone are validated against its recipe and that invalid operations are denied; this includes examples such as public accessibility and customer-managed-key requirements. [OCI Core Landing Zone — Security Zones](https://docs.oracle.com/en-us/iaas/Content/cloud-adoption-framework/oci-core-landing-zone.htm)

Before enabling a Security Zone, the customer must define the in-scope compartment boundary, evaluate existing-resource violations, approve exception handling, and test workload compatibility. It complements, but cannot replace, application, data, operational, and governance controls.

## 7. Deployment and verification workflow

1. **Establish scope.** Record the tenancy, region(s), parent compartment, workload boundary, owners, ENS dimensions, and target category.
2. **Create the risk and architecture baseline.** Perform the formal ENS High risk analysis and identify which Annex II measures and reinforcements apply.
3. **Select One-OE options.** Document the exact repository commit/tag, Terraform/Resource Manager version, input files, optional modules, and any deviations.
4. **Review before apply.** Review Terraform plan and policy/network/security changes under the change-management process. Do not use a successful plan as evidence of effectiveness.
5. **Apply and capture evidence.** Store approved inputs, plan/apply outputs, resource inventory, and outputs in the approved evidence location.
6. **Verify in OCI.** Verify effective IAM policies, Cloud Guard targets/recipes, Security Zones, logs, flow logs, key configuration, scan targets, routes/NSGs, notifications, and SIEM connectivity as selected.
7. **Run security acceptance tests.** Test denied actions, least-privilege access, public exposure, log generation/retrieval, alert routing, responder approval/rollback, and segmentation.
8. **Operate and reassess.** Review drift, security findings, access, logging, vulnerabilities, exceptions, and changes periodically. Reassess when the system, risk profile, or ENS requirements change.

## 8. Minimum evidence pack

The following is a starting evidence pack, not a substitute for the auditor’s evidence request:

- System categorisation, formal risk analysis, applicable ENS measure/reinforcement matrix, and accepted residual risks.
- Security architecture, data-flow, network, IAM, and logging diagrams.
- Approved versioned One-OE inputs, source commit/tag, Terraform/provider versions, plans, applies, and drift reviews.
- OCI inventory of compartments, groups, policies, networks, Security Zones, Cloud Guard targets/recipes, log groups, vaults/keys, scan targets, and integrations.
- Screenshots or API/CLI exports showing effective configurations, retained with collection time and owner.
- Access-review records, change approvals, break-glass records, and exception register.
- Cloud Guard/VSS findings, remediation tickets, vulnerability exceptions, and recurring review evidence.
- Log coverage, retention, retrieval, and alert-routing test results.
- Backup/restore, resilience/failover, and incident-response test results for each workload where applicable.
- Supplier/service assessments and the relevant OCI contractual/compliance evidence.

## 9. Prowler use and limitation

Prowler is useful as a **methodological reference**: it demonstrates how cloud-service checks can be grouped into compliance frameworks and how a control-to-check catalogue can provide continuous evidence. Its public project lists ENS among its supported frameworks and includes OCI provider checks, but its published provider inventory must be checked at the exact version in use. [Prowler repository](https://github.com/prowler-cloud/prowler)

This guide makes **no claim** that Prowler supplies an OCI ENS framework, that AWS/Azure ENS checks apply to OCI, or that an AWS/Azure Prowler result validates OCI. Any future OCI mapping derived from Prowler must:

1. identify the OCI resource/configuration being tested;
2. document the equivalent OCI evidence source and the control rationale;
3. distinguish preventive, detective, and manual controls;
4. be reviewed by the ENS control owner and validated with CCN; and
5. remain separate from an assertion of certification.

## 10. Relationship to other material

- The [OCI PCI DSS guidance add-on](https://github.com/oci-landing-zones/oci-landing-zone-operating-entities/tree/master/addons/oci-pci-dss-guidance) is a useful format reference. Its PCI DSS mapping must not be reused as ENS evidence without a separate ENS rationale.
- The AWS [CCN-STIC-887 Anexo A / Landing Zone Accelerator article](https://aws.amazon.com/blogs/security/ccn-releases-guide-for-spains-ens-landing-zones-using-landing-zone-accelerator-on-aws/) is a validated AWS-specific reference. It is valuable for its landing-zone control-matrix and evidence approach, but it does not validate equivalent OCI controls.
- ISO/IEC 27001 may be used internally as supporting security-management guidance. It is not the normative ENS control source for this document.

## 11. Open validation items

These decisions are deliberately open until the design is validated with CCN and the customer’s ENS stakeholders:

| Item | Decision required |
|---|---|
| ENS profile | Confirm the applicable CCN-STIC profile, measures, and High reinforcements for the actual system. |
| OCI region and service eligibility | Confirm the selected OCI region, services, contracts, data residency, and any regulator requirements. |
| One-OE implementation | Identify the approved commit/tag and exact input configuration to which this guide applies. |
| Cloud Guard | Approve target scope, recipe baseline, suppressions, responder automation, and operations model. |
| Logging | Approve log sources, retention, integrity, SIEM route, access model, monitoring use cases, and evidence retrieval. |
| Cryptography | Approve cryptographic policy, key ownership/lifecycle, customer-managed-key adoption, and exception process. |
| Network | Approve segmentation, ingress/egress, interconnections, private connectivity, firewall, and certificate design. |
| Continuity | Approve workload RTO/RPO, backup, DR architecture, capacity, and test schedule. |
| Prowler-derived checks | Build an OCI-specific proposal only after each equivalent check has OCI evidence and CCN review. |

## 12. References

1. [Royal Decree 311/2022 — Esquema Nacional de Seguridad (consolidated text)](https://www.boe.es/buscar/act.php?id=BOE-A-2022-7191)
2. [OCI Landing Zones overview](https://docs.oracle.com/en-us/iaas/Content/cloud-adoption-framework/oci-landing-zones-overview.htm)
3. [OCI Core Landing Zone](https://docs.oracle.com/en-us/iaas/Content/cloud-adoption-framework/oci-core-landing-zone.htm)
4. [OCI Operating Entities Landing Zone repository](https://github.com/oci-landing-zones/oci-landing-zone-operating-entities)
5. [OCI shared responsibility model for resiliency](https://docs.oracle.com/en-us/iaas/Content/cloud-adoption-framework/oci-shared-responsibility.htm)
6. [OCI Cloud Guard targets](https://docs.oracle.com/en-us/iaas/Content/cloud-guard/using/targets-about.htm)
7. [OCI Cloud Guard detector recipes](https://docs.oracle.com/en-us/iaas/Content/cloud-guard/using/detect-recipes-about.htm)
8. [OCI Vulnerability Scanning overview](https://docs.oracle.com/en-us/iaas/Content/scanning/using/overview.htm)
9. [Prowler](https://github.com/prowler-cloud/prowler)
10. [AWS article: CCN guide for ENS landing zones](https://aws.amazon.com/blogs/security/ccn-releases-guide-for-spains-ens-landing-zones-using-landing-zone-accelerator-on-aws/)

---

# Resumen en español (para revisión)

## Propósito y límite de la guía

Esta guía define los elementos de diseño que una *landing zone* OCI One-OE puede desplegar o soportar para un sistema ENS de categoría **ALTA**. El documento distingue expresamente entre capacidades de IaC, configuración y operación manual del cliente, responsabilidades compartidas con OCI y controles fuera del alcance de la *landing zone*.

**No es una declaración de conformidad ni una certificación ENS.** La categoría ENS depende del análisis de impacto y riesgos del sistema concreto. Para categoría ALTA se exige auditoría de certificación. La validación final de la matriz propuesta corresponde al cliente y al CCN.

## Leyenda

| Etiqueta | Significado |
|---|---|
| **IaC — base** | Está declarado en la configuración One-OE revisada y se debe comprobar tras el despliegue. |
| **IaC — opcional** | Existe como opción/extensión; no se presupone habilitado. |
| **Manual — cliente** | Requiere proceso, decisión, operación o evidencia del cliente. |
| **Compartido — cliente/OCI** | OCI ofrece la capacidad; el cliente debe configurarla, operarla y evidenciarla. |
| **Fuera de alcance** | Es un control organizativo, físico, contractual o específico de la carga de trabajo. |

## Capacidades principales de la One-OE Landing Zone

- Estructura de compartimentos, grupos, políticas IAM y segregación de funciones.
- Red VCN, subredes, rutas, NSG/listas de seguridad y opciones de topología *hub-and-spoke*.
- Cloud Guard para detección, recetas de detectores/respondedores y notificaciones.
- Security Zones opcionales como barrera preventiva.
- Audit, Logging, VCN Flow Logs, Connector Hub, eventos e integración con SIEM cuando se configure.
- Vault y claves gestionadas por el cliente, si se seleccionan y se adoptan por las cargas de trabajo.
- Vulnerability Scanning Service, Bastion, firewall, etiquetado y presupuestos como opciones según diseño.

## Responsabilidades manuales imprescindibles

El cliente debe realizar y mantener el análisis formal de riesgos ENS, la categorización del sistema, las políticas y procedimientos de seguridad, la gestión de identidades y accesos, el triaje de alertas, la respuesta a incidentes, la retención y revisión de logs, el parcheado, la gestión de vulnerabilidades, las copias de seguridad, continuidad/DR, pruebas y evidencias para auditoría.

Cloud Guard y Security Zones aportan controles técnicos relevantes, pero no sustituyen estas responsabilidades ni prueban la conformidad ENS. Prowler puede servir de referencia para estructurar futuras comprobaciones OCI, pero esta guía no afirma que Prowler tenga una matriz ENS para OCI ni que las comprobaciones AWS/Azure sean válidas para OCI.

## Próximos pasos

1. Fijar el *commit/tag* y los ficheros de entrada One-OE que se evaluarán.
2. Completar la matriz ENS según el análisis de riesgos y el perfil CCN aplicable.
3. Confirmar las opciones desplegadas: Cloud Guard, Security Zones, logs/SIEM, VSS, Vault/CMK, red y continuidad.
4. Validar el mapeo y la evidencia con los responsables ENS y el CCN antes de cualquier afirmación de conformidad.
