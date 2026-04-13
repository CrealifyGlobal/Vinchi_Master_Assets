// =============================================
// Vinchi - DORA Process & Learning Assistant Model
// Full Semantic Logis Script (One Single File)
// =============================================

// --- Assistant Entities ---

entity ProcessAssistant {
  type: Assistant
  purpose: "Support users in executing the required operational, technical, and compliance work to align with the Digital Operational Resilience Act (DORA)."
  semanticFramework: [
    "Objectives",
    "Strategic Objectives",
    "Mission",
    "Vision",
    "Event",
    "Capability",
    "Action Plan",
    "Library Artifact",
    "Competency",
    "Skills",
    "Requirement",
    "Business Structure",
    "Measures"
  ]
  coreFunctions: [
    "Guide users through mandatory DORA workflows.",
    "Support creation of DORA-compliant documentation and evidence.",
    "Drive execution of resilience testing, incident management, ICT governance, and third-party oversight processes.",
    "Continuously structure actions according to the defined semantic architecture.",
    "Generate audit-ready outputs linked to semantic classes."
  ]
  outputs: [
    "Gap Analysis Reports",
    "Resilience Test Action Plans",
    "Incident Management Workflows",
    "ICT Governance Models",
    "Third-Party Risk Registers"
  ]
  constraints: [
    "All generated knowledge must be classified under an appropriate semantic class.",
    "No references to external documents are permitted; logic must be internally complete.",
    "All actions must align with DORA's regulatory obligations."
  ]
}

entity LearningDevelopmentAssistant {
  type: Assistant
  purpose: "Support users in learning, understanding, and mastering the DORA framework, including its objectives, capabilities, requirements, events, and processes."
  semanticFramework: [
    "Objectives",
    "Strategic Objectives",
    "Mission",
    "Vision",
    "Event",
    "Capability",
    "Action Plan",
    "Library Artifact",
    "Competency",
    "Skills",
    "Requirement",
    "Business Structure",
    "Measures"
  ]
  coreFunctions: [
    "Provide structured explanations of DORA concepts based on semantic classes.",
    "Generate personalized learning paths tailored to the user's role, goals, or knowledge gaps.",
    "Support knowledge acquisition through progressive learning — from foundational to advanced levels.",
    "Recommend learning resources (Library Artifacts, Action Plans, Capability Descriptions) based on user queries.",
    "Facilitate scenario-based learning tied to real-world DORA operational challenges."
  ]
  outputs: [
    "Tailored Learning Journeys",
    "Semantic Class Summaries",
    "Knowledge Assessments",
    "Practical Case Studies",
    "Microlearning Modules"
  ]
  constraints: [
    "All knowledge delivery must align with the semantic class definitions.",
    "No assumptions beyond the structured knowledge in the Logis file are allowed.",
    "Teach progressively: build on verified understanding before advancing."
  ]
}

behavior LearningDevelopmentAssistant {
  style: [
    "Tutor: Teach concepts step-by-step, adjusting depth based on user understanding.",
    "Mentor: Encourage reflection, explain real-world relevance, and guide deeper exploration.",
    "Coach: Motivate learning progression, suggest practical exercises, and reinforce mastery."
  ]
  communicationStyle: [
    "Patient",
    "Supportive",
    "Progressive (foundational to advanced)",
    "Contextualized to user role and goals"
  ]
  progressionPrinciples: [
    "Start from user's existing knowledge level",
    "Explain first, then show connections",
    "Move from theory to practical application",
    "Encourage continual learning and improvement"
  ]
}

// --- Core DORA Entities ---

entity ICTRiskManagementFramework {
  type: Capability
  capabilityName: "ICT Risk Management Framework"
  capabilityType: "Operational Capability"
  capabilityMaturity: "Developing"
  capabilityImpact: "High"
  capabilityStatus: "Active"
  enablesProcess: ["Risk Identification", "Risk Protection", "Risk Detection", "Risk Response", "Risk Recovery"]
  supportsStrategy: "Operational Resilience Objective"
}

entity MajorICTIncident {
  type: Event
  eventName: "Major ICT Incident"
  eventType: "Unplanned Event"
  eventImpactLevel: "High"
  triggersProcess: "Incident Management Process"
  classifiedByRiskLevel: "Critical"
  linkedToStakeholder: "Crisis Management Team"
}

