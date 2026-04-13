meta::
#class_behavior
#status_completed

[[Class_Pains]]

Class Name:
Pains

Behavioral Purpose:
The **Pains** class encodes the **negative experiences, frustrations, obstacles, or risks** encountered by customers when attempting to fulfill a need, complete a job, or interact with an offering. It enables systems to **identify friction**, **prioritize product improvements**, and **align solutions with real customer suffering**.

Core Behaviors:
- **Friction Identification:** Detects where and how customers experience barriers or dissatisfaction.
- **Severity Scoring:** Quantifies pain impact and frequency to guide prioritization.
- **Resolution Mapping:** Links pains to value propositions and offerings designed to eliminate or reduce them.
- **Journey Interference Detection:** Highlights where pains interrupt or diminish the customer journey.
- **Opportunity Surface Detection:** Signals high-pain areas as ripe for innovation or competitive differentiation.

Interaction Targets:
- **Customer Jobs** – Defines the tasks where pains arise.
- **Value Proposition** – Addresses the pain through targeted solution messaging and delivery.
- **Customer Segment** – Indicates who suffers from specific pain patterns.
- **Market Conditions** – External trends or disruptions that intensify or trigger pain.
- **Customer Satisfaction** – Pain contributes to dissatisfaction or churn risk.

Sensing Triggers:
- Customer complaints or support tickets
- Drop-off points in journeys (e.g., abandoned carts, onboarding churn)
- Negative reviews or feedback
- Pain-related phrases in qualitative data (e.g., “too slow”, “too expensive”)
- Risk assessments or compliance failures

Decision Rules:
- A Pain **must be linked to a Customer Job** it obstructs.
- If **painSeverityLevel > 0.7**, flag as a strategic issue requiring resolution.
- Pains should be **addressed by one or more Value Propositions**.
- **PainFrequency + Severity** should be used to prioritize backlog features or redesigns.
- Emotional or trust-related pains should be flagged for **UX, brand, and communication improvements**.

Adaptation Behavior:
- **Trend Amplification:** Pains rise in priority if observed across multiple segments or channels.
- **Misalignment Detection:** Presence of persistent pains with no linked value proposition indicates a service gap.
- **Experience Sensitivity:** New pains can emerge in response to UI/UX, delivery, or market changes.
- **Segmentation Awareness:** Pains may affect different segments differently (e.g., enterprise vs. SMB).

Lifecycle Phases:
- **Discovery:** Pains are surfaced through research, complaints, or experience analysis.
- **Classification:** Pains are typed (functional, emotional, etc.) and scored by severity and frequency.
- **Linking:** Pains are mapped to related Jobs, Segments, and Value Propositions.
- **Resolution Design:** Trigger product, process, or communication interventions.
- **Validation:** Monitor pain resolution through satisfaction improvements or behavior change.

Special Traits:
- **Trust-Damaging:** Unresolved pains can erode loyalty and brand equity.
- **Behavior-Shaping:** Pains strongly influence churn, switching, and negative word of mouth.
- **Multi-Dimensional:** A single pain may have multiple sources (technical, policy, usability).
- **Remediation-Sensitive:** Pains often require cross-functional collaboration (UX, Ops, Support).

Fine-Tuning Parameters:
- **Persistence Score:** 8/10 – Many pains remain active until systematically addressed.
- **Influence Score:** 10/10 – Pains are the primary driver of negative experience and churn.
- **Behavioral Sensitivity:** 9/10 – Pains shape usage, dropout, escalation, and complaint behaviors.
- **Cross-Domain Reusability:** 7/10 – Pains like “lack of transparency” or “manual effort” are universal.

