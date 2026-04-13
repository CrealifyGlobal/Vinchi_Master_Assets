meta::
#class_behavior
#status_completed

[[Class_Skill]]

Class Name:
Skill

Behavioral Purpose:
A **Skill** acts as a **fine-grained unit of applied human ability**—a learned and demonstrable capability that enables the execution of tasks, fulfillment of roles, and participation in business processes. It forms the **operational substrate of workforce capability** and **granular driver of organizational productivity**.

Core Behaviors:
- **Enable Task Execution** – Skills are the atomic enablers of work.
- **Support Role Fulfillment** – Map directly to job functions and performance expectations.
- **Feed Competency Construction** – Aggregate into broader Competency models.
- **Trigger Learning & Assessment** – Define targets for training programs and validation pathways.
- **Anchor Workflow Participation** – Required for performing steps in defined business processes.

Interaction Targets:
- **Competency** – Aggregated to form complex capabilities.
- **Role** – Assigned as essential or desirable for performance.
- **Business Process** – Required to complete specific tasks or subprocesses.
- **Training Program** – Taught, developed, and certified through learning interventions.
- **Assessment** – Measured via tests, certifications, or observed performance.

Sensing Triggers:
- **Job Design** – Used to define what is needed for a role.
- **Performance Review** – Evaluated to track skill gaps or development.
- **Certification Request** – Verification of proficiency for high-stakes roles.
- **Workforce Planning** – Assessed for talent readiness and skills coverage.

Decision Rules:
- `skillProficiencyLevel = Expert` → requires formal validation.
- `skillType = Technical` → must link to at least one Business Process or toolset.
- `appliesToRole ≠ null` → skill must map to one or more formal roles.
- `requiredByCompetency ≠ null` → skill must be part of a defined capability structure.

Adaptation Behavior:
- **Evolving Skill** – Subject to obsolescence or emergence (e.g., AI prompt engineering).
- **Transferable Skill** – Relevant across roles or industries (e.g., critical thinking).
- **Composite Skill** – Formed by combining multiple micro-skills (e.g., data storytelling = analysis + narrative framing).
- **Verified Skill** – Endorsed or tested through structured evaluation.

Lifecycle Phases:
- **Definition** – Identified and described with attributes.
- **Assignment** – Mapped to roles and competencies.
- **Development** – Acquired via learning experiences or practice.
- **Validation** – Confirmed through assessments or certifications.
- **Application** – Used in day-to-day execution of tasks.

Special Traits:
- **Atomicity** – Minimum actionable unit of human capability.
- **Cross-Domain Utility** – Can be reused across contexts, roles, and sectors.
- **AI-Compatible** – Skills can be parsed, extracted, or inferred using NLP/NLU models.
- **Workforce Planning Primitive** – Foundation for skill gap analysis, upskilling, and talent deployment.

Fine-Tuning Parameters:
- **Precision in Role Matching:** 9.7/10 – Key determinant of fit and readiness.
- **Training Program Integration:** 9.5/10 – Central to L&D architecture.
- **Granularity for Assessment Design:** 9.3/10 – Drives micro-credentialing and evaluation.
- **Adaptability in Evolving Workflows:** 9.0/10 – Essential for future-proofing talent.