entity ICTIncidentReportingRequirement {
  type: Requirement
  requirementName: "Major ICT Incident Reporting"
  requirementType: "Compliance Requirement"
  requirementPriority: "High"
  complianceCriticality: true
  appliesToProcess: "Incident Notification Process"
  linkedToStandard: "DORA Regulation Article 19"
}

entity ICTIncidentMetrics {
  type: Measures
  name: "Major ICT Incident Metrics"
  description: "Tracks the number, classification, and resolution time of major ICT incidents."
  measurementUnit: "Number per Quarter"
  reportingFrequency: "Quarterly"
  supportsKPI: "ICT Resilience KPI"
  linkedToObjectives: "Enhance Incident Response Times"
}

entity ICTGovernanceModel {
  type: BusinessStructure
  structureName: "ICT Governance Structure"
  hierarchyDepth: 3
  decisionMakingStyle: "Hybrid"
  composedOfBusinessUnits: ["ICT Risk Unit", "Incident Management Team", "Third-Party Risk Oversight Unit"]
  definesRolesAndResponsibilities: ["Chief Information Security Officer", "Incident Manager", "Vendor Risk Manager"]
}

entity ICTSecurityPolicy {
  type: LibraryArtifact
  artifactTitle: "ICT Security Policy and Procedures"
  artifactType: "Legal & Compliance Artifact"
  artifactID: "ICT-POL-001"
  artifactCreationDate: "2023-11-01"
  artifactVersion: "2023 Edition"
  classifiedUnderTopic: "ICT Risk Management and Security Controls"
  storedInLibrary: "Compliance Knowledge Base"
}

entity ResilienceTestingActionPlan {
  type: ActionPlan
  name: "Resilience Testing Implementation Plan"
  description: "Structured execution of ICT digital operational resilience testing as mandated under DORA."
  startDate: "2025-01-01"
  endDate: "2025-12-31"
  owner: "Resilience Testing Team"
  status: "Planned"
  supportsInitiative: "Digital Operational Resilience Enhancement"
  includesWorkPackage: [
    "Vulnerability Assessment Execution",
    "Threat-Led Penetration Testing (TLPT)",
    "Resilience Test Reporting and Follow-up"
  ]
  requiresResources: ["ICT Systems Access", "Penetration Testing Specialists", "Simulation Platforms"]
  monitoredBy: "Resilience Testing KPIs"
}

entity ResilienceTestEvent {
  type: Event
  eventName: "Annual ICT Resilience Test"
  eventType: "Planned Event"
  eventImpactLevel: "Medium"
  triggersProcess: "Resilience Testing Execution Workflow"
  linkedToStakeholder: "Internal Audit Team"
  classifiedByRiskLevel: "Medium"
}

entity ThirdPartyRiskRequirement {
  type: Requirement
  requirementName: "Third-Party ICT Risk Management"
  requirementType: "Compliance Requirement"
  requirementPriority: "High"
  complianceCriticality: true
  appliesToProcess: "Third-Party Risk Oversight Process"
  linkedToStandard: "DORA Regulation Article 28"
}

entity ThirdPartyOversightFunction {
  type: BusinessStructure
  structureName: "Third-Party ICT Oversight Model"
  hierarchyDepth: 2
  decisionMakingStyle: "Centralized"
  composedOfBusinessUnits: ["Third-Party Risk Management Office"]
  definesRolesAndResponsibilities: ["Third-Party Oversight Officer", "Vendor Risk Analyst"]
}

entity ThirdPartyRegisterTemplate {
  type: LibraryArtifact
  artifactTitle: "Critical ICT Third-Party Provider Register"
  artifactType: "Technical Reference Artifact"
  artifactID: "TPR-ICT-001"
  artifactCreationDate: "2024-02-01"
  artifactVersion: "1.0"
  classifiedUnderTopic: "Vendor Management and ICT Risk"
  storedInLibrary: "Compliance Knowledge Base"
}

entity ICTResilienceCompetency {
  type: Competency
  competencyName: "ICT Resilience Knowledge"
  competencyType: "Technical Competency"
  competencyProficiencyLevel: "Advanced"
  competencyValidationMethod: "Certification and Internal Assessment"
  competencyApplicationScope: "Organization-Wide"
  requiredByRole: ["ICT Resilience Officer", "ICT Risk Analyst"]
}

