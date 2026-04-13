meta::
#class_behavior
#status_completed

[[Class_Risk]]

Class Name:
Risk

Behavioral Purpose:
The **Risk** class captures **uncertainties with potential negative impact** on business operations, financial stability, reputation, or strategic objectives. It enables **systematic identification, classification, evaluation, and mitigation** of risks in dynamic environments. It supports risk-informed decision-making and regulatory alignment.

Core Behaviors:
- **Threat Identification:** Risks are actively discovered through operational monitoring, audits, or scenario modeling.
- **Impact Assessment:** Each risk carries metrics for likelihood and business impact.
- **Mitigation Coordination:** Risks trigger or relate to controls, responses, or remediation processes.
- **Governance Compliance:** Risks linked to legal or regulatory obligations enforce policy and compliance actions.
- **Continuity Enablement:** Acts as a signal for continuity planning and resiliency strategies.

Interaction Targets:
- **Business Processes** – Each risk must be scoped within or affect at least one business function or operation.
- **Compliance Requirements** – Legal or regulatory risks are mapped to frameworks (e.g., GDPR, SOX).
- **Risk Controls** – Risks invoke or are associated with mitigations such as backups, firewalls, redundancies.
- **Risk Responses** – Maps to playbooks, escalation procedures, and crisis plans.
- **Financial Structures** – Financial risks affect budgeting, cash flow planning, and investment forecasting.

Sensing Triggers:
- Internal **anomalies or failures** detected in monitored processes.
- **Changes in external context** (e.g., legislation, natural disasters, cyber threats).
- **Audit results** reveal control weaknesses or new vulnerabilities.
- **Business decisions or new initiatives** introduce unassessed uncertainty.

Decision Rules:
- Every Risk must be identified in a `Business Process` or linked to a `Compliance` framework.
- If `riskImpactLevel > 0.75`, prioritize for mitigation planning and executive review.
- If `riskMitigationStatus = "Unmitigated" AND riskLikelihood > 0.6`, escalate for immediate action.
- Compliance-related Risks must have traceable links to one or more `Standards` or `Requirements`.
- If `riskCategory = "Cybersecurity"`, route to security operations and technical control mapping.

Adaptation Behavior:
- **Dynamic Recalibration:** Risk likelihood and impact levels are adjusted based on new evidence or events.
- **Categorical Escalation:** Risk categorization informs who needs to respond (e.g., board-level for strategic, CISO for cybersecurity).
- **Cross-Domain Propagation:** High-impact risks may cascade to other semantic classes (e.g., affecting Projects, Financial Models, Capabilities).
- **Portfolio Aggregation:** Risks can be rolled up to assess aggregate enterprise risk exposure.

Lifecycle Phases:
- **Identification** – Detected via monitoring, incident, assessment, or forecasting.
- **Evaluation** – Quantified and classified (likelihood × impact).
- **Assignment** – Linked to business units, processes, and owners.
- **Mitigation** – Controlled or remediated by associated strategies or safeguards.
- **Monitoring** – Continuously observed for changes in status or severity.

Special Traits:
- **Probabilistic Nature:** All risks exist on a spectrum of likelihood and impact.
- **Multidimensional Impact:** Can simultaneously affect cost, compliance, continuity, and reputation.
- **Linked Responsibility:** Often requires coordinated response across governance, operations, and IT/security teams.

Fine-Tuning Parameters:
- **Persistence Score:** 6/10 – Risks can emerge, be mitigated, or evolve over time.
- **Influence Score:** 9/10 – Directly shapes compliance posture, operational continuity, and board-level strategy.
- **Behavioral Sensitivity:** 10/10 – Requires rapid adaptation to contextual changes.
- **Cross-Domain Reusability:** 8/10 – Appears in nearly all enterprise domains with domain-specific implications.

