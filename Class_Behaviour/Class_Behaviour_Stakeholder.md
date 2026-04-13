meta::
#class_behavior
#status_completed

[[Class_Stakeholder]]

Class Name:
Stakeholder

Behavioral Purpose:
A **Stakeholder** is an **entity of influence**—individual, group, or organization—that **affects, is affected by, or holds an interest in** the outcomes of a decision, process, or objective. Stakeholders function as **strategic participants, enablers, gatekeepers, or beneficiaries**, shaping governance, resource allocation, and risk dynamics across an organization’s lifecycle.

Core Behaviors:
- **Expectation Holding:** Defines success, risk tolerance, and satisfaction conditions.
- **Influence Assertion:** Exerts formal or informal power over decisions, outcomes, or approvals.
- **Engagement Cycling:** Participates in recurring cycles of consultation, escalation, or feedback.
- **Governance Alignment:** Anchors processes to policies, standards, and compliance rules.
- **Risk & Reputation Sensitivity:** Responds to shifts in operational, financial, or reputational impact.

Interaction Targets:
- **Projects & Programs**, as objects of interest or investment.
- **Decisions**, where stakeholder inputs shape or constrain outcomes.
- **Strategic Objectives**, which stakeholders may support, contest, or refine.
- **Policies & Governance Frameworks**, which formalize stakeholder roles and rights.
- **Compliance Standards**, especially for regulatory or oversight stakeholders.

Sensing Triggers:
- Project or business milestone events (e.g., funding round, product launch).
- Policy updates or regulatory changes.
- Escalation of stakeholder concerns or objections.
- Initiation of new initiatives or cross-functional programs.
- Realignment of strategy or organizational design.

Decision Rules:
- Stakeholders may be **internal or external**, but must be tied to a Project, Program, or Objective.
- Stakeholders are **non-executable** entities but influence executable classes like Decisions or Metrics.
- A Stakeholder must be **linked to at least one Policy** if classified as Regulatory.
- Influence is quantifiable (e.g., via `levelOfInfluence`) and guides prioritization.

Adaptation Behavior:
- **Reclassification:** Stakeholders may shift between internal/external or primary/secondary based on scope.
- **Escalation Response:** Engagement frequency increases when project impact rises.
- **Influence Modeling:** Influence weight can be dynamically reassessed based on project stage or conflict.
- **Consent or Objection Cycling:** Stakeholder feedback loops may lead to decision gates or design pivots.

Lifecycle Phases:
- **Identification:** Stakeholders are mapped and classified at the start of strategic initiatives.
- **Analysis:** Their interests, expectations, and power dynamics are profiled.
- **Engagement Planning:** Interaction frequency and methods are defined.
- **Monitoring:** Influence and satisfaction are tracked through feedback and decision outcomes.
- **Closure or Transformation:** Stakeholder relationships are resolved, transferred, or institutionalized.

Special Traits:
- **Multi-Class Anchoring:** A Stakeholder may appear in both compliance models and value delivery chains.
- **Cross-Entity Visibility:** Stakeholders often connect disparate organizational elements (e.g., Strategy ↔ Project ↔ Policy).
- **Asymmetric Power:** Influence may not be proportional to visibility or participation.
- **Alignment Tension:** Stakeholders can drive or resist change depending on perceived alignment with objectives.

Fine-Tuning Parameters:
- **Persistence Score:** 9/10 – Stakeholders often remain involved across multiple lifecycles.
- **Influence Score:** 10/10 – Stakeholders are key determinants of approval, risk posture, and public trust.
- **Sensitivity to Change Triggers:** 10/10 – Highly reactive to political, financial, and ethical shifts.
- **Dependency Score:** 7/10 – May be critical to success but not always formally involved.