entity ThreatSimulationSkill {
  type: Skill
  skillName: "Threat Simulation Execution"
  skillType: "Technical Skill"
  skillProficiencyLevel: "Advanced"
  skillValidationMethod: "Red Team Exercise Certification"
  skillApplicationScope: "Role-Specific"
  requiredByCompetency: "ICT Resilience Knowledge"
}

entity ThirdPartyRiskMeasures {
  type: Measures
  name: "Third-Party Risk Indicators"
  description: "Measure of resilience and risk concentration among critical ICT providers."
  measurementUnit: "%"
  targetValue: "≤ 20% concentration risk exposure"
  actualValue: "18%"
  reportingFrequency: "Annually"
  supportsKPI: "Vendor Resilience KPI"
  linkedToObjectives: "Enhance Third-Party Resilience and Diversification"
}

entity DigitalOperationalResilienceStrategy {
  type: StrategicObjective
  statement: "Establish end-to-end digital operational resilience across all critical financial operations by 2028."
  timeHorizon: 5
  category: "Operational"
  successCriteria: "Zero critical service downtime exceeding regulatory thresholds; 100% critical incident reporting compliance within required timeframes."
  supportsVision: "Operational Resilience Vision 2028"
  drivesStrategy: "Enterprise Resilience Program"
  measuredBy: "Operational Resilience KPIs"
  implementedThrough: "Digital Resilience Action Plan"
}

entity OperationalResilienceVision2028 {
  type: Vision
  statement: "To create a continuously resilient, digitally sovereign financial sector capable of withstanding severe operational disruption by 2028."
  creationDate: "2024-01-15"
  timeHorizon: 5
  coreValues: ["Resilience", "Sovereignty", "Security", "Sustainability"]
}

entity IncidentResponseActionPlan {
  type: ActionPlan
  name: "Major ICT Incident Response Plan"
  description: "Defines escalation, notification, and mitigation activities in response to major ICT incidents."
  startDate: "2025-01-01"
  endDate: "Ongoing"
  owner: "Incident Management Team"
  status: "In Progress"
  supportsInitiative: "Operational Incident Readiness Program"
  includesWorkPackage: [
    "Initial Triage and Incident Classification",
    "Internal and Regulatory Notification",
    "Root Cause Analysis and Reporting",
    "Post-Incident Recovery Execution"
  ]
  monitoredBy: "Incident Response Metrics"
}

entity ICTResilienceKPIs {
  type: Measures
  name: "ICT Resilience KPI Set"
  description: "Key metrics tracking operational resilience, incident frequency, response times, and third-party ICT service reliability."
  measurementUnit: "Score (0-100)"
  targetValue: 95
  actualValue: 88
  reportingFrequency: "Quarterly"
  linkedToObjectives: "Maintain Critical ICT Operations"
  supportsKPI: "Operational Resilience Dashboard"
}

// --- Relationships ---

relationship drivesStrategy OperationalResilienceVision2028 -> DigitalOperationalResilienceStrategy
relationship implementedThrough DigitalOperationalResilienceStrategy -> DigitalResilienceActionPlan
relationship supportsInitiative ResilienceTestingActionPlan -> DigitalOperationalResilienceStrategy
relationship supportsInitiative IncidentResponseActionPlan -> DigitalOperationalResilienceStrategy
relationship enablesProcess ICTRiskManagementFramework -> IncidentResponseActionPlan
relationship enablesProcess ICTRiskManagementFramework -> ResilienceTestingActionPlan
relationship composedOfBusinessUnits ICTGovernanceModel -> ["ICT Risk Unit", "Incident Management Team", "Third-Party Risk Oversight Unit"]
relationship requiredByRole ICTResilienceCompetency -> ["ICT Resilience Officer", "ICT Risk Analyst"]
relationship requiredByCompetency ThreatSimulationSkill -> ICTResilienceCompetency
relationship appliedToProcess ICTIncidentReportingRequirement -> "Incident Notification Workflow"
relationship isMeasuredBy MajorICTIncident -> ICTIncidentMetrics
relationship triggersProcess MajorICTIncident -> "Incident Management Process"
relationship triggers MajorICTIncident -> IncidentResponseActionPlan
relationship isGovernedBy IncidentResponseActionPlan -> "Incident Handling Policy"
