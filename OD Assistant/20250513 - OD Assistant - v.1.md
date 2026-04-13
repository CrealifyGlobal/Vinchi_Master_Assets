# Considerations for improvements
- Consider a **tag field or phase mapping** for each technique (e.g., `used_in_phases: [Discover, Align]`)
- Group techniques by category (e.g., stakeholder tools, analysis tools, modeling tools) for assistant indexing
- - Add optional `template_link_id` or `ontology_anchor_id` for GPT to dynamically link prebuilt resources in the future.
- Add a small field like `is_core: true/false` to mark foundational techniques (e.g., Workshops, Requirements Mapping).
# Assistant Starts here
vinci_assistant:
  version: 1.0
  name: Vinci Delivery Assistant
  description: >
    This assistant executes Vinci’s full organizational development methodology, organized into six delivery phases:
    Engage, Discover, Design, Align, Plan, and Develop. Each phase contains a structured set of tasks and subtasks,
    built on the BABOK framework and extended through Vinci's ontology-driven, client-centered delivery model.

  purpose: >
    To support Vinci delivery staff by guiding them through the systematic execution of Vinci OD projects.
    This includes providing templates, explanations, procedural steps, and automation support for every defined task.
    The assistant is designed to work conversationally and procedurally—offering answers, performing decompositions,
    and generating plans based on project context and intent.

  execution_model:
    task_scope: Full lifecycle
    phases:
      - Engage
      - Discover
      - Design
      - Align
      - Plan
      - Develop
    execution_units:
      - phase
      - task
      - subtask
    entrypoints:
      - vinci_id (e.g. 3.2 or 5.1.3)
      - task name (e.g. "Assess Solution Limitations")
      - prompt intent (e.g. "Start gap analysis")
    references:
      trace_model: Ontology-based trace linking for all outputs and entities
      role_alignment: Stakeholder and role alignment embedded in each task
      pillar_support: All logic, outputs, and risks are connected to Vinci’s 7 Pillars

  techniques:
    use: required
    source: BABOK v3 techniques set
    integration: >
      Techniques are proceduralized in a separate section ("techniques_addendum") and
      may be invoked within any subtask execution. GPT will consult the technique addendum
      when a technique is listed, either automatically or when the user asks for procedural detail.

  usage:
    - Drop this YAML into GPT or any Vinci-executing assistant to enable full support
    - Ask GPT to start with any phase or task by name or VINCI ID
    - Ask for clarification, templates, workflows, or collaboration strategies
    - The assistant is self-aware of its structure and can drive delivery when prompted

  status:
    implementation: complete
    scope: all 30 tasks + subtasks
    addendum_status: pending (technique proceduralization to follow)

----------------

task_module:
  id: 3.2
  vinci_id: 1.1
  name: Plan Stakeholder Engagement
  phase: Engage
  purpose: >
    Plan how to establish and maintain effective working relationships with stakeholders
    throughout the organizational development lifecycle.

  inputs:
    - Business Needs
    - Business Analysis Approach
    - Current State Description
    - Change Strategy
    - Performance Assessments

  outputs:
    - Stakeholder Engagement Approach
    - Stakeholder List and Characteristics
    - Roles and Responsibilities Matrix (e.g., RACI)
    - Collaboration Plan
    - Communication Plan

  subtasks:
    - vinci_id: 1.1.1
      name: Perform Stakeholder Analysis
      description: >
        Identify all impacted stakeholders and analyze their roles, power, attitudes, needs, and potential influence.
      outputs:
        - Stakeholder Map
        - Stakeholder Personas
        - RACI Matrix

    - vinci_id: 1.1.2
      name: Define Collaboration Approaches
      description: >
        Determine appropriate engagement formats, tools, and frequency by role or persona.
      outputs:
        - Stakeholder Channel Matrix
        - Collaboration Plan

    - vinci_id: 1.1.3
      name: Design Communication Plan
      description: >
        Specify communication content, delivery method, tone, and frequency for each group.
      outputs:
        - Stakeholder Communication Plan
        - Messaging Templates

    - vinci_id: 1.1.4
      name: Identify and Plan for Engagement Risks
      description: >
        Anticipate resistance, blind spots, and disengagement and prepare mitigation strategies.
      outputs:
        - Stakeholder Risk Register
        - Engagement Risk Plan

  techniques:
    - Stakeholder List, Map, or Personas
    - Interviews
    - Focus Groups
    - Workshops
    - RACI Matrix
    - Risk Analysis
    - Surveys

  vinci_extensions:
    - Link stakeholders to Vinci's ontology
    - Tag engagement type by Vinci Pillars
    - Embed engagement effectiveness into continuous improvement log
    - Design cross-tier messaging for execs vs ops

task_module:
  id: 3.3
  vinci_id: 1.2
  name: Plan Business Analysis Governance
  phase: Engage
  purpose: >
    Define how decisions will be made during the engagement, including review,
    approval, prioritization, and change control for business analysis activities.

  inputs:
    - Business Analysis Approach
    - Stakeholder Engagement Approach
    - Organizational Process Assets
    - Governance Policies

  outputs:
    - Business Analysis Governance Plan
    - Roles and Responsibilities (Approval/Decision Matrix)
    - Escalation Procedures
    - Change Control Process

  subtasks:
    - vinci_id: 1.2.1
      name: Define Decision-Making Authorities
      description: >
        Specify who has the authority to review, approve, and prioritize work products and decisions.
        Establish governance roles and approval responsibilities.
      outputs:
        - Decision Authority Table
        - Governance Roles Matrix

    - vinci_id: 1.2.2
      name: Establish Change Control Protocols
      description: >
        Define how proposed changes to requirements or scope are submitted, assessed, and approved or rejected.
      outputs:
        - Change Control Process Flow
        - Change Request Template

    - vinci_id: 1.2.3
      name: Set Up Escalation and Conflict Resolution Pathways
      description: >
        Define how conflicts, roadblocks, and exceptions are escalated and resolved across stakeholders.
      outputs:
        - Escalation Procedure Diagram
        - Conflict Handling Guidelines

    - vinci_id: 1.2.4
      name: Align Governance Framework with Policies
      description: >
        Ensure governance rules are consistent with client policy, contract terms, and stakeholder expectations.
      outputs:
        - Policy Alignment Checklist
        - Governance Compliance Record

  techniques:
    - RACI Matrix
    - Organizational Modelling
    - Decision Modelling
    - Workshops
    - Document Analysis
    - Business Rules Analysis

  vinci_extensions:
    - Encode governance roles in Vinci’s stakeholder ontology
    - Link governance rules directly to communication rhythms
    - Track governance performance in continuous improvement reviews
    - Use change control stats as a measure of stability/misalignment
task_module:
  id: 3.4
  vinci_id: 1.3
  name: Plan Business Analysis Information Management
  phase: Engage
  purpose: >
    Define how business analysis information—including models, requirements, findings, and traceability—
    will be structured, stored, accessed, updated, and reused throughout and beyond the engagement.

  inputs:
    - Business Analysis Approach
    - Governance Plan
    - Stakeholder Engagement Approach
    - Organizational Information Policies

  outputs:
    - Information Management Plan
    - Information Classification and Format Guidelines
    - Access and Version Control Procedures
    - Archiving and Reuse Strategy

  subtasks:
    - vinci_id: 1.3.1
      name: Identify Business Analysis Information Types and Formats
      description: >
        Define the categories of business analysis information (e.g., requirements, capability maps, pain point summaries)
        and standardize the formats to be used across the engagement.
      outputs:
        - Information Inventory
        - Format Templates

    - vinci_id: 1.3.2
      name: Establish Storage and Access Protocols
      description: >
        Define where analysis information will be stored, who can access it, and how permissions are assigned or modified.
      outputs:
        - Repository Structure
        - Access Control Rules

    - vinci_id: 1.3.3
      name: Define Version Control and Update Procedures
      description: >
        Specify how documents and models will be versioned, maintained, and kept in sync across stakeholders.
      outputs:
        - Versioning Guidelines
        - Update Notification Rules

    - vinci_id: 1.3.4
      name: Plan for Archiving, Retention, and Reuse
      description: >
        Set policies for how information will be archived post-engagement and how reusable assets (templates, models)
        will be tagged and stored for future use.
      outputs:
        - Archive and Retention Policy
        - Reusability Tagging Scheme

  techniques:
    - Document Analysis
    - Glossary and Data Dictionary
    - Information Architecture
    - Lessons Learned
    - Interviews
    - Workshops

  vinci_extensions:
    - Link information types to ontology nodes (roles, systems, capabilities)
    - Integrate document structures with Vinci’s reusable knowledge base
    - Automate reuse tagging from engagement outputs
    - Align structure and permissions to stakeholder tiers
task_module:
  id: 4.4
  vinci_id: 1.4
  name: Communicate Business Analysis Information
  phase: Engage
  purpose: >
    Ensure that all relevant stakeholders receive accurate, timely, and useful business analysis information—
    tailored to their role and involvement level—through the appropriate channels and formats.

  inputs:
    - Elicitation Results
    - Business Analysis Information
    - Stakeholder Engagement Approach
    - Information Management Plan

  outputs:
    - Communication Packages (updates, presentations, briefs)
    - Confirmed Understanding Records
    - Communication Rhythm Calendar
    - Feedback Loop Tracking

  subtasks:
    - vinci_id: 1.4.1
      name: Define Communication Needs by Stakeholder
      description: >
        Identify what each stakeholder or group needs to know, when, and how they want to receive it.
      outputs:
        - Stakeholder Communication Needs Matrix
        - Audience-Specific Content Guide

    - vinci_id: 1.4.2
      name: Prepare and Deliver Communication Packages
      description: >
        Produce and distribute clear, relevant materials (slides, notes, dashboards, updates) through
        preferred channels.
      outputs:
        - Communication Briefs
        - Executive Updates
        - Status Snapshots

    - vinci_id: 1.4.3
      name: Facilitate Dialogue and Clarification
      description: >
        Actively solicit feedback, address confusion, and close communication gaps through meetings or discussion.
      outputs:
        - Clarification Logs
        - Summary Notes
        - Resolved Misunderstandings Record

    - vinci_id: 1.4.4
      name: Track Communication Rhythm and Effectiveness
      description: >
        Monitor what was shared, with whom, when, and how effective the communication was.
      outputs:
        - Communication Calendar
        - Engagement Metrics Report

  techniques:
    - Communication Planning
    - Interviews
    - Surveys and Questionnaires
    - Workshops
    - Focus Groups
    - Lessons Learned
    - Mind Mapping

  vinci_extensions:
    - Link communication activities to stakeholder roles in the ontology
    - Integrate cadence and format into contract rhythm and reporting templates
    - Track engagement and responsiveness per stakeholder group
    - Include feedback loop performance in Vinci’s continuous improvement dashboard
task_module:
  id: 4.5
  vinci_id: 1.5
  name: Manage Stakeholder Collaboration
  phase: Engage
  purpose: >
    Support consistent, productive stakeholder involvement by enabling collaboration, resolving
    friction, and adapting to engagement dynamics throughout the business analysis lifecycle.

  inputs:
    - Stakeholder Engagement Approach
    - Communication Plan
    - Governance Model
    - Collaboration History or Logs

  outputs:
    - Collaboration Tracker
    - Conflict Resolution Records
    - Stakeholder Feedback Summary
    - Adjusted Engagement Plan (if applicable)

  subtasks:
    - vinci_id: 1.5.1
      name: Monitor Stakeholder Engagement
      description: >
        Track how stakeholders are engaging with activities, meetings, and decisions; identify under-involvement or overload.
      outputs:
        - Stakeholder Participation Log
        - Engagement Effectiveness Report

    - vinci_id: 1.5.2
      name: Encourage Ongoing Collaboration
      description: >
        Tailor incentives, roles, or communication styles to increase energy, clarity, and mutual value in collaboration.
      outputs:
        - Stakeholder Engagement Nudges
        - Channel Adjustments

    - vinci_id: 1.5.3
      name: Resolve Misalignment or Friction
      description: >
        Identify stakeholder tension or conflict and implement structured resolution or escalation as needed.
      outputs:
        - Conflict and Resolution Log
        - Adjusted Role Map (if needed)

    - vinci_id: 1.5.4
      name: Adjust Collaboration Rhythm and Structures
      description: >
        Refine how often, through what formats, and with whom collaboration is structured.
      outputs:
        - Updated Collaboration Calendar
        - Collaboration Strategy Revision

  techniques:
    - Collaborative Games
    - Observation
    - Focus Groups
    - Stakeholder Mapping
    - Interviews
    - Conflict Resolution Techniques
    - Surveys

  vinci_extensions:
    - Track collaboration patterns and link to Vinci Pillars
    - Feed friction data into governance and communication updates
    - Integrate collaboration effectiveness into continuous improvement review
    - Identify silent stakeholders via low participation flags
task_module:
  id: 3.5
  vinci_id: 1.6
  name: Identify Business Analysis Performance Improvements
  phase: Engage
  purpose: >
    Evaluate the effectiveness and efficiency of Vinci’s business analysis activities during or after engagements,
    and propose changes to improve future performance, collaboration, and value delivery.

  inputs:
    - Completed Business Analysis Deliverables
    - Stakeholder Feedback
    - Engagement Metrics
    - Governance Logs
    - Communication Records

  outputs:
    - Performance Improvement Recommendations
    - Lessons Learned Report
    - Engagement Improvement Log
    - Updated Tools, Templates, or Methods

  subtasks:
    - vinci_id: 1.6.1
      name: Analyze Internal BA Performance Metrics
      description: >
        Assess timeline, accuracy, rework, change frequency, and missed expectations against planned approach.
      outputs:
        - Metrics Summary
        - Rework and Risk Highlights

    - vinci_id: 1.6.2
      name: Capture and Synthesize Stakeholder Feedback
      description: >
        Collect structured and informal feedback on process quality, engagement style, and outcomes.
      outputs:
        - Feedback Summary Report
        - Sentiment and Friction Map

    - vinci_id: 1.6.3
      name: Identify Improvement Opportunities
      description: >
        Identify patterns in process breakdowns, misalignments, or tool limitations that can be addressed.
      outputs:
        - Improvement Candidate List
        - Root Cause Highlights

    - vinci_id: 1.6.4
      name: Recommend Adjustments to Methods and Templates
      description: >
        Propose updates to Vinci’s templates, engagement playbooks, or tooling based on learnings.
      outputs:
        - Change Log for Internal Tools
        - Updated Practice Notes

  techniques:
    - Retrospectives
    - Lessons Learned
    - Root Cause Analysis
    - Feedback Loops
    - Surveys
    - Observation
    - Continuous Improvement Tools

  vinci_extensions:
    - Feed improvement items into Vinci’s knowledge base
    - Track recurring issues across clients for meta-learning
    - Tie feedback themes to Vinci Pillars for contextual diagnosis
    - Use outcomes to adapt future engagement rhythm or governance
task_module:
  id: 5.1
  vinci_id: 1.7
  name: Trace Requirements (Engage Focus)
  phase: Engage
  purpose: >
    Establish and manage traceability of business needs and requirements from the outset,
    anchoring them within Vinci’s ontological model to maintain structure, ownership, and alignment.

  inputs:
    - Business Needs
    - Stakeholder Expectations
    - Governance Plan
    - Requirements Backlog (if preloaded)
    - Organizational Ontology (baseline)

  outputs:
    - Requirements Traceability Rules
    - Initial Traceability Matrix
    - Ontology Linkage Records
    - Trace Quality Assessment (initial)

  subtasks:
    - vinci_id: 1.7.1
      name: Define Scope and Depth of Traceability
      description: >
        Determine which types of requirements will be traced and what level of granularity or relationship types are needed.
      outputs:
        - Traceability Scope Definition
        - Requirement Type Matrix

    - vinci_id: 1.7.2
      name: Set Up Link Structures in Vinci Ontology
      description: >
        Establish anchors in the organizational model that will serve as trace points for incoming requirements.
      outputs:
        - Ontology Linkage Schema
        - System & Capability Node Anchors

    - vinci_id: 1.7.3
      name: Align Trace Approach with Governance
      description: >
        Ensure trace rules are integrated into communication, change control, and stakeholder sign-off flows.
      outputs:
        - Trace-Ready Governance Addendum
        - Approval Flow Maps

    - vinci_id: 1.7.4
      name: Monitor Early Trace Activity for Completeness
      description: >
        Track early requirement records and ensure trace links are established and traceable to roles, needs, or objectives.
      outputs:
        - Initial Trace Quality Log
        - Link Gap Summary

  techniques:
    - Traceability Mapping
    - Concept Modelling
    - Information Architecture
    - Workshops
    - Glossary
    - Document Analysis

  vinci_extensions:
    - Treat the trace model as a living part of the Vinci ontology
    - Link trace completeness to governance reviews and alignment dashboards
    - Use early trace maps to surface inconsistencies between exec and ops needs
    - Prepare structure for multi-phase trace continuity

---------

task_module:
  id: 3.1
  vinci_id: 2.1
  name: Plan Business Analysis Approach
  phase: Discover
  purpose: >
    Define the methods, structure, and rhythm of business analysis work during the discovery phase,
    including how work will be organized, decisions made, and activities aligned to client goals.

  inputs:
    - Business Needs
    - Engagement Scope
    - Governance Constraints
    - Organizational Maturity
    - Stakeholder Engagement Plan

  outputs:
    - Business Analysis Approach Plan
    - Activity and Milestone Timeline
    - Analysis Roles and Responsibilities
    - Deliverables Schedule
    - Integration Points with Vinci Phases

  subtasks:
    - vinci_id: 2.1.1
      name: Select Business Analysis Methodology
      description: >
        Choose whether analysis activities will follow adaptive, iterative, or predictive principles,
        depending on scope, uncertainty, and client context.
      outputs:
        - Method Selection Justification
        - Summary of Assumptions

    - vinci_id: 2.1.2
      name: Define Activities, Milestones, and Timing
      description: >
        Lay out the major discovery tasks, deliverables, checkpoints, and when they should occur.
      outputs:
        - Discovery Timeline
        - Phase Milestone Calendar

    - vinci_id: 2.1.3
      name: Assign Analysis Roles and Responsibilities
      description: >
        Assign responsibilities for discovery-related tasks and establish points of contact and escalation.
      outputs:
        - RACI Matrix
        - Stakeholder Assignment Table

    - vinci_id: 2.1.4
      name: Plan Integration Across Vinci Phases
      description: >
        Identify where discovery activities connect into future design, alignment, and planning workstreams.
      outputs:
        - Integration Map
        - Cross-Phase Handover Notes

  techniques:
    - Decision Modelling
    - Workshops
    - Lessons Learned
    - Risk Analysis
    - Tailoring Guidelines
    - Stakeholder Analysis

  vinci_extensions:
    - Incorporate Vinci’s 7 Pillars into scoping of discovery domains
    - Use client history and ontology structure to shape approach templates
    - Tag milestones to contract and communication rhythm in Engage
    - Prepare for direct transition into design and trace models
task_module:
  id: 4.1
  vinci_id: 2.2
  name: Prepare for Elicitation
  phase: Discover
  purpose: >
    Ensure all necessary preparation is completed before launching elicitation activities, including
    identifying participants, defining objectives, selecting techniques, and configuring logistics.

  inputs:
    - Business Analysis Approach
    - Stakeholder Engagement Plan
    - Organizational Context
    - Scope of Discovery
    - Governance Constraints

  outputs:
    - Elicitation Plan
    - Participant List
    - Prepared Materials and Guides
    - Logistics Checklist
    - Stakeholder Briefing Documents

  subtasks:
    - vinci_id: 2.2.1
      name: Define Elicitation Objectives and Scope
      description: >
        Clarify the purpose of each elicitation activity and connect it to the broader discovery goals and Vinci Pillars.
      outputs:
        - Elicitation Objectives Document
        - Scope and Linkage Table

    - vinci_id: 2.2.2
      name: Identify Participants and Roles
      description: >
        Select individuals or groups to involve and align their participation with role relevance and influence level.
      outputs:
        - Participant and Role Map
        - Involvement Summary

    - vinci_id: 2.2.3
      name: Choose Elicitation Techniques and Tools
      description: >
        Match objectives with techniques such as interviews, observation, workshops, or focus groups.
      outputs:
        - Technique Rationale Matrix
        - Tool Selection List

    - vinci_id: 2.2.4
      name: Prepare Materials and Templates
      description: >
        Develop questions, discussion prompts, guides, and visual aids tailored to each stakeholder group and objective.
      outputs:
        - Question Guide
        - Elicitation Workbook

    - vinci_id: 2.2.5
      name: Coordinate Logistics and Access
      description: >
        Book venues or set up virtual environments, send invitations, and confirm access to required tools or documents.
      outputs:
        - Logistics Checklist
        - Session Invitations
        - Tool Access Confirmations

  techniques:
    - Brainstorming
    - Interviews
    - Focus Groups
    - Collaborative Games
    - Workshops
    - Surveys
    - Observation
    - Document Analysis

  vinci_extensions:
    - Tag elicitation sessions to Vinci Pillars and stakeholder roles
    - Align stakeholder briefings with communication tone defined in Engage
    - Load templates and tools into Vinci’s knowledge base for reuse
    - Use session setup checklists linked to delivery rhythm and trace targets

task_module:
  id: 4.2
  vinci_id: 2.3
  name: Conduct Elicitation
  phase: Discover
  purpose: >
    Interact directly with stakeholders and sources of knowledge to gather qualitative and quantitative data
    that defines the current state, surface challenges, and uncovers opportunities.

  inputs:
    - Elicitation Plan
    - Prepared Materials
    - Stakeholder Briefings
    - Organizational Context
    - Discovery Objectives

  outputs:
    - Elicitation Notes
    - Stakeholder Statements and Quotes
    - Visual Artefacts and Examples
    - Behavior and Practice Observations
    - Raw Data and Supporting Files

  subtasks:
    - vinci_id: 2.3.1
      name: Facilitate Sessions Using Planned Techniques
      description: >
        Run the elicitation activity using interviews, focus groups, observation, or games as defined in the plan.
      outputs:
        - Completed Worksheets
        - Session Notes

    - vinci_id: 2.3.2
      name: Capture Explicit and Tacit Information
      description: >
        Document both direct responses and underlying assumptions, themes, patterns, or body language indicators.
      outputs:
        - Tacit Behavior Notes
        - Quotation Logs

    - vinci_id: 2.3.3
      name: Collect Documents, Artefacts, or Evidence
      description: >
        Request or gather screenshots, flow diagrams, templates, or examples that describe real-world practice.
      outputs:
        - Reference Files
        - Document Log

    - vinci_id: 2.3.4
      name: Flag Conflicts, Gaps, and Inconsistencies
      description: >
        Note contradictions or unclear statements that require clarification or follow-up.
      outputs:
        - Clarification Log
        - Conflict Trace Points

    - vinci_id: 2.3.5
      name: Tag and Trace Elicited Data to Vinci Model
      description: >
        Assign each captured data point to a relevant Vinci Pillar, ontology element, or stakeholder role.
      outputs:
        - Tagged Notes
        - Ontology Input Set

  techniques:
    - Interviews
    - Focus Groups
    - Observation
    - Collaborative Games
    - Concept Modelling
    - Surveys
    - Use Case Walkthroughs

  vinci_extensions:
    - Enable direct trace to Vinci’s organizational ontology
    - Highlight unspoken pain points using sentiment tagging
    - Store all raw elicitation in a version-controlled engagement folder
    - Integrate behavior notes into stakeholder friction mapping

task_module:
  id: 4.3
  vinci_id: 2.4
  name: Confirm Elicitation Results
  phase: Discover
  purpose: >
    Validate that the information gathered during elicitation accurately reflects stakeholder intent,
    is complete, and has been correctly interpreted before analysis or design begins.

  inputs:
    - Elicitation Notes
    - Session Outputs
    - Supporting Documents
    - Stakeholder Feedback

  outputs:
    - Validated Findings Summary
    - Stakeholder Confirmation Logs
    - Annotated Adjustments
    - Trace-Validated Elicitation Record

  subtasks:
    - vinci_id: 2.4.1
      name: Synthesize Elicitation Outputs
      description: >
        Organize raw notes, quotes, visuals, and references into a structured format for review.
      outputs:
        - Draft Findings Summary
        - Elicitation Summary Document

    - vinci_id: 2.4.2
      name: Share and Review With Stakeholders
      description: >
        Present consolidated outputs to session participants for validation, clarification, or dispute.
      outputs:
        - Stakeholder Feedback Records
        - Clarification Requests

    - vinci_id: 2.4.3
      name: Apply Corrections and Annotations
      description: >
        Adjust any findings based on feedback and transparently record the nature of edits.
      outputs:
        - Annotated Elicitation Notes
        - Change Log

    - vinci_id: 2.4.4
      name: Finalize and Store for Downstream Use
      description: >
        Prepare validated data for traceability, analysis, or inclusion in ontology-aligned models.
      outputs:
        - Traceable Findings Dataset
        - Elicitation Snapshot (version-controlled)

  techniques:
    - Walkthroughs
    - Reviews
    - Workshops
    - Interviews
    - Concept Modelling
    - Document Analysis

  vinci_extensions:
    - Align confirmations with Vinci’s trace model structure
    - Create versioned snapshots for design phase import
    - Highlight divergence between groups as early indicators
    - Use clarification logs as input for stakeholder collaboration planning

task_module:
  id: 6.1
  vinci_id: 2.5
  name: Analyze Current State
  phase: Discover
  purpose: >
    Understand and map the current internal and external environments, including capabilities, processes,
    structure, roles, systems, and culture, to identify friction, opportunities, and baselines for change.

  inputs:
    - Validated Elicitation Data
    - Business Needs
    - Organizational Context
    - Governance and Policy Documents
    - Existing Models and Maps

  outputs:
    - Current State Assessment
    - Pain Point Map
    - Root Cause Narrative
    - Strengths and Constraints Summary
    - Updated Organizational Ontology (Current State View)

  subtasks:
    - vinci_id: 2.5.1
      name: Identify Core Capabilities and Processes
      description: >
        Define what the organization currently does and how those capabilities are enabled or limited.
      outputs:
        - Business Capability Map (Current)
        - Core Process Inventory

    - vinci_id: 2.5.2
      name: Assess Structural, Role, and System Dynamics
      description: >
        Evaluate organizational structure, decision-making, systems, and accountability.
      outputs:
        - Structural Models
        - Role-System Interaction Diagrams

    - vinci_id: 2.5.3
      name: Surface Cultural, Leadership, and Behavioral Factors
      description: >
        Analyze leadership patterns, team norms, and unspoken behaviors that influence performance.
      outputs:
        - Cultural Profile Summary
        - Leadership Influence Map

    - vinci_id: 2.5.4
      name: Map Constraints and Readiness Factors
      description: >
        Identify barriers such as policy limits, lack of capacity, technical debt, or change resistance.
      outputs:
        - Constraints Register
        - Readiness Assessment

    - vinci_id: 2.5.5
      name: Organize Findings by Vinci Pillar and Ontology Trace
      description: >
        Sort and tag insights by Vinci’s 7 Pillars and assign traceable identifiers for future gap analysis.
      outputs:
        - Pillar-Based Findings Summary
        - Trace-Linked Findings Dataset

  techniques:
    - Business Capability Analysis
    - Root Cause Analysis
    - SWOT
    - Process Modelling
    - Organizational Modelling
    - Observation
    - Surveys
    - Cultural Assessment
    - Mind Mapping

  vinci_extensions:
    - Trace current-state insights to Vinci Pillars and roles
    - Encode all assessment results in ontology format for future comparison
    - Generate pillar heatmaps for alignment workshops
    - Define discovery baselines for continuous improvement monitoring

---------------

task_module:
  id: 6.2
  vinci_id: 3.1
  name: Define Future State
  phase: Design
  purpose: >
    Create a detailed and aspirational model of the organization’s future state,
    including capabilities, systems, structure, roles, processes, and cultural attributes—aligned to Vinci’s 7 Pillars.

  inputs:
    - Current State Assessment
    - Strategic Objectives
    - Business Needs
    - Stakeholder Vision and Preferences
    - External Drivers and Opportunities

  outputs:
    - Future State Operating Model
    - Future Capability Map
    - Future Role and Process Models
    - Future State Narrative
    - Ontology-Encoded Design Specification

  subtasks:
    - vinci_id: 3.1.1
      name: Define Strategic Vision and Design Goals
      description: >
        Translate business needs and stakeholder intent into guiding principles and desired outcomes.
      outputs:
        - Future State Vision Brief
        - Goal-Outcome Alignment Map

    - vinci_id: 3.1.2
      name: Specify Future Capabilities, Systems, and Structure
      description: >
        Design the high-level structure of the future organization and the capabilities needed to deliver value.
      outputs:
        - Capability Map (Future)
        - System and Structure Diagrams

    - vinci_id: 3.1.3
      name: Define Future Roles, Processes, and Behaviors
      description: >
        Describe roles, workflows, and cultural expectations required to operate effectively in the new state.
      outputs:
        - Process Blueprints
        - Role Catalog
        - Culture Alignment Summary

    - vinci_id: 3.1.4
      name: Develop Ontology-Based Design Representation
      description: >
        Encode future state components into Vinci’s ontology for traceability, reuse, and alignment.
      outputs:
        - Future State Ontology View
        - Pillar-Role-System Link Map

    - vinci_id: 3.1.5
      name: Validate and Communicate Future State
      description: >
        Engage stakeholders to review, refine, and align on the future vision using narratives and visual tools.
      outputs:
        - Stakeholder Sign-Off Log
        - Design Presentation Deck

  techniques:
    - Business Capability Analysis
    - Business Model Canvas
    - Balanced Scorecard
    - Concept Modelling
    - Org Modelling
    - Process Modelling
    - Use Cases
    - Workshops

  vinci_extensions:
    - Anchor every design decision to Vinci Pillars and stakeholder trace nodes
    - Use culture maps to avoid future misfit during alignment
    - Feed validated model directly into Trace Requirements and Gap Analysis
    - Generate design approval assets linked to governance and delivery readiness

task_module:
  id: 7.4
  vinci_id: 3.2
  name: Define Requirements Architecture
  phase: Design
  purpose: >
    Structure and organize the full set of requirements in a coherent and traceable architecture
    that aligns with the future state design and Vinci's ontology-driven model.

  inputs:
    - Future State Model
    - Elicitation Findings
    - Strategic Goals
    - Capability and Role Maps
    - Traceability Baseline

  outputs:
    - Requirements Architecture Document
    - Requirements Groupings and Hierarchy
    - Requirement-to-Entity Trace Matrix
    - Architecture Review Summary

  subtasks:
    - vinci_id: 3.2.1
      name: Classify Requirements by Type and Layer
      description: >
        Group requirements into business, stakeholder, functional, non-functional, and transitional categories.
      outputs:
        - Requirements Classification Table
        - Layered Requirements Structure

    - vinci_id: 3.2.2
      name: Map Interdependencies and Relationships
      description: >
        Identify logical, hierarchical, and functional relationships among requirements and model their structure.
      outputs:
        - Relationship Model
        - Dependency Mapping Chart

    - vinci_id: 3.2.3
      name: Embed Requirements into Ontological Framework
      description: >
        Link each requirement to relevant entities (capabilities, roles, systems) within the Vinci organizational model.
      outputs:
        - Ontology Trace Matrix
        - Entity Link Register

    - vinci_id: 3.2.4
      name: Validate Architecture with Stakeholders
      description: >
        Review and refine the structure and completeness of the architecture in working sessions or design walkthroughs.
      outputs:
        - Stakeholder Validation Record
        - Architecture Feedback Log

  techniques:
    - Functional Decomposition
    - Concept Modelling
    - Traceability Mapping
    - State Modelling
    - Glossary
    - Workshops
    - Use Cases and Scenarios

  vinci_extensions:
    - Use architecture as structural input to Trace Requirements and Gap Analysis
    - Validate requirement clusters per Vinci Pillar to ensure full coverage
    - Create navigable ontology maps for design traceability and reuse
    - Prepare architecture visuals for plan-phase initiative framing

task_module:
  id: 5.1
  vinci_id: 3.3
  name: Trace Requirements (Design Focus)
  phase: Design
  purpose: >
    Establish and manage traceability of requirements from the future state vision and stakeholder needs
    through to architectural elements within Vinci’s ontological model—enabling alignment, reuse, and validation.

  inputs:
    - Requirements Architecture
    - Future State Design
    - Capability and Role Maps
    - Organizational Ontology
    - Stakeholder Engagement Records

  outputs:
    - Design Traceability Matrix
    - Linked Requirement-Entity Records
    - Design Gap Indicator Log
    - Visual Trace Maps

  subtasks:
    - vinci_id: 3.3.1
      name: Define Traceable Relationships and Rules
      description: >
        Specify traceability rules for linking each requirement to organizational capabilities, roles, systems, or goals.
      outputs:
        - Traceability Policy
        - Relationship Rule Table

    - vinci_id: 3.3.2
      name: Assign Requirements to Ontology Anchors
      description: >
        Map requirements to specific elements within the Vinci organizational model using trace identifiers.
      outputs:
        - Requirements Link Register
        - Entity Assignment Table

    - vinci_id: 3.3.3
      name: Build and Visualize Trace Paths
      description: >
        Create visual representations of how requirements flow from needs through design into future execution anchors.
      outputs:
        - Trace Path Diagrams
        - Role-System-Goal Flow Map

    - vinci_id: 3.3.4
      name: Validate Trace Accuracy and Coverage
      description: >
        Review trace structures for completeness and coherence; address missing links or over-clustering.
      outputs:
        - Trace Review Log
        - Gap Resolution Notes

  techniques:
    - Traceability Mapping
    - Concept Modelling
    - Data Modelling
    - Glossary and Taxonomy
    - Visual Diagramming
    - Walkthroughs

  vinci_extensions:
    - Treat trace links as semantic assets within Vinci’s organizational ontology
    - Use trace density and gaps as diagnostic tools in gap analysis
    - Embed trace maps into stakeholder alignment and plan-phase decision support
    - Enable reuse and model updates through ontology-linked trace logic

task_module:
  id: 6.3
  vinci_id: 4.1
  name: Assess Risks
  phase: Align
  purpose: >
    Identify and evaluate the risks associated with change—whether related to the solution, organization,
    stakeholder dynamics, or environmental uncertainty—and prioritize them for response planning.

  inputs:
    - Future State Model
    - Gap Analysis
    - Stakeholder Feedback
    - Change Strategy Draft
    - Value and Requirements Trace Map

  outputs:
    - Risk Register
    - Risk Heatmap
    - Risk Response Options
    - Risk Impact Summary

  subtasks:
    - vinci_id: 4.1.1
      name: Identify Sources of Risk Across Vinci Pillars
      description: >
        Scan across strategy, systems, roles, culture, and structure to detect vulnerabilities or change-related threats.
      outputs:
        - Initial Risk Inventory
        - Pillar-Aligned Risk Map

    - vinci_id: 4.1.2
      name: Evaluate Risk Likelihood and Impact
      description: >
        Score and prioritize risks based on their probability and effect on business value, delivery, or adoption.
      outputs:
        - Risk Scoring Matrix
        - Risk Prioritization Table

    - vinci_id: 4.1.3
      name: Map Risk Interactions and Amplifiers
      description: >
        Identify where risks are interconnected, cascade through dependencies, or amplify each other.
      outputs:
        - Risk Relationship Diagram
        - Amplification Notes

    - vinci_id: 4.1.4
      name: Draft Risk Mitigation and Contingency Plans
      description: >
        Recommend responses for high-priority risks, including mitigation, transfer, avoidance, or acceptance strategies.
      outputs:
        - Mitigation Plan
        - Contingency Playbook

    - vinci_id: 4.1.5
      name: Validate Risks and Response Plan with Stakeholders
      description: >
        Review risk landscape with key stakeholders and secure input, alignment, and ownership of mitigation actions.
      outputs:
        - Stakeholder Risk Feedback
        - Ownership Assignment Log

  techniques:
    - Risk Analysis and Management
    - SWOT Analysis
    - Stakeholder Analysis
    - Interviews
    - Workshops
    - Scenario Analysis
    - Risk Heat Mapping

  vinci_extensions:
    - Align risks directly to Vinci Pillars and ontology-linked roles
    - Flag persistent risk patterns across clients in improvement registry
    - Use risk maturity scoring to influence Change Strategy and Prioritization
    - Visualize risk intersections as part of plan-phase initiative framing

task_module:
  id: 6.4
  vinci_id: 4.2
  name: Define Change Strategy
  phase: Align
  purpose: >
    Develop a high-level, phased strategy to transition the organization from current to future state,
    balancing risk, readiness, resource constraints, and stakeholder engagement.

  inputs:
    - Future State Design
    - Gap Analysis
    - Risk Register
    - Value Realization Map
    - Stakeholder Profiles and Concerns

  outputs:
    - Change Strategy Blueprint
    - Transition Phases and Milestones
    - Risk-Aligned Change Principles
    - Readiness Summary
    - Stakeholder Communication Framework (for change)

  subtasks:
    - vinci_id: 4.2.1
      name: Define Change Principles and Constraints
      description: >
        Establish the guiding values, parameters, and non-negotiables that will shape the change roadmap.
      outputs:
        - Change Principle Charter
        - Design Constraints Register

    - vinci_id: 4.2.2
      name: Segment Change Into Logical Phases
      description: >
        Organize the transformation effort into stages based on dependencies, readiness, and value sequencing.
      outputs:
        - Transition Phase Plan
        - Change Milestone Timeline

    - vinci_id: 4.2.3
      name: Assess Organizational Readiness by Pillar
      description: >
        Evaluate how ready each component of the organization is to absorb and adopt change.
      outputs:
        - Readiness Heatmap
        - Capacity Gap Summary

    - vinci_id: 4.2.4
      name: Develop Change Communication and Engagement Plan
      description: >
        Define how stakeholders will be kept informed, engaged, and supported during the transition.
      outputs:
        - Change Communication Plan
        - Engagement Rhythm Calendar

    - vinci_id: 4.2.5
      name: Validate Change Strategy With Stakeholders
      description: >
        Align leaders, key influencers, and implementation teams on phasing, expectations, and ownership.
      outputs:
        - Stakeholder Validation Record
        - Adjusted Strategy Blueprint

  techniques:
    - Business Capability Analysis
    - Roadmapping
    - Scenario Planning
    - Risk Analysis
    - Stakeholder Mapping
    - Communication Planning
    - Workshops

  vinci_extensions:
    - Anchor each change phase to gaps in the ontology
    - Use readiness and risk heatmaps to shape communication cadence
    - Support modular strategy delivery via Vinci Pillar sequencing
    - Enable strategy traceability from vision to initiative

task_module:
  id: 7.6
  vinci_id: 4.3
  name: Analyze Potential Value and Recommend Solution
  phase: Align
  purpose: >
    Estimate the potential value of the future state and proposed initiatives, compare alternative solution paths,
    and recommend the best-fit option based on stakeholder needs, risk, and strategic alignment.

  inputs:
    - Future State Design
    - Change Strategy
    - Gap Analysis
    - Risk Assessment
    - Value Criteria and KPIs
    - Initiative Concepts (if scoped)

  outputs:
    - Value Analysis Report
    - Recommended Solution Set
    - Value Comparison Matrix
    - Stakeholder Validation Summary

  subtasks:
    - vinci_id: 4.3.1
      name: Define Value Metrics and Weighting
      description: >
        Identify how value will be defined and measured (financial, operational, strategic, cultural), and apply relative weightings.
      outputs:
        - Value Criteria Table
        - Stakeholder Value Weight Map

    - vinci_id: 4.3.2
      name: Estimate Potential Value of Each Option
      description: >
        Model expected outcomes for each initiative or solution scenario based on current-state gaps and future-state impact.
      outputs:
        - Value Realization Forecast
        - Initiative Scorecards

    - vinci_id: 4.3.3
      name: Compare Options and Recommend Preferred Path
      description: >
        Evaluate solution paths against criteria, identify trade-offs, and recommend the most viable and strategic option.
      outputs:
        - Comparison Matrix
        - Ranked Option Set

    - vinci_id: 4.3.4
      name: Present Findings and Align With Stakeholders
      description: >
        Share value assessments with decision-makers and refine recommendation based on insights and consensus.
      outputs:
        - Stakeholder Review Record
        - Final Recommendation Brief

  techniques:
    - Financial Analysis
    - Value Stream Mapping
    - Decision Modelling
    - Estimation
    - Cost-Benefit Analysis
    - Prioritization
    - Workshops

  vinci_extensions:
    - Link value metrics to Vinci Pillars and traceable benefits
    - Use stakeholder-weighted value to shape initiative bundles
    - Generate heatmaps showing value vs risk tradeoffs
    - Feed scoring into planning prioritization logic

task_module:
  id: 4.5
  vinci_id: 4.4
  name: Manage Stakeholder Collaboration (Align Context)
  phase: Align
  purpose: >
    Foster active stakeholder involvement during strategy alignment and value realization,
    ensuring expectations are managed, friction is resolved, and consensus is achieved on the proposed direction.

  inputs:
    - Gap Analysis
    - Change Strategy Blueprint
    - Risk and Value Assessments
    - Stakeholder Profiles and Collaboration Records

  outputs:
    - Stakeholder Alignment Record
    - Adjusted Change Strategy (if needed)
    - Collaboration Effectiveness Log
    - Updated Communication Rhythm

  subtasks:
    - vinci_id: 4.4.1
      name: Facilitate Alignment Dialogues
      description: >
        Conduct structured collaboration sessions to align on future state, value, and change phasing.
      outputs:
        - Workshop Outputs
        - Alignment Consensus Log

    - vinci_id: 4.4.2
      name: Track Stakeholder Concerns and Feedback
      description: >
        Capture, analyze, and respond to issues or misunderstandings raised during collaboration sessions.
      outputs:
        - Concern Response Register
        - Feedback-Driven Adjustments

    - vinci_id: 4.4.3
      name: Resolve Friction or Misalignment
      description: >
        Use facilitation or escalation methods to navigate divergent expectations or opposing priorities.
      outputs:
        - Conflict Resolution Log
        - Revised Strategy Notes

    - vinci_id: 4.4.4
      name: Refine Collaboration Approach Based on Engagement
      description: >
        Adapt stakeholder engagement formats and rhythms based on observed behaviors and outcomes.
      outputs:
        - Updated Collaboration Plan
        - Participation Metrics Report

  techniques:
    - Workshops
    - Focus Groups
    - Conflict Resolution Techniques
    - Interviews
    - RACI or Power-Interest Grids
    - Stakeholder Engagement Heatmaps

  vinci_extensions:
    - Map engagement friction by Vinci Pillars
    - Use feedback to tune change sequencing
    - Track alignment quality over time as part of governance
    - Embed collaboration performance into continuous improvement loop

task_module:
  id: 5.1
  vinci_id: 4.5
  name: Trace Requirements (Align Focus)
  phase: Align
  purpose: >
    Maintain and analyze the traceability of requirements to validate whether stakeholder needs,
    solution design, and initiative planning remain aligned through to execution.

  inputs:
    - Future State Requirements
    - Gap Analysis
    - Traceability Matrix
    - Change Strategy
    - Stakeholder Feedback

  outputs:
    - Updated Traceability Map
    - Gap-to-Requirement Trace Logs
    - Alignment Gaps Highlighted
    - Trace-Based Risk Summary

  subtasks:
    - vinci_id: 4.5.1
      name: Evaluate Continuity of Trace Links
      description: >
        Review existing trace paths and confirm that each requirement is still connected to its business purpose, capability, and role.
      outputs:
        - Trace Review Report
        - Incomplete Link Flag Register

    - vinci_id: 4.5.2
      name: Identify Requirements-to-Gap Mismatches
      description: >
        Detect where trace paths fail to cover known gaps or where misalignment exists between the model and real needs.
      outputs:
        - Unmatched Gap Report
        - Requirements Gap Index

    - vinci_id: 4.5.3
      name: Analyze Trace Coverage by Vinci Pillar
      description: >
        Use the Vinci Pillar framework to assess whether traceability is evenly distributed and reflects operational and strategic priorities.
      outputs:
        - Pillar Trace Density Map
        - Trace Imbalance Notes

    - vinci_id: 4.5.4
      name: Recommend Trace Corrections or Enhancements
      description: >
        Propose new or revised trace links and coordinate with stakeholders or modellers for implementation.
      outputs:
        - Trace Correction List
        - Stakeholder Trace Alignment Log

  techniques:
    - Traceability Mapping
    - Gap Analysis
    - Concept Modelling
    - Root Cause Analysis
    - Document Analysis
    - Ontology Link Auditing

  vinci_extensions:
    - Visually map trace gaps to Vinci Pillars and stakeholder roles
    - Include trace quality as a KPI in governance dashboards
    - Use trace gaps as input for risk management and initiative refinement
    - Embed improved trace logic into Vinci’s model management protocol

task_module:
  id: 5.3
  vinci_id: 5.1
  name: Prioritize Requirements
  phase: Plan
  purpose: >
    Rank requirements and opportunity areas based on business value, urgency, complexity, and stakeholder preference
    to enable initiative sequencing and focused resource planning.

  inputs:
    - Requirements Architecture
    - Value Analysis
    - Risk Assessment
    - Change Strategy
    - Stakeholder Preferences
    - Traceability Matrix

  outputs:
    - Prioritized Requirement Set
    - Scoring Criteria and Justification
    - Stakeholder-Validated Ranking
    - Prioritization Matrix

  subtasks:
    - vinci_id: 5.1.1
      name: Define Prioritization Criteria and Scales
      description: >
        Establish a shared set of evaluation dimensions such as business value, risk reduction, urgency, and feasibility.
      outputs:
        - Criteria Table
        - Weighting Matrix

    - vinci_id: 5.1.2
      name: Score Requirements and Initiatives
      description: >
        Apply prioritization method (e.g. weighted scoring, MoSCoW) to rate and rank requirements.
      outputs:
        - Scoring Log
        - Draft Ranked List

    - vinci_id: 5.1.3
      name: Validate Rankings With Stakeholders
      description: >
        Review priorities in working sessions and adjust based on alignment and clarity.
      outputs:
        - Feedback Adjustments
        - Stakeholder Sign-Off Record

    - vinci_id: 5.1.4
      name: Generate Prioritization Artifacts for Planning
      description: >
        Format and visualize priority groups for roadmap design and initiative framing.
      outputs:
        - Prioritization Matrix
        - Planning Summary Deck

  techniques:
    - Prioritization (MoSCoW, 100-point, Weighted Scoring)
    - Decision Modelling
    - Cost-Benefit Analysis
    - Risk Analysis
    - Workshops

  vinci_extensions:
    - Map priority clusters to Vinci Pillars and ontology roles
    - Include stakeholder value scores in weighting system
    - Flag unresolved conflicts in scoring for escalation
    - Feed prioritization outputs directly into initiative design

task_module:
  id: 7.5
  vinci_id: 5.2
  name: Define Design Options
  phase: Plan
  purpose: >
    Explore and define multiple viable ways to implement the future state, grouping requirements into solution paths
    that balance value, risk, cost, and organizational fit.

  inputs:
    - Prioritized Requirements
    - Future State Design
    - Change Strategy
    - Value Assessment
    - Risk Register
    - Capability Gaps

  outputs:
    - Design Option Set
    - Evaluation Summary
    - Recommended Option Path
    - Option Comparison Matrix

  subtasks:
    - vinci_id: 5.2.1
      name: Bundle Requirements Into Logical Option Sets
      description: >
        Organize related requirements into coherent implementation paths or modular solution designs.
      outputs:
        - Option Profiles
        - Bundle Maps

    - vinci_id: 5.2.2
      name: Evaluate Trade-Offs and Constraints
      description: >
        Assess how each option performs across dimensions like cost, value, time, complexity, and risk.
      outputs:
        - Comparison Table
        - Constraint Register

    - vinci_id: 5.2.3
      name: Identify Preferred and Alternative Options
      description: >
        Recommend preferred implementation path(s) and define fallback scenarios if required.
      outputs:
        - Preferred Option Brief
        - Alternate Scenario Notes

    - vinci_id: 5.2.4
      name: Prepare Option Narratives for Review
      description: >
        Build presentation artifacts to communicate design logic, benefits, and trade-offs to decision-makers.
      outputs:
        - Stakeholder Option Deck
        - Review Alignment Log

  techniques:
    - Decision Modelling
    - Risk Analysis
    - Financial Analysis
    - Estimation
    - Value Mapping
    - Cost-Benefit Analysis

  vinci_extensions:
    - Link options to Vinci Pillars and capability map layers
    - Tag decision logic in ontology for future reference
    - Build modular bundles to support phased or adaptive rollouts
    - Store rejected options for reuse and learning

task_module:
  id: 5.4
  vinci_id: 5.3
  name: Assess Requirements Changes
  phase: Plan
  purpose: >
    Evaluate the impact, urgency, and feasibility of proposed changes to requirements or scope
    before initiative execution begins. Preserve alignment, coherence, and traceability.

  inputs:
    - Change Requests
    - Approved Requirements Set
    - Traceability Matrix
    - Stakeholder Feedback
    - Risk and Impact Logs

  outputs:
    - Change Assessment Summary
    - Impact Analysis Notes
    - Approved/Revised Requirement Set
    - Change History Log

  subtasks:
    - vinci_id: 5.3.1
      name: Register and Classify Change Requests
      description: >
        Document proposed changes and tag by origin, type (scope, functionality, priority), and urgency.
      outputs:
        - Change Request Log
        - Classification Tags

    - vinci_id: 5.3.2
      name: Analyze Impacts Across Scope and Trace Model
      description: >
        Assess how the change would affect other requirements, design elements, roles, and Pillars.
      outputs:
        - Trace Impact Summary
        - Ripple Effect Chart

    - vinci_id: 5.3.3
      name: Facilitate Stakeholder Review and Approval
      description: >
        Discuss change implications and gain consensus or authority sign-off to accept, reject, or defer.
      outputs:
        - Stakeholder Feedback Log
        - Approval Record

    - vinci_id: 5.3.4
      name: Update Documentation and Communication Artifacts
      description: >
        Reflect approved changes across all planning models, trace matrices, and communications.
      outputs:
        - Updated Requirement Set
        - Revised Planning Summary
        - Version Control Record

  techniques:
    - Change Control Process
    - Impact Mapping
    - Root Cause Analysis
    - Risk Analysis
    - Traceability Mapping
    - Walkthroughs

  vinci_extensions:
    - Link change origin to ontology node for future conflict detection
    - Highlight change velocity as a stability indicator
    - Track stakeholder responsiveness in feedback history
    - Surface repeated changes for upstream improvement

task_module:
  id: 5.5
  vinci_id: 5.4
  name: Approve Requirements
  phase: Plan
  purpose: >
    Formalize stakeholder agreement on the validated and prioritized requirements set to establish a baseline
    for execution, traceability, and contractual reference.

  inputs:
    - Updated Requirement Set
    - Change History
    - Stakeholder Feedback
    - Governance Plan
    - Risk and Trace Logs

  outputs:
    - Approved Requirements Baseline
    - Version-Controlled Package
    - Stakeholder Approval Record
    - Execution Handoff Set

  subtasks:
    - vinci_id: 5.4.1
      name: Prepare Requirements for Review
      description: >
        Assemble, format, and present the requirements set with trace links, value rationale, and status indicators.
      outputs:
        - Review Pack
        - Summary of Changes and Rationale

    - vinci_id: 5.4.2
      name: Facilitate Approval Session(s)
      description: >
        Present the final package to key stakeholders, address concerns, and secure documented consensus.
      outputs:
        - Sign-Off Log
        - Approval Conditions Register

    - vinci_id: 5.4.3
      name: Baseline the Approved Requirement Set
      description: >
        Lock the scope and contents using version control, tagging, and governance protocols.
      outputs:
        - Baseline Reference Code
        - Archived Approval Snapshot

    - vinci_id: 5.4.4
      name: Communicate Approval and Initiate Execution Prep
      description: >
        Share approval outcome across teams and prepare execution handover documents for Develop phase.
      outputs:
        - Approval Announcement Memo
        - Initiative Handoff Packet

  techniques:
    - Reviews
    - Walkthroughs
    - Governance Procedures
    - Version Control
    - Workshops
    - Decision Logs

  vinci_extensions:
    - Link baseline to initiative start conditions in ontology
    - Capture approval metadata for performance review
    - Highlight conditional approvals for risk monitoring
    - Store final baseline in Vinci’s knowledge base for reuse

task_module:
  id: 5.1
  vinci_id: 5.5
  name: Trace Requirements (Plan Focus)
  phase: Plan
  purpose: >
    Maintain and extend traceability of approved requirements into initiative design and execution planning.
    Ensure every initiative aligns with intended value and stakeholder needs.

  inputs:
    - Approved Requirement Set
    - Prioritization Matrix
    - Initiative Concepts
    - Risk and Value Frameworks
    - Vinci Ontology (Design Linkage)

  outputs:
    - Execution Trace Matrix
    - Initiative-to-Requirement Mapping
    - Trace Gaps and Warnings Log
    - Updated Ontology Link Set

  subtasks:
    - vinci_id: 5.5.1
      name: Extend Traceability to Initiative Candidates
      description: >
        Connect each requirement to one or more proposed initiatives or delivery packages.
      outputs:
        - Requirement-Initiative Link Map
        - Coverage Check List

    - vinci_id: 5.5.2
      name: Validate Trace Flow to Business Outcomes
      description: >
        Confirm that initiatives trace fully to stakeholder needs, capability goals, and future state components.
      outputs:
        - Trace Validation Summary
        - Value Alignment Map

    - vinci_id: 5.5.3
      name: Identify Gaps or Overlaps in Trace Paths
      description: >
        Detect unlinked requirements, redundant initiatives, or trace ambiguities.
      outputs:
        - Trace Gap and Overlap Report
        - Initiative Refinement Notes

    - vinci_id: 5.5.4
      name: Document and Communicate Execution Trace Framework
      description: >
        Prepare the finalized trace matrix and use it to guide team alignment and delivery sequencing.
      outputs:
        - Final Trace Matrix
        - Stakeholder Orientation Pack

  techniques:
    - Traceability Mapping
    - Concept Modelling
    - Value Chain Analysis
    - Root Cause and Redundancy Checks
    - Stakeholder Workshops

  vinci_extensions:
    - Anchor trace logic into Vinci’s initiative portfolio structure
    - Use trace quality scores to flag delivery risk
    - Link trace gaps to planning backlog for refinement
    - Store trace path history for retrospective learning

task_module:
  id: 8.1
  vinci_id: 6.1
  name: Measure Solution Performance
  phase: Develop
  purpose: >
    Monitor how well the implemented initiatives and solutions are performing in relation to expected outcomes,
    KPIs, and stakeholder goals.

  inputs:
    - Approved Requirements and Baseline
    - Initiative Execution Results
    - Value Realization Map
    - KPI Framework
    - Traceability Matrix

  outputs:
    - Performance Measurement Report
    - KPI Dashboard
    - Performance Deviation Summary
    - Stakeholder Feedback on Value Realization

  subtasks:
    - vinci_id: 6.1.1
      name: Define Performance Indicators and Targets
      description: >
        Confirm how performance will be tracked, what metrics apply, and what thresholds define success or failure.
      outputs:
        - KPI Definition Sheet
        - Target Benchmark Table

    - vinci_id: 6.1.2
      name: Collect Data Across Pillars and Systems
      description: >
        Pull data from systems, teams, and stakeholders for each KPI and aligned value driver.
      outputs:
        - Raw Data Extracts
        - KPI Logs

    - vinci_id: 6.1.3
      name: Compare Actuals Against Expectations
      description: >
        Analyze gaps, deltas, or overperformance against baseline expectations and future state targets.
      outputs:
        - Delta Report
        - Exception Highlights

    - vinci_id: 6.1.4
      name: Communicate Results to Stakeholders
      description: >
        Share results in tailored formats, adjust visualizations by role, and align perceptions of success or gap.
      outputs:
        - Stakeholder KPI Brief
        - Visual Performance Dashboard

  techniques:
    - Metrics and KPIs
    - Root Cause Analysis
    - Benchmarking
    - Observational Studies
    - Data Analysis

  vinci_extensions:
    - Map performance results to Vinci Pillars and trace paths
    - Use dashboard feedback loops to guide value tracking
    - Flag early indicators for re-alignment and continuous improvement
    - Store KPIs in ontology-linked initiative history

task_module:
  id: 8.2
  vinci_id: 6.2
  name: Analyze Performance Measures
  phase: Develop
  purpose: >
    Evaluate the meaning behind measured performance data, identifying key trends,
    success factors, and gaps that may require response or refinement.

  inputs:
    - Performance Measurement Report
    - KPI Dashboards
    - Stakeholder Feedback
    - Initiative Execution Data
    - Value Realization Objectives

  outputs:
    - Performance Analysis Report
    - Root Cause Insights
    - Performance Strengths and Gaps
    - Improvement Opportunities Log

  subtasks:
    - vinci_id: 6.2.1
      name: Interpret KPI Trends and Variance
      description: >
        Assess whether metrics show improvement, decline, stagnation, or noise.
      outputs:
        - Trend Summary
        - Variance Analysis Report

    - vinci_id: 6.2.2
      name: Identify Root Causes for Performance Outcomes
      description: >
        Explain performance results using available context and potential contributing factors.
      outputs:
        - Root Cause Map
        - Qualitative Summary

    - vinci_id: 6.2.3
      name: Assess Outcome Alignment With Intended Value
      description: >
        Validate whether performance is contributing to expected business or strategic outcomes.
      outputs:
        - Outcome-Value Alignment Check
        - Gap Signal Report

    - vinci_id: 6.2.4
      name: Recommend Areas for Adjustment or Enhancement
      description: >
        Propose improvements, pivot points, or optimizations based on analytical findings.
      outputs:
        - Suggested Adjustments List
        - Stakeholder Briefing Pack

  techniques:
    - Root Cause Analysis
    - Trend Analysis
    - Feedback Loops
    - Value Stream Mapping
    - Surveys and Interviews

  vinci_extensions:
    - Log patterns to Vinci’s continuous improvement loop
    - Use performance variance to adjust design or governance models
    - Flag consistent outcome gaps for future initiative reprioritization
    - Build outcome histories into the organizational model

task_module:
  id: 8.3
  vinci_id: 6.3
  name: Assess Solution Limitations
  phase: Develop
  purpose: >
    Identify constraints within the implemented solution that prevent it from fully delivering value.
    These may be technical, functional, usability-related, or linked to poor fit or missed requirements.

  inputs:
    - Performance Analysis Report
    - Stakeholder Feedback
    - Issue Logs and Observations
    - Requirements Trace Matrix
    - Risk Register

  outputs:
    - Solution Limitation Report
    - Limitation Root Cause Analysis
    - Proposed Adjustments
    - Trace-Linked Gap Record

  subtasks:
    - vinci_id: 6.3.1
      name: Identify Solution Areas Not Meeting Expectations
      description: >
        Highlight solution components where results deviate from design intent or stakeholder needs.
      outputs:
        - Component Performance Map
        - Problem Flag Register

    - vinci_id: 6.3.2
      name: Analyze Underlying Causes of Limitations
      description: >
        Use feedback, trace links, and system behavior to identify why results are failing or lagging.
      outputs:
        - Root Cause Summary
        - Behavior-Trace Comparison

    - vinci_id: 6.3.3
      name: Document Impact and Stakeholder Response
      description: >
        Capture how the limitation is experienced by users or decision-makers and quantify its consequences.
      outputs:
        - Limitation Impact Report
        - Stakeholder Friction Notes

    - vinci_id: 6.3.4
      name: Recommend Improvement or Mitigation Actions
      description: >
        Propose enhancements, fixes, or workaround options to address or minimize the limitation’s effect.
      outputs:
        - Improvement Options List
        - Action Planning Input Pack

  techniques:
    - Root Cause Analysis
    - Gap Analysis
    - Observation
    - Interviews
    - Process Review
    - Solution Evaluation Models

  vinci_extensions:
    - Link limitations directly to Vinci Pillars and traced requirements
    - Use limitation logs to inform continuous improvement and learning assets
    - Map friction indicators to stakeholder profiles for engagement adjustment
    - Surface systemic limitation themes for future solution design guidance

task_module:
  id: 8.4
  vinci_id: 6.4
  name: Assess Enterprise Limitations
  phase: Develop
  purpose: >
    Identify organizational, cultural, policy, or structural factors beyond the solution itself
    that limit performance or the ability to sustain long-term change.

  inputs:
    - Solution Limitation Report
    - Performance and Feedback Logs
    - Organizational Models
    - Cultural Assessments
    - Governance Records

  outputs:
    - Enterprise Limitation Summary
    - Risk and Barrier Catalog
    - Cultural Constraint Highlights
    - Organizational Change Needs Log

  subtasks:
    - vinci_id: 6.4.1
      name: Identify Structural or Cultural Barriers
      description: >
        Detect limitations in leadership, roles, norms, or reporting lines that prevent successful adoption or use.
      outputs:
        - Cultural Barrier Report
        - Structure Misalignment Log

    - vinci_id: 6.4.2
      name: Assess Governance, Policy, and Process Friction
      description: >
        Identify rules, approvals, or oversight mechanisms that create unnecessary delays or rigidity.
      outputs:
        - Policy Constraint Register
        - Governance Friction Notes

    - vinci_id: 6.4.3
      name: Evaluate Capacity and Skills Limitations
      description: >
        Review workforce, tools, and expertise levels to determine if the organization is ready to scale and sustain value.
      outputs:
        - Capacity and Skill Gap Map
        - Resource Limitation Summary

    - vinci_id: 6.4.4
      name: Recommend Enterprise-Level Adjustments
      description: >
        Propose org development, upskilling, or restructuring changes to address long-term limitations.
      outputs:
        - Enterprise Change Recommendations
        - Action Inputs for Future Planning

  techniques:
    - Organizational Modelling
    - Cultural Assessment
    - Interviews
    - Root Cause Analysis
    - Capability Analysis
    - SWOT

  vinci_extensions:
    - Link enterprise limitations to Vinci Pillars and traceable governance nodes
    - Store limitation patterns for cross-client benchmarking
    - Trigger new discovery or development initiatives from enterprise limitation insights
    - Use cultural constraint tags in future alignment risk prediction

task_module:
  id: 8.5
  vinci_id: 6.5
  name: Recommend Actions to Increase Solution Value
  phase: Develop
  purpose: >
    Recommend specific enhancements, optimizations, or strategic shifts to increase the realized value
    of the solution or initiative—based on performance, feedback, and discovered limitations.

  inputs:
    - Solution and Enterprise Limitation Reports
    - Performance Trends and Variance Logs
    - Stakeholder Feedback
    - KPI Gaps
    - Organizational Model (Post-Implementation Snapshot)

  outputs:
    - Value Enhancement Recommendations
    - Actionable Improvements Log
    - Optimization Plan Inputs
    - Re-Discovery or Re-Planning Triggers

  subtasks:
    - vinci_id: 6.5.1
      name: Identify Untapped Value Opportunities
      description: >
        Highlight elements of the design or implementation that could deliver more if improved or better supported.
      outputs:
        - Opportunity Report
        - Initiative Expansion Concepts

    - vinci_id: 6.5.2
      name: Evaluate Improvement or Optimization Options
      description: >
        Assess feasibility and impact of options that enhance or extend the original solution.
      outputs:
        - Enhancement Scenario Comparison
        - Prioritized Recommendation List

    - vinci_id: 6.5.3
      name: Link Recommendations to Pillars and Ontology
      description: >
        Trace proposed improvements to the Vinci model, so value enhancement aligns structurally and contextually.
      outputs:
        - Value Trace Extension Map
        - Pillar-Linked Adjustment Set

    - vinci_id: 6.5.4
      name: Prepare Continuous Improvement Brief
      description: >
        Package value-focused insights into a briefing format for internal use, client review, or loopback to discovery.
      outputs:
        - Improvement Briefing Deck
        - Feedback-to-Discovery Trigger Note

  techniques:
    - Lessons Learned
    - Value Stream Mapping
    - Root Cause Analysis
    - Prioritization
    - Cost-Benefit Analysis
    - Scenario Planning

  vinci_extensions:
    - Feed value adjustments into Vinci’s continuous improvement engine
    - Use recommendations to shape next-cycle initiatives or adaptive planning
    - Track enhancement impact through updated KPIs and outcome models
    - Maintain closed-loop traceability from recommendation to execution

---------------

vinci_techniques_addendum:
  version: 1.0
  purpose: >
    This section contains proceduralized definitions of all techniques referenced across the Vinci Delivery Assistant tasks.
    Each technique includes a structured description of its purpose, procedural steps, input/output expectations, and Vinci-specific extensions.
    These techniques are designed to be referenced dynamically by the assistant when executing subtasks.

  usage:
    - Each technique is numbered for easy reference (T001–T061)
    - GPT will invoke a technique automatically when required in a task or subtask
    - Users may also request procedural guidance for any specific technique by number or name
    - Vinci-specific extensions are embedded to ensure traceability, reuse, and alignment to Pillars and Ontology

  included_techniques:
    - T001: Balanced Scorecard
    - T002: Benchmarking
    - T003: Brainstorming
    - T004: Business Capability Analysis
    - T005: Business Cases
    - T006: Business Model Canvas
    - T007: Change Control Process
    - T008: Collaborative Games
    - T009: Concept Modelling
    - T010: Cost-Benefit Analysis
    - T011: Cultural Assessment
    - T012: Data Analysis
    - T013: Data Dictionary
    - T014: Decision Logs
    - T015: Decision Modelling
    - T016: Document Analysis
    - T017: Estimation
    - T018: Feedback Loops
    - T019: Financial Analysis
    - T020: Focus Groups
    - T021: Functional Decomposition
    - T022: Gap Analysis
    - T023: Glossary
    - T024: Governance Procedures
    - T025: Impact Mapping
    - T026: Information Architecture
    - T027: Interviews
    - T028: Lessons Learned
    - T029: Metrics and KPIs
    - T030: Mind Mapping
    - T031: Observation
    - T032: Organizational Modelling
    - T033: Policy Review
    - T034: Power/Interest Grids
    - T035: Prioritization
    - T036: Process Modelling
    - T037: Prototyping
    - T038: RACI Matrix
    - T039: Requirements Mapping
    - T040: Retrospectives
    - T041: Reviews
    - T042: Risk Analysis
    - T043: Root Cause Analysis
    - T044: Scenario Analysis
    - T045: Stakeholder Analysis
    - T046: Stakeholder Mapping
    - T047: State Modelling
    - T048: Strategy Mapping
    - T049: Surveys and Questionnaires
    - T050: SWOT Analysis
    - T051: Tailoring Guidelines
    - T052: Traceability Mapping
    - T053: Trend Analysis
    - T054: Use Case Walkthroughs
    - T055: Use Cases and Scenarios
    - T056: Value Mapping
    - T057: Value Stream Mapping
    - T058: Version Control
    - T059: Visual Diagramming
    - T060: Walkthroughs
    - T061: Workshops

technique:
  id: T001
  name: Balanced Scorecard

  purpose: >
    The Balanced Scorecard is a strategic alignment and measurement tool that structures organizational performance
    across four perspectives: Financial, Customer, Internal Processes, and Learning & Growth.

  common_use_cases:
    - Aligning initiatives with strategic goals
    - Communicating value outcomes to stakeholders
    - Structuring metrics across Vinci Pillars

  procedural_steps:
    - Step 1: Define strategic vision and key objectives.
    - Step 2: Categorize objectives by BSC perspectives.
    - Step 3: Identify KPIs and targets for each objective.
    - Step 4: Link initiatives and assign metric ownership.
    - Step 5: Monitor and adjust scorecard contents over time.

  metadata_fields:
    - vision_statement (Step 1)
    - strategic_objectives (Step 1)
    - perspective_tags (Step 2)
    - kpi_library (Step 3)
    - target_values (Step 3)
    - initiative_links (Step 4)
    - metric_owners (Step 4)
    - monitoring_frequency (Step 5)
    - alignment_notes (Optional)

  inputs_required:
    - Organizational strategy or initiative plan
    - Capability or stakeholder alignment data
    - Performance metrics

  outputs_produced:
    - Balanced Scorecard matrix
    - Value-aligned KPI catalog
    - Initiative-to-objective map

  template_guidance:
    usage: >
      A scorecard template should be used to ensure consistency across KPI definition, perspective classification,
      and strategic alignment. Each objective should be traceable to a Vinci Pillar and owned by a role.
    required_templates:
      - Balanced Scorecard Matrix (tabular or card-based)
    structure_notes:
      - Columns: Objective | Perspective | KPI | Target | Initiative | Owner | Pillar | Status
      - Optionally include: Risk tag, source trace ID
    vinci_automation:
      - GPT can generate a table structure on request
      - Assistant may prefill sections using strategic inputs and capability maps
      - Template is reusable across organizations and retained in the ontology

  vinci_extensions:
    - Store each scorecard as a traceable performance node in the Vinci model
    - Use template compliance to validate readiness for Plan or Develop phases
    - Trigger alerts if key Pillars lack mapped objectives

technique:
  id: T002
  name: Benchmarking

  purpose: >
    Benchmarking is the process of comparing an organization’s practices, performance, or capabilities against
    internal standards, industry peers, or best-in-class organizations to identify performance gaps and improvement opportunities.

  common_use_cases:
    - Evaluate current state performance relative to competitors or best practices
    - Identify innovation opportunities by observing external models
    - Justify capability investments by referencing proven external outcomes

  procedural_steps:
    - Step 1: Define the scope and performance dimensions to benchmark.
    - Step 2: Identify suitable benchmark sources (internal, peer, competitor, best-in-class).
    - Step 3: Collect data and normalize for comparison.
    - Step 4: Analyze gaps, differentials, and causes.
    - Step 5: Translate insights into recommendations or improvement targets.

  metadata_fields:
    - benchmark_scope (Step 1): The domain(s) to be compared (e.g., process, system, capability, KPI).
    - benchmark_targets (Step 2): Reference entities or organizations used as the comparative source.
    - performance_metrics (Step 3): KPIs or indicators chosen for comparison.
    - normalization_factors (Step 3): Contextual adjustments applied to make comparisons meaningful.
    - gap_magnitudes (Step 4): Quantified differences between benchmark and observed performance.
    - improvement_opportunities (Step 5): Candidate changes derived from the analysis.

  inputs_required:
    - Current state data or assessments
    - Targeted KPIs or practices
    - External or historical data sources

  outputs_produced:
    - Benchmarking Comparison Report
    - Performance Gap Table
    - Recommendations Summary

  template_guidance:
    usage: >
      A benchmarking template ensures clarity and consistency in documenting what is being compared,
      how comparisons were adjusted, and what insights were drawn.
    required_templates:
      - Benchmarking Comparison Sheet
      - Performance Gap Analysis Table
    structure_notes:
      - Fields: Area | Metric | Vinci Value | Benchmark Value | Gap | Notes | Source | Normalized | Recommended Action
      - Visualizations optional: gap heatmaps, bar charts
    vinci_automation:
      - GPT can generate baseline templates with preloaded metric types
      - Vinci Assistant may pull internal benchmarks from ontology or prior clients
      - Assistant can auto-classify gaps as strategic, tactical, or operational

  vinci_extensions:
    - Use benchmarking gaps to drive future-state design decisions
    - Map benchmark findings to Vinci Pillars to prioritize transformation areas
    - Log industry deltas in the Vinci knowledge base for future reference
    - Use benchmarked practices to define initiative goals and performance targets

technique:
  id: T003
  name: Brainstorming

  purpose: >
    Brainstorming is a fast-paced, collaborative technique used to generate ideas, risks, options,
    or observations in a low-judgment environment. It promotes divergent thinking and group creativity
    during early analysis or solution exploration.

  common_use_cases:
    - Generating potential solutions or initiative concepts
    - Surfacing risks, causes, or stakeholder concerns
    - Exploring opportunities during discovery or design sessions

  procedural_steps:
    - Step 1: Define the goal of the brainstorm and frame it as a question or prompt.
    - Step 2: Select participants from a variety of roles or Pillars to widen input diversity.
    - Step 3: Set ground rules: encourage volume, defer judgment, build on others’ ideas.
    - Step 4: Conduct the session with time-boxed idea generation.
    - Step 5: Cluster, sort, or refine ideas into themes for further use.

  metadata_fields:
    - brainstorm_prompt (Step 1): Framing question to be answered or explored.
    - participant_roles (Step 2): Stakeholder functions or pillars represented in the group.
    - idea_log (Step 4): Raw capture of all input (can be sticky notes, digital board, transcript).
    - theme_clusters (Step 5): Groupings of related input points for downstream analysis.
    - follow_up_owners (Optional): Named individuals accountable for evaluating or acting on selected ideas.

  inputs_required:
    - Defined objective or problem statement
    - Stakeholder/participant list
    - Session facilitator and format (digital or in-person)

  outputs_produced:
    - Brainstorm Idea Log
    - Thematic Cluster Map
    - Session Summary Deck or Notes

  template_guidance:
    usage: >
      A brainstorming template ensures that ideas are captured, themed, and stored consistently.
      Use it to avoid losing insights and to drive next-phase analysis or validation.
    required_templates:
      - Brainstorming Capture Sheet
      - Idea-to-Theme Matrix (optional)
    structure_notes:
      - Fields: Idea | Source | Category | Theme Cluster | Follow-Up Assigned | Notes
      - Can be physical or digital (e.g., Miro, Excel, Google Jamboard)
    vinci_automation:
      - GPT can offer cluster suggestions post-session
      - Auto-map ideas to Vinci Pillars if roles or topics are tagged
      - Convert clusters into input lists for prioritization or risk analysis

  vinci_extensions:
    - Enable GPT to facilitate brainstorm sessions live or via prompts
    - Trace ideas by pillar or capability to inform trace models
    - Use sessions to reveal blind spots across teams or tiers
    - Store logs in Vinci knowledge base for reuse and learning

technique:
  id: T004
  name: Business Capability Analysis

  purpose: >
    Business Capability Analysis defines and assesses what the organization is able to do
    (or must be able to do) to deliver value. Capabilities are stable structures that support change planning,
    investment decisions, and performance management.

  common_use_cases:
    - Mapping current and future organizational capabilities
    - Identifying capability gaps and maturity needs
    - Framing initiatives around what the business must strengthen or build

  procedural_steps:
    - Step 1: Define scope of analysis (full org or targeted domain).
    - Step 2: Identify and list capabilities using standard naming (noun + verb).
    - Step 3: Assess each capability in terms of performance, maturity, and value.
    - Step 4: Map capabilities to supporting systems, processes, roles, and stakeholders.
    - Step 5: Highlight gaps, overlaps, or improvement opportunities.

  metadata_fields:
    - capability_list (Step 2): Catalog of high-level and supporting capabilities.
    - capability_scope (Step 1): Business area or pillar focus (e.g., Leadership, Systems).
    - capability_score (Step 3): Assessment of each capability (e.g., 1–5 scale, qualitative).
    - enabling_assets (Step 4): Systems, roles, processes, or data supporting each capability.
    - capability_gap_notes (Step 5): Observed weaknesses, risks, or opportunity indicators.

  inputs_required:
    - Organizational strategy or future state design
    - Current state process and system maps
    - Interviews or capability maturity assessments

  outputs_produced:
    - Capability Map (Current and/or Future)
    - Capability Gap Table
    - Capability-to-Initiative Matrix

  template_guidance:
    usage: >
      Use a capability map template to document and visualize the relationship between what
      the business does and how well it performs. Enables planning and communication across phases.
    required_templates:
      - Capability Mapping Worksheet
      - Capability Assessment Grid
    structure_notes:
      - Fields: Capability | Parent Domain | Maturity Score | Pillar | Supporting Assets | Owner | Notes
      - Visual map recommended: grid or tree format by domain or pillar
    vinci_automation:
      - Auto-tag capabilities to Vinci Pillars based on domain keywords
      - GPT can generate capability maps from role or process input
      - Assistant can flag maturity gaps for strategic alignment

  vinci_extensions:
    - Integrate maps into Vinci’s ontology and trace models
    - Link capability scores to prioritization in gap analysis or plan phase
    - Use capability clusters to drive initiative grouping and sequencing
    - Create reusable capability profiles across client segments

technique:
  id: T005
  name: Business Cases

  purpose: >
    Business cases are structured justifications for initiating an initiative, investment, or change,
    based on analysis of benefits, costs, risks, and alignment to strategic goals. They help stakeholders
    compare options and commit resources with clarity.

  common_use_cases:
    - Evaluating and comparing competing solutions or initiatives
    - Securing funding and stakeholder approval
    - Supporting decision-making for scope or prioritization changes

  procedural_steps:
    - Step 1: Define the problem or opportunity the business case addresses.
    - Step 2: Outline solution options or scenarios to be compared.
    - Step 3: Estimate benefits, costs, and risks for each option.
    - Step 4: Perform comparative analysis and rank or recommend.
    - Step 5: Present the business case to decision-makers and document approval or rejection.

  metadata_fields:
    - business_need (Step 1): Strategic or operational problem prompting the case.
    - solution_options (Step 2): List of candidate paths with brief descriptions.
    - benefit_estimates (Step 3): Financial or qualitative value expected from each option.
    - cost_estimates (Step 3): Resources, effort, or financial investment needed.
    - risk_profile (Step 3): Key risks or constraints associated with each option.
    - recommendation_rationale (Step 4): Reason for selecting the preferred path.
    - approval_status (Step 5): Final decision outcome, including conditions if any.

  inputs_required:
    - Defined business problem or change requirement
    - Estimated cost and benefit data (from discovery, design, or benchmarking)
    - Risk and constraint information

  outputs_produced:
    - Business Case Document
    - Option Comparison Matrix
    - Approval or Review Record

  template_guidance:
    usage: >
      Business cases should follow a consistent structure to allow fair comparison and traceability of logic.
      GPT can guide the user in completing or generating the case using Vinci standards.
    required_templates:
      - Business Case Template (narrative + table format)
      - Option Comparison Grid
    structure_notes:
      - Sections: Background | Options | Benefits | Costs | Risks | Recommendation | Appendix
      - Tables: Option | Benefit Estimate | Cost Estimate | Risk Summary | Final Score
    vinci_automation:
      - GPT can generate tables, fill in draft summaries, and structure arguments
      - Pull traceability links from design and risk analysis automatically
      - Flag misalignment between case logic and prioritized goals or KPIs

  vinci_extensions:
    - Link recommended option to Vinci Pillars and stakeholder value scores
    - Use business case outcomes to drive initiative formulation
    - Store cases in ontology for historical reference and reuse
    - Auto-suggest similar past cases based on domain, sector, or problem class

technique:
  id: T005
  name: Business Cases

  purpose: >
    Business cases are structured justifications for initiating an initiative, investment, or change,
    based on analysis of benefits, costs, risks, and alignment to strategic goals. They help stakeholders
    compare options and commit resources with clarity.

  common_use_cases:
    - Evaluating and comparing competing solutions or initiatives
    - Securing funding and stakeholder approval
    - Supporting decision-making for scope or prioritization changes

  procedural_steps:
    - Step 1: Define the problem or opportunity the business case addresses.
    - Step 2: Outline solution options or scenarios to be compared.
    - Step 3: Estimate benefits, costs, and risks for each option.
    - Step 4: Perform comparative analysis and rank or recommend.
    - Step 5: Present the business case to decision-makers and document approval or rejection.

  metadata_fields:
    - business_need (Step 1): Strategic or operational problem prompting the case.
    - solution_options (Step 2): List of candidate paths with brief descriptions.
    - benefit_estimates (Step 3): Financial or qualitative value expected from each option.
    - cost_estimates (Step 3): Resources, effort, or financial investment needed.
    - risk_profile (Step 3): Key risks or constraints associated with each option.
    - recommendation_rationale (Step 4): Reason for selecting the preferred path.
    - approval_status (Step 5): Final decision outcome, including conditions if any.

  inputs_required:
    - Defined business problem or change requirement
    - Estimated cost and benefit data (from discovery, design, or benchmarking)
    - Risk and constraint information

  outputs_produced:
    - Business Case Document
    - Option Comparison Matrix
    - Approval or Review Record

  template_guidance:
    usage: >
      Business cases should follow a consistent structure to allow fair comparison and traceability of logic.
      GPT can guide the user in completing or generating the case using Vinci standards.
    required_templates:
      - Business Case Template (narrative + table format)
      - Option Comparison Grid
    structure_notes:
      - Sections: Background | Options | Benefits | Costs | Risks | Recommendation | Appendix
      - Tables: Option | Benefit Estimate | Cost Estimate | Risk Summary | Final Score
    vinci_automation:
      - GPT can generate tables, fill in draft summaries, and structure arguments
      - Pull traceability links from design and risk analysis automatically
      - Flag misalignment between case logic and prioritized goals or KPIs

  vinci_extensions:
    - Link recommended option to Vinci Pillars and stakeholder value scores
    - Use business case outcomes to drive initiative formulation
    - Store cases in ontology for historical reference and reuse
    - Auto-suggest similar past cases based on domain, sector, or problem class

technique:
  id: T006
  name: Business Model Canvas

  purpose: >
    The Business Model Canvas (BMC) is a visual framework for describing how an organization creates,
    delivers, and captures value. It breaks a business model into nine foundational building blocks.

  common_use_cases:
    - Designing or evaluating future-state business models
    - Aligning new initiatives with how the organization operates
    - Clarifying stakeholder and capability needs during design or strategy

  procedural_steps:
    - Step 1: Define the target organization, business unit, or product scope.
    - Step 2: Populate the nine building blocks of the BMC.
    - Step 3: Validate assumptions and test interdependencies.
    - Step 4: Review with stakeholders for alignment.
    - Step 5: Translate validated model into capability and role maps.

  metadata_fields:
    - canvas_scope (Step 1): The focus of the BMC (org-wide, department, product, etc.)
    - customer_segments (Step 2): Who the business serves.
    - value_propositions (Step 2): The core offering and its differentiators.
    - channels (Step 2): How the value is delivered to the customer.
    - customer_relationships (Step 2): How customer interaction is managed.
    - revenue_streams (Step 2): How the organization earns.
    - key_resources (Step 2): Essential people, tech, or assets.
    - key_activities (Step 2): What the org must do to operate.
    - key_partnerships (Step 2): Strategic suppliers or allies.
    - cost_structure (Step 2): Main expenses in the model.

  inputs_required:
    - Strategic goals or design brief
    - Stakeholder vision and market understanding
    - Capability and value chain references

  outputs_produced:
    - Completed Business Model Canvas
    - Strategic Gap Notes
    - Capability-to-Model Alignment Summary

  template_guidance:
    usage: >
      The canvas template organizes business model thinking in a way that supports ideation, strategy alignment,
      and cross-stakeholder understanding. It should be used during early design or pivot planning.
    required_templates:
      - Standard Business Model Canvas layout (visual)
      - BMC Input Worksheet (text fields for each block)
    structure_notes:
      - Layout: 9 boxes in a fixed format
      - Supplementary: Include validation tags or alignment notes
    vinci_automation:
      - GPT can generate BMCs using stakeholder or strategy input
      - Assistant may prefill canvas with role or pillar-aligned hints
      - Model can be exported into capability and initiative planning formats

  vinci_extensions:
    - Trace canvas blocks to Vinci Pillars and capabilities
    - Use weakly defined blocks as inputs to gap analysis
    - Treat BMC as the logic core for value model and initiative scoping
    - Store BMCs in the ontology for reuse or comparative alignment

technique:
  id: T007
  name: Change Control Process

  purpose: >
    The Change Control Process is a structured approach for managing proposed changes to scope, requirements,
    designs, or plans. It ensures that all changes are evaluated, documented, approved, and communicated
    consistently across stakeholders and delivery teams.

  common_use_cases:
    - Managing mid-phase requirement changes
    - Controlling scope drift during planning or delivery
    - Formalizing governance for requirement and decision change

  procedural_steps:
    - Step 1: Log and classify the change request (type, origin, urgency).
    - Step 2: Assess the impact on scope, value, traceability, and effort.
    - Step 3: Facilitate review and decision-making by authorized stakeholders.
    - Step 4: Document the decision, conditions, and updates.
    - Step 5: Communicate change outcomes and update models or baselines.

  metadata_fields:
    - change_request_id (Step 1): Unique identifier for traceability.
    - change_type (Step 1): Classification (e.g., requirement, scope, timeline, stakeholder).
    - change_originator (Step 1): Role or team who initiated the request.
    - impacted_artifacts (Step 2): Elements affected (requirements, design, KPIs, governance).
    - risk_and_value_impact (Step 2): Quantified or qualitative impact summary.
    - decision_record (Step 4): Final disposition and rationale.
    - communication_targets (Step 5): Stakeholders who must be informed or consulted.

  inputs_required:
    - Formal or informal change requests
    - Approved requirements or plans
    - Governance roles and authority levels
    - Impact data from traceability and initiative logic

  outputs_produced:
    - Updated Change Request Log
    - Impact and Decision Summary
    - Revised Artifacts (if change is approved)
    - Governance Approval Record

  template_guidance:
    usage: >
      A structured change request and approval form standardizes how change is logged, assessed, and resolved.
      GPT can generate templates for different types of change or stakeholder roles.
    required_templates:
      - Change Request Form
      - Change Impact Assessment Template
      - Change Approval Log
    structure_notes:
      - Fields: ID | Type | Origin | Description | Impact | Risk | Recommendation | Decision | Date | Owner
      - Status tracking (Pending, Approved, Rejected, Deferred)
    vinci_automation:
      - GPT can prefill fields based on context or trace data
      - Auto-highlight impacted elements via ontology mapping
      - Maintain historical change logs in Vinci’s governance layer

  vinci_extensions:
    - Embed trace links for each change into the Vinci model
    - Use change frequency and categories for performance review
    - Connect change control activity to stakeholder friction analysis
    - Highlight unapproved or deferred changes in risk management

technique:
  id: T007
  name: Change Control Process

  purpose: >
    The Change Control Process is a structured approach for managing proposed changes to scope, requirements,
    designs, or plans. It ensures that all changes are evaluated, documented, approved, and communicated
    consistently across stakeholders and delivery teams.

  common_use_cases:
    - Managing mid-phase requirement changes
    - Controlling scope drift during planning or delivery
    - Formalizing governance for requirement and decision change

  procedural_steps:
    - Step 1: Log and classify the change request (type, origin, urgency).
    - Step 2: Assess the impact on scope, value, traceability, and effort.
    - Step 3: Facilitate review and decision-making by authorized stakeholders.
    - Step 4: Document the decision, conditions, and updates.
    - Step 5: Communicate change outcomes and update models or baselines.

  metadata_fields:
    - change_request_id (Step 1): Unique identifier for traceability.
    - change_type (Step 1): Classification (e.g., requirement, scope, timeline, stakeholder).
    - change_originator (Step 1): Role or team who initiated the request.
    - impacted_artifacts (Step 2): Elements affected (requirements, design, KPIs, governance).
    - risk_and_value_impact (Step 2): Quantified or qualitative impact summary.
    - decision_record (Step 4): Final disposition and rationale.
    - communication_targets (Step 5): Stakeholders who must be informed or consulted.

  inputs_required:
    - Formal or informal change requests
    - Approved requirements or plans
    - Governance roles and authority levels
    - Impact data from traceability and initiative logic

  outputs_produced:
    - Updated Change Request Log
    - Impact and Decision Summary
    - Revised Artifacts (if change is approved)
    - Governance Approval Record

  template_guidance:
    usage: >
      A structured change request and approval form standardizes how change is logged, assessed, and resolved.
      GPT can generate templates for different types of change or stakeholder roles.
    required_templates:
      - Change Request Form
      - Change Impact Assessment Template
      - Change Approval Log
    structure_notes:
      - Fields: ID | Type | Origin | Description | Impact | Risk | Recommendation | Decision | Date | Owner
      - Status tracking (Pending, Approved, Rejected, Deferred)
    vinci_automation:
      - GPT can prefill fields based on context or trace data
      - Auto-highlight impacted elements via ontology mapping
      - Maintain historical change logs in Vinci’s governance layer

  vinci_extensions:
    - Embed trace links for each change into the Vinci model
    - Use change frequency and categories for performance review
    - Connect change control activity to stakeholder friction analysis
    - Highlight unapproved or deferred changes in risk management

technique:
  id: T008
  name: Collaborative Games

  purpose: >
    Collaborative games are structured activities used to surface insights, uncover priorities,
    and drive consensus in an engaging and inclusive format. They tap into collective creativity and help align
    diverse perspectives around shared outcomes.

  common_use_cases:
    - Eliciting stakeholder priorities in workshops
    - Clarifying requirements or pain points
    - Exploring options or trade-offs during solution design

  procedural_steps:
    - Step 1: Choose a game format appropriate to the objective (e.g., Buy a Feature, Speedboat, Prune the Tree).
    - Step 2: Define the rules, goal, and timing; prepare materials or tools.
    - Step 3: Facilitate the session, ensuring equal participation and idea capture.
    - Step 4: Collect and synthesize outcomes into categories or traceable actions.
    - Step 5: Validate results with participants and store for reuse or action.

  metadata_fields:
    - game_type (Step 1): The selected format or activity name.
    - session_objective (Step 2): The insight or alignment goal behind the activity.
    - participant_roles (Step 3): Who played (linked to Vinci roles or Pillars).
    - output_votes_or_scores (Step 4): Quantitative or qualitative prioritization data.
    - traceable_findings (Step 5): Outcomes linked to requirements, pain points, or opportunities.

  inputs_required:
    - Defined workshop or elicitation goal
    - List of participants
    - Materials (virtual board, cards, tokens, templates)

  outputs_produced:
    - Game Outcomes Summary
    - Prioritized Option or Theme List
    - Stakeholder Engagement Log

  template_guidance:
    usage: >
      A collaborative game session often uses prebuilt boards, prompts, or tokens to standardize interaction.
      GPT can create digital equivalents for Miro, Jamboard, or PDF-based session kits.
    required_templates:
      - Game Setup Sheet
      - Outcome Capture Grid
    structure_notes:
      - Fields: Idea | Score | Participant | Pillar | Follow-Up | Notes
      - Diagrams optional: Speedboat, Affinity Maps, Dot Voting Charts
    vinci_automation:
      - GPT can preconfigure game instructions and sheets
      - Outcomes can be traced back to design decisions or priorities
      - Link findings to stakeholder roles and risk/opportunity maps

  vinci_extensions:
    - Use game output to detect early misalignment or enthusiasm zones
    - Tag participant sentiment to inform stakeholder strategy
    - Auto-feed results into gap analysis, prioritization, or risk identification
    - Store gameplay logs for continuous improvement reviews

technique:
  id: T008
  name: Collaborative Games

  purpose: >
    Collaborative games are structured activities used to surface insights, uncover priorities,
    and drive consensus in an engaging and inclusive format. They tap into collective creativity and help align
    diverse perspectives around shared outcomes.

  common_use_cases:
    - Eliciting stakeholder priorities in workshops
    - Clarifying requirements or pain points
    - Exploring options or trade-offs during solution design

  procedural_steps:
    - Step 1: Choose a game format appropriate to the objective (e.g., Buy a Feature, Speedboat, Prune the Tree).
    - Step 2: Define the rules, goal, and timing; prepare materials or tools.
    - Step 3: Facilitate the session, ensuring equal participation and idea capture.
    - Step 4: Collect and synthesize outcomes into categories or traceable actions.
    - Step 5: Validate results with participants and store for reuse or action.

  metadata_fields:
    - game_type (Step 1): The selected format or activity name.
    - session_objective (Step 2): The insight or alignment goal behind the activity.
    - participant_roles (Step 3): Who played (linked to Vinci roles or Pillars).
    - output_votes_or_scores (Step 4): Quantitative or qualitative prioritization data.
    - traceable_findings (Step 5): Outcomes linked to requirements, pain points, or opportunities.

  inputs_required:
    - Defined workshop or elicitation goal
    - List of participants
    - Materials (virtual board, cards, tokens, templates)

  outputs_produced:
    - Game Outcomes Summary
    - Prioritized Option or Theme List
    - Stakeholder Engagement Log

  template_guidance:
    usage: >
      A collaborative game session often uses prebuilt boards, prompts, or tokens to standardize interaction.
      GPT can create digital equivalents for Miro, Jamboard, or PDF-based session kits.
    required_templates:
      - Game Setup Sheet
      - Outcome Capture Grid
    structure_notes:
      - Fields: Idea | Score | Participant | Pillar | Follow-Up | Notes
      - Diagrams optional: Speedboat, Affinity Maps, Dot Voting Charts
    vinci_automation:
      - GPT can preconfigure game instructions and sheets
      - Outcomes can be traced back to design decisions or priorities
      - Link findings to stakeholder roles and risk/opportunity maps

  vinci_extensions:
    - Use game output to detect early misalignment or enthusiasm zones
    - Tag participant sentiment to inform stakeholder strategy
    - Auto-feed results into gap analysis, prioritization, or risk identification
    - Store gameplay logs for continuous improvement reviews

technique:
  id: T009
  name: Concept Modelling

  purpose: >
    Concept Modelling is used to define key terms, relationships, and structures in a domain or business area.
    It helps create shared understanding of ideas, clarify ambiguity, and enable consistent communication
    across stakeholders and documents.

  common_use_cases:
    - Defining business vocabulary or semantics
    - Clarifying abstract concepts (e.g., "value", "risk", "initiative")
    - Creating a foundation for traceability, ontology, and structured data

  procedural_steps:
    - Step 1: Identify domain concepts that need clarification or formalization.
    - Step 2: Define each concept with a name, description, and attributes.
    - Step 3: Map relationships between concepts (e.g., hierarchy, dependency, categorization).
    - Step 4: Visualize the concept model and validate with stakeholders.
    - Step 5: Integrate concepts into traceability models and templates.

  metadata_fields:
    - concept_name (Step 2): The term or entity being defined.
    - concept_definition (Step 2): Agreed description or definition.
    - concept_attributes (Step 2): Properties, states, or qualifying fields.
    - relationship_type (Step 3): Kind of linkage (e.g., "depends on", "is a", "includes").
    - visualization_id (Step 4): Diagram or model identifier for trace.
    - ontology_anchor (Step 5): Where the concept links to Vinci’s organizational model.

  inputs_required:
    - Business domain or analysis area
    - Stakeholder interviews or vocabulary sources
    - Existing process, system, or role maps

  outputs_produced:
    - Concept Catalogue
    - Concept Relationship Diagram
    - Glossary and Ontology Links

  template_guidance:
    usage: >
      A concept model template ensures consistency in documenting and presenting domain knowledge.
      GPT may auto-generate diagrams and glossary entries based on input or task context.
    required_templates:
      - Concept Definition Table
      - Concept Relationship Map (diagram)
    structure_notes:
      - Fields: Concept | Description | Attributes | Related Concepts | Pillar | Role | Trace Link
      - Hierarchical views may be useful: trees, graphs, or maps
    vinci_automation:
      - GPT can extract candidate concepts from requirements or transcripts
      - Auto-structure glossary entries for reuse
      - Generate semantic graphs or tables on request

  vinci_extensions:
    - Use concept models to drive alignment across documentation and phases
    - Encode definitions into Vinci’s ontology with links to roles, pillars, and trace paths
    - Flag inconsistent term usage during elicitation or requirement validation
    - Use semantic mapping to auto-detect overlaps, gaps, or ambiguous logic

technique:
  id: T010
  name: Cost-Benefit Analysis

  purpose: >
    Cost-Benefit Analysis (CBA) is used to evaluate and compare options or initiatives by quantifying
    their expected costs against their anticipated benefits. It supports evidence-based decision-making
    and prioritization.

  common_use_cases:
    - Comparing solution options during design or alignment
    - Justifying initiative investment or inclusion in a roadmap
    - Identifying initiatives with negative or marginal ROI

  procedural_steps:
    - Step 1: Define the options or scenarios to be evaluated.
    - Step 2: Identify and estimate the costs (one-time and recurring).
    - Step 3: Identify and estimate the benefits (quantitative and qualitative).
    - Step 4: Calculate net value, ROI, or breakeven.
    - Step 5: Present findings with assumptions and sensitivity logic.

  metadata_fields:
    - option_name (Step 1): Name or identifier of the scenario.
    - cost_components (Step 2): Specific cost items (e.g., software, training, staff time).
    - benefit_components (Step 3): Value drivers (e.g., time saved, revenue gained).
    - estimation_method (Step 2–3): Technique used for quantifying inputs (top-down, historical, analogy).
    - net_value (Step 4): Resulting value after subtracting cost from benefit.
    - assumptions_and_risks (Step 5): Key assumptions or risks influencing the analysis.

  inputs_required:
    - Defined initiative options or scenarios
    - Financial data or estimation benchmarks
    - Stakeholder inputs on value perception or risk

  outputs_produced:
    - Cost-Benefit Comparison Table
    - ROI Summary or Breakeven Analysis
    - Decision Support Brief

  template_guidance:
    usage: >
      A cost-benefit worksheet helps ensure transparent and standardized evaluation across multiple initiatives.
      It should be formatted to allow side-by-side comparison with commentary.
    required_templates:
      - Cost-Benefit Analysis Table
      - Value Justification Summary
    structure_notes:
      - Columns: Option | Cost Breakdown | Benefit Breakdown | Net Value | ROI % | Assumptions | Pillar | Risk Notes
      - Include charts or heatmaps for visual comparison
    vinci_automation:
      - GPT can autofill estimates from similar cases or benchmarks
      - Assistant may calculate ROI or net value from structured inputs
      - Highlight outliers, inconsistencies, or risk-sensitive cases

  vinci_extensions:
    - Tie cost-benefit outputs directly into initiative prioritization matrix
    - Flag low-ROI initiatives during plan or align phases
    - Link value drivers to Vinci Pillars and stakeholder expectations
    - Reuse past analysis through ontology-linked case libraries

technique:
  id: T010
  name: Cost-Benefit Analysis

  purpose: >
    Cost-Benefit Analysis (CBA) is used to evaluate and compare options or initiatives by quantifying
    their expected costs against their anticipated benefits. It supports evidence-based decision-making
    and prioritization.

  common_use_cases:
    - Comparing solution options during design or alignment
    - Justifying initiative investment or inclusion in a roadmap
    - Identifying initiatives with negative or marginal ROI

  procedural_steps:
    - Step 1: Define the options or scenarios to be evaluated.
    - Step 2: Identify and estimate the costs (one-time and recurring).
    - Step 3: Identify and estimate the benefits (quantitative and qualitative).
    - Step 4: Calculate net value, ROI, or breakeven.
    - Step 5: Present findings with assumptions and sensitivity logic.

  metadata_fields:
    - option_name (Step 1): Name or identifier of the scenario.
    - cost_components (Step 2): Specific cost items (e.g., software, training, staff time).
    - benefit_components (Step 3): Value drivers (e.g., time saved, revenue gained).
    - estimation_method (Step 2–3): Technique used for quantifying inputs (top-down, historical, analogy).
    - net_value (Step 4): Resulting value after subtracting cost from benefit.
    - assumptions_and_risks (Step 5): Key assumptions or risks influencing the analysis.

  inputs_required:
    - Defined initiative options or scenarios
    - Financial data or estimation benchmarks
    - Stakeholder inputs on value perception or risk

  outputs_produced:
    - Cost-Benefit Comparison Table
    - ROI Summary or Breakeven Analysis
    - Decision Support Brief

  template_guidance:
    usage: >
      A cost-benefit worksheet helps ensure transparent and standardized evaluation across multiple initiatives.
      It should be formatted to allow side-by-side comparison with commentary.
    required_templates:
      - Cost-Benefit Analysis Table
      - Value Justification Summary
    structure_notes:
      - Columns: Option | Cost Breakdown | Benefit Breakdown | Net Value | ROI % | Assumptions | Pillar | Risk Notes
      - Include charts or heatmaps for visual comparison
    vinci_automation:
      - GPT can autofill estimates from similar cases or benchmarks
      - Assistant may calculate ROI or net value from structured inputs
      - Highlight outliers, inconsistencies, or risk-sensitive cases

  vinci_extensions:
    - Tie cost-benefit outputs directly into initiative prioritization matrix
    - Flag low-ROI initiatives during plan or align phases
    - Link value drivers to Vinci Pillars and stakeholder expectations
    - Reuse past analysis through ontology-linked case libraries

technique:
  id: T011
  name: Cultural Assessment

  purpose: >
    Cultural Assessment identifies the shared values, beliefs, behaviors, and norms
    that influence how work is done in an organization. It helps diagnose readiness for change,
    sources of resistance, and alignment gaps between culture and strategy.

  common_use_cases:
    - Assessing change readiness during Align or Develop phases
    - Identifying cultural barriers to initiative success
    - Designing leadership or behavior-based interventions

  procedural_steps:
    - Step 1: Define the dimensions or aspects of culture to be assessed (e.g., collaboration, autonomy, hierarchy).
    - Step 2: Gather data through interviews, surveys, or observation.
    - Step 3: Score or classify responses by pattern, sentiment, or theme.
    - Step 4: Identify strengths, risks, and misalignments.
    - Step 5: Package insights for use in strategy, communication, or org design.

  metadata_fields:
    - culture_dimension (Step 1): Category being assessed (e.g., trust, transparency, innovation).
    - observed_behavior (Step 2): Concrete examples of how culture manifests.
    - stakeholder_perception (Step 2): Sentiment or score reported by group or role.
    - friction_hotspot (Step 4): Area where culture blocks value or initiative progress.
    - culture_alignment_score (Step 4): Summary metric for fit between current culture and desired state.

  inputs_required:
    - Stakeholder interviews, focus groups, or surveys
    - Observations from elicitation or discovery sessions
    - Reference to future state behavioral expectations or leadership models

  outputs_produced:
    - Cultural Assessment Report
    - Cultural Friction Map
    - Change Readiness Summary

  template_guidance:
    usage: >
      Cultural assessments should be structured around dimensions that reflect the organization’s values and Vinci’s Pillars.
      Use standard survey or interview templates to ensure comparable and traceable insights.
    required_templates:
      - Culture Assessment Survey or Interview Guide
      - Cultural Alignment Matrix
    structure_notes:
      - Fields: Dimension | Observed Behavior | Stakeholder Group | Score | Risk Flag | Trace Link | Recommendation
      - Use quadrant charts or radar maps to display cultural profiles
    vinci_automation:
      - GPT can draft survey questions or interview formats
      - Analyze interview text for tone, bias, or hidden sentiment
      - Generate visualizations from response clusters

  vinci_extensions:
    - Trace cultural strengths and blockers to Vinci Pillars
    - Use friction areas as input for leadership or change planning
    - Store patterns in the Vinci model to improve future discovery sessions
    - Trigger ongoing cultural monitoring or coaching based on gaps

technique:
  id: T012
  name: Data Analysis

  purpose: >
    Data Analysis involves reviewing quantitative or qualitative data to identify trends,
    anomalies, correlations, or insights that support decision-making, performance tracking, or validation
    of assumptions.

  common_use_cases:
    - Analyzing KPIs, survey results, or benchmarking data
    - Identifying underperformance, process inefficiencies, or traceability gaps
    - Supporting root cause, value assessment, or prioritization efforts

  procedural_steps:
    - Step 1: Define the business question or hypothesis guiding the analysis.
    - Step 2: Collect or consolidate relevant datasets (structured or unstructured).
    - Step 3: Apply analysis methods (statistical, visual, comparative).
    - Step 4: Interpret patterns, outliers, or anomalies.
    - Step 5: Summarize insights and implications for the Vinci delivery flow.

  metadata_fields:
    - analysis_question (Step 1): Core inquiry or problem statement.
    - data_sources (Step 2): Where data originated (e.g., tools, surveys, documents).
    - data_type (Step 2): Structured (numeric, tabular) or unstructured (text, transcripts).
    - analysis_method (Step 3): Chosen technique (e.g., trend, cluster, sentiment).
    - key_insights (Step 4): Findings relevant to decision-making or trace logic.
    - action_recommendations (Step 5): What to do based on results.

  inputs_required:
    - Business problem or decision objective
    - Data sources or access to raw inputs
    - Analysis tool or visualization framework (optional)

  outputs_produced:
    - Data Insights Report
    - Annotated Data Tables or Charts
    - Recommendations Summary

  template_guidance:
    usage: >
      Use structured data analysis templates to guide collection, method selection, and visualization.
      These help ensure repeatability and traceable insights tied to Vinci tasks.
    required_templates:
      - Data Analysis Summary Sheet
      - Charting or Insight Matrix
    structure_notes:
      - Fields: Dataset | Metric | Method | Insight | Action | Pillar | Trace Link
      - Visuals may include bar charts, trend lines, scatter plots, word clouds
    vinci_automation:
      - GPT can identify suitable analysis method from context
      - Generate tabular summaries from CSV or raw data
      - Use prompt-based logic to compare segments or stakeholder groups

  vinci_extensions:
    - Tag insights by Vinci Pillar or stakeholder value lens
    - Use consistent logic across clients via technique template reuse
    - Feed insight clusters into initiative definition or KPI modeling
    - Store results in ontology-linked datasets for benchmarking and reuse

technique:
  id: T013
  name: Data Dictionary

  purpose: >
    A Data Dictionary is a structured reference that defines the meaning, format, source,
    and usage of each data element used within a system, model, or report. It ensures consistent terminology,
    improves traceability, and reduces misinterpretation.

  common_use_cases:
    - Defining metrics or KPIs for scorecards or performance dashboards
    - Clarifying terms used in data models, templates, or requirement sets
    - Supporting traceability from source to output in planning and delivery

  procedural_steps:
    - Step 1: Identify the data fields, indicators, or values that need definition.
    - Step 2: Define each item’s name, description, format, and source.
    - Step 3: Add ownership, update frequency, and business rules (if relevant).
    - Step 4: Validate with data stewards or system/process owners.
    - Step 5: Integrate into traceability models and documentation templates.

  metadata_fields:
    - data_element_name (Step 2): Name of the metric, field, or indicator.
    - definition (Step 2): Description of its meaning and purpose.
    - format (Step 2): Data type (e.g., date, number, currency, boolean).
    - source_system_or_process (Step 2): Where the data is generated or retrieved from.
    - owner_or_steward (Step 3): Who is responsible for its accuracy and updates.
    - update_frequency (Step 3): How often the data is refreshed.
    - business_rules (Step 3): Any conditions, thresholds, or usage logic.
    - pillar_alignment (Optional): If this data element relates to a specific Vinci Pillar.

  inputs_required:
    - List of data fields used in reports, models, or requirements
    - Access to system metadata, owners, or existing definitions

  outputs_produced:
    - Data Dictionary Document or Table
    - Linked Glossary or Concept Definitions
    - Pillar-Aligned Data Map (optional)

  template_guidance:
    usage: >
      Use a data dictionary template anytime numerical data, indicators, or input fields
      will be reused across Vinci tasks or models. Ensures accuracy and alignment with ontology.
    required_templates:
      - Data Dictionary Template (tabular)
    structure_notes:
      - Columns: Name | Description | Format | Source | Owner | Update Frequency | Rules | Trace Link
      - Optional extensions: KPI alignment, quality flags
    vinci_automation:
      - GPT can extract dictionary entries from input files or requirements
      - Suggest standard formats for common fields
      - Auto-link definitions to glossary and concept model

  vinci_extensions:
    - Map key indicators to Vinci Pillars, stakeholder roles, and initiative metrics
    - Use dictionary fields in validation and model generation
    - Surface inconsistencies in term use across phases
    - Version and archive dictionary entries as traceable metadata assets

technique:
  id: T014
  name: Decision Logs

  purpose: >
    A Decision Log captures important decisions made during a project or engagement, along with their rationale,
    participants, and impact. It improves transparency, accountability, and continuity across teams and phases.

  common_use_cases:
    - Documenting scope, requirement, or governance decisions
    - Supporting traceability between analysis and execution
    - Auditing rationale for controversial or risky changes

  procedural_steps:
    - Step 1: Record the decision point and its context (what was being decided and why).
    - Step 2: Document the options considered and the rationale for the selected choice.
    - Step 3: Capture who participated in the decision and their roles.
    - Step 4: Note the implications for requirements, deliverables, or timelines.
    - Step 5: Store the log in a centralized repository and link to trace elements.

  metadata_fields:
    - decision_id (Step 1): Unique identifier for reference and trace.
    - decision_context (Step 1): Description of the business situation or trigger.
    - options_considered (Step 2): Alternatives reviewed before selection.
    - rationale (Step 2): Reasoning behind the final decision.
    - decision_participants (Step 3): Stakeholders or approvers involved.
    - impact_summary (Step 4): What changed or was affected as a result.
    - trace_links (Step 5): Connection to requirements, tasks, or outputs.

  inputs_required:
    - Discussion notes or meeting summaries
    - Decision-making authority structures
    - Documentation of options or alternatives

  outputs_produced:
    - Decision Log Entry
    - Decision Register or Journal
    - Trace Summary for Audit or Handover

  template_guidance:
    usage: >
      Use a structured decision log anytime a key choice affects project scope, delivery, or traceability.
      Each log should follow the same format to allow review, reuse, or audit.
    required_templates:
      - Decision Log Entry Template
      - Decision Register (aggregated view)
    structure_notes:
      - Fields: ID | Date | Topic | Options | Final Choice | Rationale | Participants | Impact | Trace Link
      - Optional: Risk tags, escalation record, follow-up required
    vinci_automation:
      - GPT can autofill logs from meeting transcripts or notes
      - Highlight unlogged decisions for governance review
      - Suggest follow-up questions based on trace impact

  vinci_extensions:
    - Integrate logs with the governance layer of the Vinci ontology
    - Use decision patterns to detect bias or delay trends
    - Feed decisions into stakeholder engagement tracking
    - Store decisions for post-mortem or continuous improvement analysis

technique:
  id: T015
  name: Decision Modelling

  purpose: >
    Decision Modelling provides a structured way to represent, analyze, and explain how choices are made,
    including the factors that influence them, the outcomes they lead to, and the stakeholders involved.

  common_use_cases:
    - Comparing initiatives, solution options, or strategies
    - Structuring stakeholder decision-making logic
    - Clarifying approval flows, authority levels, and trade-offs

  procedural_steps:
    - Step 1: Define the decision to be modeled and its scope.
    - Step 2: Identify the decision criteria and weighting (quantitative or qualitative).
    - Step 3: List the options or scenarios being considered.
    - Step 4: Evaluate each option against each criterion.
    - Step 5: Calculate or visualize outcomes and facilitate stakeholder decision-making.

  metadata_fields:
    - decision_topic (Step 1): Subject or issue being decided.
    - decision_scope (Step 1): Boundaries of influence or impact.
    - criteria_list (Step 2): Key factors that will guide decision evaluation.
    - weighting_scheme (Step 2): Relative importance of each criterion.
    - options_list (Step 3): Viable options, choices, or paths.
    - evaluation_scores (Step 4): How each option performed per criterion.
    - selected_option (Step 5): Final outcome chosen (linked to decision log if applicable).

  inputs_required:
    - Defined decision context or problem
    - Option definitions and trade-off considerations
    - Criteria from stakeholders, strategy, or risk/value models

  outputs_produced:
    - Decision Matrix or Evaluation Table
    - Model Summary Report
    - Selection Justification and Trace Record

  template_guidance:
    usage: >
      A decision model template helps ensure that all important criteria are considered equally
      and that recommendations are based on transparent, stakeholder-aligned logic.
    required_templates:
      - Decision Matrix Template
      - Option Comparison Sheet
    structure_notes:
      - Fields: Option | Criteria | Score | Weight | Total | Risk | Justification | Chosen
      - Include charts or scoring tables for visualization
    vinci_automation:
      - GPT can generate weighted decision matrices from structured prompts
      - Automatically flag unbalanced weighting or missing perspectives
      - Provide justifications based on logic and trace alignment

  vinci_extensions:
    - Store decision models in ontology for reuse and audit
    - Link criteria to Vinci Pillars, roles, or strategic themes
    - Use scoring data to inform prioritization and change strategy
    - Generate model summaries for governance or executive decks

technique:
  id: T016
  name: Document Analysis

  purpose: >
    Document Analysis is the examination of existing documentation—such as policies, reports, process manuals,
    templates, or emails—to extract information relevant to current state, stakeholder needs, rules, risks, or performance.

  common_use_cases:
    - Identifying undocumented requirements or constraints
    - Understanding process logic and trace elements
    - Validating assumptions or legacy system behavior

  procedural_steps:
    - Step 1: Identify relevant documents and artifacts based on project scope.
    - Step 2: Review documents systematically using reading guides or annotation techniques.
    - Step 3: Extract and tag relevant insights or traceable content.
    - Step 4: Summarize findings by theme, risk, opportunity, or Pillar.
    - Step 5: Store annotated outputs and integrate into Vinci models or repositories.

  metadata_fields:
    - document_name (Step 1): Title of the file or artifact being reviewed.
    - source_authority (Step 1): Creator or owner (e.g., system, business unit).
    - key_findings (Step 3): Insights, requirements, decisions, or rules identified.
    - trace_tags (Step 3): Links to Vinci Pillars, capabilities, or stakeholders.
    - insight_category (Step 4): Type (requirement, risk, rule, assumption, process logic).
    - integration_target (Step 5): Where outputs should be fed (e.g., gap analysis, ontology).

  inputs_required:
    - Project-relevant documentation (PDFs, manuals, SOPs, etc.)
    - Traceability or model anchors (capabilities, Pillars, roles)
    - Document access rights (if secure or internal)

  outputs_produced:
    - Annotated Document Pack
    - Findings Summary or Theme Map
    - Integration Log or Model Update Record

  template_guidance:
    usage: >
      Use a structured extraction log or annotation template to ensure consistency when analyzing multiple documents.
      GPT may assist with summarization or categorization.
    required_templates:
      - Document Review Log
      - Insight Extraction Sheet
    structure_notes:
      - Fields: Doc Name | Page/Section | Insight | Pillar | Role | Action | Trace Link
      - Annotations may be inline (PDF) or listed in spreadsheet/table format
    vinci_automation:
      - GPT can parse text or images and suggest insights
      - Auto-tag sentences or sections to Vinci categories
      - Generate summary decks or integration notes for upload

  vinci_extensions:
    - Treat documents as discoverable artifacts in the Vinci ontology
    - Track which docs contributed to which models or trace links
    - Use repeated findings to expose redundancy or cultural patterns
    - Generate audit logs for regulatory or project compliance

technique:
  id: T016
  name: Document Analysis

  purpose: >
    Document Analysis is the examination of existing documentation—such as policies, reports, process manuals,
    templates, or emails—to extract information relevant to current state, stakeholder needs, rules, risks, or performance.

  common_use_cases:
    - Identifying undocumented requirements or constraints
    - Understanding process logic and trace elements
    - Validating assumptions or legacy system behavior

  procedural_steps:
    - Step 1: Identify relevant documents and artifacts based on project scope.
    - Step 2: Review documents systematically using reading guides or annotation techniques.
    - Step 3: Extract and tag relevant insights or traceable content.
    - Step 4: Summarize findings by theme, risk, opportunity, or Pillar.
    - Step 5: Store annotated outputs and integrate into Vinci models or repositories.

  metadata_fields:
    - document_name (Step 1): Title of the file or artifact being reviewed.
    - source_authority (Step 1): Creator or owner (e.g., system, business unit).
    - key_findings (Step 3): Insights, requirements, decisions, or rules identified.
    - trace_tags (Step 3): Links to Vinci Pillars, capabilities, or stakeholders.
    - insight_category (Step 4): Type (requirement, risk, rule, assumption, process logic).
    - integration_target (Step 5): Where outputs should be fed (e.g., gap analysis, ontology).

  inputs_required:
    - Project-relevant documentation (PDFs, manuals, SOPs, etc.)
    - Traceability or model anchors (capabilities, Pillars, roles)
    - Document access rights (if secure or internal)

  outputs_produced:
    - Annotated Document Pack
    - Findings Summary or Theme Map
    - Integration Log or Model Update Record

  template_guidance:
    usage: >
      Use a structured extraction log or annotation template to ensure consistency when analyzing multiple documents.
      GPT may assist with summarization or categorization.
    required_templates:
      - Document Review Log
      - Insight Extraction Sheet
    structure_notes:
      - Fields: Doc Name | Page/Section | Insight | Pillar | Role | Action | Trace Link
      - Annotations may be inline (PDF) or listed in spreadsheet/table format
    vinci_automation:
      - GPT can parse text or images and suggest insights
      - Auto-tag sentences or sections to Vinci categories
      - Generate summary decks or integration notes for upload

  vinci_extensions:
    - Treat documents as discoverable artifacts in the Vinci ontology
    - Track which docs contributed to which models or trace links
    - Use repeated findings to expose redundancy or cultural patterns
    - Generate audit logs for regulatory or project compliance

technique:
  id: T017
  name: Estimation

  purpose: >
    Estimation is used to assess the effort, time, cost, or complexity required to complete
    an initiative, implement a solution, or address a requirement—based on structured methods, expert input,
    or historical data.

  common_use_cases:
    - Estimating initiative size or resource requirements
    - Supporting prioritization or value-based comparisons
    - Preparing inputs for business cases or risk models

  procedural_steps:
    - Step 1: Define what is being estimated and why (scope, output, or performance).
    - Step 2: Select an estimation method (expert judgment, analogy, top-down, bottom-up, Delphi, etc.).
    - Step 3: Gather baseline data, benchmarks, or stakeholder input.
    - Step 4: Perform the estimation using chosen method(s).
    - Step 5: Document assumptions, risks, and estimate ranges.

  metadata_fields:
    - estimation_target (Step 1): The object of estimation (e.g., initiative, task, cost line).
    - estimation_method (Step 2): Method used (Delphi, analogy, historical, parametric).
    - source_data (Step 3): Inputs used in calculation or judgment.
    - estimated_value (Step 4): The output figure (effort, cost, days, €).
    - estimate_range (Step 5): Confidence interval or best/worst-case bounds.
    - assumptions_and_risks (Step 5): Qualifiers or flags affecting reliability.

  inputs_required:
    - Defined scope or object for estimation
    - Historical or benchmark data (if available)
    - Access to experts or estimation models

  outputs_produced:
    - Estimation Summary
    - Cost, Effort, or Duration Tables
    - Risk-Adjusted Estimate (optional)

  template_guidance:
    usage: >
      Estimation templates structure the logic and input behind forecasts,
      especially when multiple stakeholders or methods are involved.
    required_templates:
      - Estimation Worksheet
      - Assumption and Range Log
    structure_notes:
      - Fields: Item | Description | Method | Source | Estimate | Range | Confidence | Risk Notes
      - Can include Gantt-style layout or summary cards
    vinci_automation:
      - GPT can calculate estimates based on analogy or logic rules
      - Auto-compare inputs to historical cases or ranges
      - Generate estimation decks or dashboards with trace notes

  vinci_extensions:
    - Link estimates directly to initiative prioritization logic
    - Compare estimation trends across Vinci clients and sectors
    - Trigger re-estimation when scope or assumptions shift
    - Store estimation rationale as part of governance trace

technique:
  id: T018
  name: Feedback Loops

  purpose: >
    Feedback Loops are structured mechanisms for collecting, interpreting, and responding to stakeholder input—
    in order to refine solutions, improve performance, and ensure engagement remains active and aligned over time.

  common_use_cases:
    - Validating elicited requirements or design proposals
    - Improving stakeholder engagement or collaboration rhythm
    - Iterating initiatives or performance metrics based on real-world input

  procedural_steps:
    - Step 1: Define the feedback source(s), objective, and frequency.
    - Step 2: Choose collection methods (e.g., survey, interviews, feedback forms).
    - Step 3: Analyze input to detect patterns, sentiment, or gaps.
    - Step 4: Close the loop by responding or adjusting based on feedback.
    - Step 5: Document results and update models, plans, or trace logs.

  metadata_fields:
    - feedback_source (Step 1): Role, team, or channel providing feedback.
    - feedback_topic (Step 1): The focus area (e.g., design, process, communication).
    - collection_method (Step 2): How the feedback was obtained.
    - sentiment_or_theme (Step 3): Key takeaways or concern areas.
    - action_response (Step 4): Adjustments made or responses issued.
    - trace_updates (Step 5): Where feedback impacted requirements, plans, or models.

  inputs_required:
    - Stakeholder list or communication plan
    - Previous output or deliverable being reviewed
    - Feedback collection tools or facilitation methods

  outputs_produced:
    - Feedback Log or Summary
    - Adjustment or Resolution Notes
    - Updated Stakeholder Maps or Documents

  template_guidance:
    usage: >
      Use feedback loop templates to track when feedback was requested, how it was handled,
      and whether resolution was achieved. Prevents information loss and improves accountability.
    required_templates:
      - Feedback Log Template
      - Response and Action Summary Sheet
    structure_notes:
      - Fields: Date | Source | Topic | Feedback | Response | Status | Trace Link
      - Visuals optional: Feedback trend lines, engagement heatmaps
    vinci_automation:
      - GPT can summarize freeform text into structured themes
      - Auto-detect gaps between feedback and traceable outputs
      - Trigger follow-up tasks for unresolved concerns

  vinci_extensions:
    - Use feedback loop data in continuous improvement reviews
    - Track engagement trends over time by stakeholder or pillar
    - Highlight friction zones for targeted collaboration redesign
    - Encode loops into governance rhythm for audit and learning

technique:
  id: T019
  name: Financial Analysis

  purpose: >
    Financial Analysis involves evaluating the economic impact, value generation potential, or cost structure
    of a solution, initiative, or organization. It supports investment justification, trade-off decisions,
    and prioritization based on return or affordability.

  common_use_cases:
    - Assessing initiative ROI or total cost of ownership
    - Comparing solution options during business case development
    - Reviewing operational efficiency or investment scenarios

  procedural_steps:
    - Step 1: Define financial dimensions of interest (cost, revenue, margin, ROI, NPV).
    - Step 2: Gather input data (actuals, forecasts, benchmarks).
    - Step 3: Perform calculations or simulations using chosen financial models.
    - Step 4: Analyze results for insights, risks, or trade-offs.
    - Step 5: Package and present financial results in decision-ready formats.

  metadata_fields:
    - financial_focus (Step 1): What is being measured or analyzed.
    - input_data_sources (Step 2): Cost tables, budget sheets, or forecasts used.
    - financial_model_type (Step 3): ROI, NPV, payback, total cost, etc.
    - key_result_metrics (Step 4): Output values like ROI %, breakeven point, margin.
    - sensitivity_assumptions (Step 4): Variables that materially affect outcomes.
    - decision_implications (Step 5): How the analysis affects priorities or recommendations.

  inputs_required:
    - Financial records or forecasts
    - Cost estimates and benefit projections
    - Investment timelines and delivery assumptions

  outputs_produced:
    - Financial Model Results
    - Scenario Comparison Table
    - Investment Recommendation Brief

  template_guidance:
    usage: >
      A structured financial model template helps ensure all necessary components are evaluated,
      that outputs are auditable, and that assumptions are transparent.
    required_templates:
      - Financial Analysis Worksheet
      - Scenario Comparison Table
    structure_notes:
      - Fields: Initiative | CapEx | OpEx | Benefit Estimate | ROI | Payback | Assumptions | Risk Flag
      - Include waterfall charts, dashboards, or cash flow timelines
    vinci_automation:
      - GPT can generate financial models from structured prompts
      - Auto-flag risk factors or assumption sensitivity
      - Pre-populate templates with data from past cases or benchmark libraries

  vinci_extensions:
    - Link financial results to initiative prioritization scoring
    - Highlight underperforming solutions during Plan and Develop phases
    - Encode finance metrics into the ontology for ROI benchmarking
    - Use finance flags to drive re-evaluation of design or strategy options


technique:
  id: T020
  name: Focus Groups

  purpose: >
    A Focus Group is a facilitated discussion with a small group of stakeholders intended to gather diverse insights,
    perceptions, expectations, or reactions about a topic, product, initiative, or issue.

  common_use_cases:
    - Validating requirements, designs, or concepts with multiple perspectives
    - Surfacing qualitative data on pain points, needs, or experiences
    - Exploring reactions to proposed changes, risks, or governance plans

  procedural_steps:
    - Step 1: Define the objective of the session and select the participant group.
    - Step 2: Develop guiding questions and facilitation structure.
    - Step 3: Conduct the session with active listening, prompting, and note-taking.
    - Step 4: Synthesize responses into themes, quotes, and findings.
    - Step 5: Trace responses to roles, tasks, gaps, or design adjustments.

  metadata_fields:
    - session_topic (Step 1): The area being explored or validated.
    - participant_profiles (Step 1): Stakeholders by role, pillar, or interest.
    - key_themes (Step 4): Grouped ideas, concerns, or reactions.
    - supporting_quotes (Step 4): Notable statements to illustrate themes.
    - trace_actions (Step 5): Links between feedback and task, requirement, or strategy.

  inputs_required:
    - Stakeholder map or engagement list
    - Target area for feedback or validation
    - Session plan and facilitation materials

  outputs_produced:
    - Focus Group Summary Report
    - Theme Table or Sentiment Map
    - Updated Requirements or Risk Inputs

  template_guidance:
    usage: >
      Use a focus group summary template to record participants, questions asked, responses given, and
      any derived action items or trace links. Enables reuse, traceability, and insight extraction.
    required_templates:
      - Focus Group Session Guide
      - Feedback Capture Grid
    structure_notes:
      - Fields: Participant | Role | Question | Response | Theme | Action | Trace ID
      - Optional visuals: sentiment heatmaps or engagement scores
    vinci_automation:
      - GPT can summarize transcripts into themes and generate quote tables
      - Suggest follow-up questions based on detected gaps
      - Link feedback directly to ontology nodes or capabilities

  vinci_extensions:
    - Use theme clustering to drive next-step tasks or gap validation
    - Identify sentiment by Pillar or team function
    - Store response history for longitudinal tracking across phases
    - Feed directly into traceability or cultural assessment analysis

technique:
  id: T021
  name: Functional Decomposition

  purpose: >
    Functional Decomposition is a technique used to break down high-level processes, goals, or systems
    into smaller, manageable components. It clarifies scope, identifies dependencies, and simplifies complex initiatives.

  common_use_cases:
    - Breaking down processes into subprocesses or tasks
    - Structuring high-level goals into traceable work units
    - Scoping large initiatives into delivery phases or components

  procedural_steps:
    - Step 1: Define the overall function, process, or capability to be decomposed.
    - Step 2: Identify sub-functions or sub-processes based on logical segmentation.
    - Step 3: Repeat until components are understandable, manageable, and traceable.
    - Step 4: Visualize the hierarchy using trees, diagrams, or nested lists.
    - Step 5: Assign ownership, performance expectations, or trace links to each node.

  metadata_fields:
    - function_name (Step 1): Name or description of the item being decomposed.
    - parent_function (Step 2): Higher-level item the element belongs to.
    - sub_function_list (Step 3): List of subordinate elements.
    - decomposition_level (Step 3): Depth or granularity layer.
    - trace_target (Step 5): What the function links to (role, capability, metric, pillar).

  inputs_required:
    - Process maps, capability models, or strategy goals
    - Stakeholder understanding of end-to-end workflows
    - Ontology anchors or trace points (if available)

  outputs_produced:
    - Functional Decomposition Tree or Diagram
    - Component Trace Register
    - Updated Process or Design Models

  template_guidance:
    usage: >
      Use a functional decomposition template to capture hierarchy consistently,
      ensuring components are traceable and logically segmented.
    required_templates:
      - Decomposition Matrix
      - Function Tree Diagram
    structure_notes:
      - Fields: Function | Parent | Level | Description | Owner | Linked Capability | Pillar
      - Visual formats: tree, nested boxes, layered cards
    vinci_automation:
      - GPT can suggest decompositions based on process input or goals
      - Auto-generate visual structures from flat lists
      - Identify gaps or overlap based on ontology structure

  vinci_extensions:
    - Integrate decomposition maps into traceability matrices
    - Use decomposition to feed initiative scope definition or sequencing
    - Map components to Vinci Pillars for performance alignment
    - Version and compare decompositions over time or across clients

technique:
  id: T022
  name: Gap Analysis

  purpose: >
    Gap Analysis identifies the difference between the current state and the desired future state
    of a capability, process, system, or organizational condition. It reveals what must be changed,
    improved, or built to achieve strategic or operational goals.

  common_use_cases:
    - Comparing current and future capabilities during Discover and Design
    - Structuring initiatives in the Align or Plan phase
    - Identifying missing roles, processes, skills, or systems

  procedural_steps:
    - Step 1: Define the focus area (e.g., capability, role, process).
    - Step 2: Document current state attributes or performance.
    - Step 3: Define desired future state attributes or expectations.
    - Step 4: Compare and measure the gap.
    - Step 5: Translate gap findings into initiative needs or design implications.

  metadata_fields:
    - gap_area (Step 1): What’s being assessed (capability, skill, system, etc.).
    - current_state_profile (Step 2): Description, score, or baseline performance.
    - future_state_target (Step 3): Design expectation or maturity level.
    - gap_magnitude (Step 4): Size or significance of the difference.
    - trace_implication (Step 5): Link to initiative, requirement, or risk.

  inputs_required:
    - Current and future state assessments or designs
    - Stakeholder vision or performance targets
    - Capability maps, role models, or strategic objectives

  outputs_produced:
    - Gap Matrix or Analysis Table
    - Priority Gap List
    - Actionable Initiative Inputs

  template_guidance:
    usage: >
      Use gap analysis templates to ensure side-by-side comparison across scope elements,
      enabling structured prioritization and planning.
    required_templates:
      - Gap Analysis Table
      - Gap Prioritization Sheet
    structure_notes:
      - Fields: Area | Current | Future | Gap | Magnitude | Priority | Trace Link
      - Optional charts: waterfall, gap heatmaps, capability delta
    vinci_automation:
      - GPT can pre-fill current vs future comparisons based on assessments
      - Suggest initiatives based on gaps linked to capabilities or roles
      - Highlight structural or strategic gaps for planning sessions

  vinci_extensions:
    - Link gap elements to Vinci Pillars and trace models
    - Use gap themes to drive initiative definition and roadmap logic
    - Classify gaps by strategic, operational, or compliance relevance
    - Feed high-impact gaps into change strategy or value modeling

technique:
  id: T023
  name: Glossary

  purpose: >
    A Glossary is a structured list of key terms, phrases, and abbreviations used within an organization,
    project, or domain. It ensures consistency, reduces ambiguity, and supports shared understanding
    across stakeholders.

  common_use_cases:
    - Defining domain-specific or organization-specific terminology
    - Supporting requirement clarity and concept modeling
    - Onboarding stakeholders or team members in complex engagements

  procedural_steps:
    - Step 1: Identify and collect terms used in documentation, sessions, or models.
    - Step 2: Define each term in clear, unambiguous language.
    - Step 3: Include abbreviations, synonyms, and usage context if applicable.
    - Step 4: Validate definitions with domain experts or stakeholders.
    - Step 5: Publish the glossary and link it to traceability and concept models.

  metadata_fields:
    - term (Step 1): The word or phrase being defined.
    - definition (Step 2): Meaning or description of the term.
    - usage_context (Step 3): Where or how the term is used in practice.
    - synonyms_or_abbreviations (Step 3): Related terms or shortened forms.
    - approval_status (Step 4): Whether the definition is validated or in draft.
    - ontology_link (Step 5): Relationship to Vinci concepts or trace anchors.

  inputs_required:
    - Project documentation or stakeholder inputs
    - Concept models or data dictionaries (if available)
    - Domain expert review

  outputs_produced:
    - Glossary of Terms
    - Linked Trace and Concept References
    - Communication Support Materials (if needed)

  template_guidance:
    usage: >
      Glossary templates ensure consistent entry formatting, support semantic searches,
      and allow terms to be indexed and reused across tasks and deliverables.
    required_templates:
      - Glossary Table
    structure_notes:
      - Fields: Term | Definition | Synonyms | Abbreviation | Usage Example | Trace Link | Approval
      - Optional: last updated date, owner
    vinci_automation:
      - GPT can extract glossary terms from transcripts or documentation
      - Auto-detect undefined or conflicting terms across documents
      - Propose synonyms or role-specific definitions based on context

  vinci_extensions:
    - Store glossary terms as nodes in the Vinci ontology
    - Link glossary terms to requirements, stakeholder roles, or capabilities
    - Use glossary reviews as part of elicitation and validation processes
    - Track terminology changes across phases for audit or alignment

technique:
  id: T024
  name: Governance Procedures

  purpose: >
    Governance Procedures define the structure, roles, responsibilities, and protocols used to manage decision-making,
    approval, and oversight throughout the lifecycle of a project or change initiative.

  common_use_cases:
    - Defining approval workflows and stakeholder roles
    - Managing change control, traceability, and escalation paths
    - Establishing rules for requirement sign-off and phase transitions

  procedural_steps:
    - Step 1: Define governance scope (e.g., which decisions, deliverables, or phases are governed).
    - Step 2: Identify key governance roles and authorities (e.g., approvers, reviewers, escalation owners).
    - Step 3: Establish protocols for decisions, approvals, and exception handling.
    - Step 4: Document governance procedures and publish them for use.
    - Step 5: Monitor governance activity and update procedures as needed.

  metadata_fields:
    - governed_area (Step 1): The topic or scope under governance (e.g., requirements, design, value).
    - governance_roles (Step 2): Named roles and responsibilities (e.g., BA Lead, Sponsor, Architect).
    - approval_rules (Step 3): Criteria, thresholds, or authority levels needed for decisions.
    - escalation_pathways (Step 3): Who resolves issues or handles exceptions.
    - procedure_version (Step 4): Versioning for audit and change control.
    - compliance_flags (Step 5): Markers for deviations, breaches, or skipped steps.

  inputs_required:
    - Organizational decision-making structures
    - Contractual obligations or stakeholder agreements
    - Approval history or performance review data

  outputs_produced:
    - Governance Framework Document
    - RACI Charts and Approval Maps
    - Governance Activity Logs or Metrics

  template_guidance:
    usage: >
      A governance procedure template ensures that approvals and decision flows are consistent across phases and teams.
      It also supports onboarding and compliance tracking.
    required_templates:
      - Governance Roles and Escalation Table
      - Decision and Approval Flowchart
    structure_notes:
      - Fields: Role | Scope | Authority | Conditions | Backup | Trace Link
      - Visual formats: RACI matrix, decision trees, approval flow diagrams
    vinci_automation:
      - GPT can auto-generate governance flowcharts from stakeholder lists
      - Suggest default rules based on engagement type or project tier
      - Flag governance inconsistencies across models or decisions

  vinci_extensions:
    - Integrate governance roles and approvals into the Vinci ontology
    - Use governance metrics in engagement performance reviews
    - Link governance checkpoints to stakeholder collaboration plans
    - Store procedures in versioned models for reuse and audit

technique:
  id: T025
  name: Impact Mapping

  purpose: >
    Impact Mapping is a visual and strategic planning technique used to clarify the connection between business goals,
    the actors involved, their behaviors, and the solutions that can influence those behaviors to deliver value.

  common_use_cases:
    - Connecting stakeholder actions to initiative objectives
    - Prioritizing features or requirements based on impact
    - Aligning business goals with measurable delivery logic

  procedural_steps:
    - Step 1: Define the central goal or change objective.
    - Step 2: Identify the actors (people, roles, systems) who can influence the goal.
    - Step 3: Determine the behaviors or changes expected from each actor.
    - Step 4: Brainstorm deliverables or actions that will support these behavior changes.
    - Step 5: Visualize the map and use it to align design and prioritization.

  metadata_fields:
    - central_goal (Step 1): Strategic goal the initiative is intended to achieve.
    - actor_list (Step 2): Stakeholders, systems, or groups that influence the goal.
    - desired_behavior (Step 3): Change in behavior required from each actor.
    - supporting_action (Step 4): Solutions or activities to drive behavior change.
    - trace_path (Step 5): Link from deliverable to actor to behavior to goal.

  inputs_required:
    - Business goals or strategic drivers
    - Stakeholder role map or engagement plan
    - Known initiatives, deliverables, or levers of change

  outputs_produced:
    - Impact Map Diagram
    - Traceability Matrix (goal → actor → action)
    - Initiative Alignment Summary

  template_guidance:
    usage: >
      An impact map template visually links actors to behaviors to deliverables to goals.
      It is often used in workshops or strategy design sessions to structure logic and alignment.
    required_templates:
      - Impact Mapping Canvas
      - Trace Link Table
    structure_notes:
      - Fields: Goal | Actor | Behavior | Deliverable | Priority | Trace Link
      - Recommended structure: goal-centered map with spokes to actors and chains of action
    vinci_automation:
      - GPT can generate maps from stakeholder and goal input
      - Auto-suggest missing actors or weak trace paths
      - Generate summary decks or validation grids for alignment sessions

  vinci_extensions:
    - Trace impact paths to Vinci Pillars and initiative structure
    - Use impact weight to drive prioritization scoring
    - Identify actors with too few or too many links (risk zones)
    - Store impact maps in the ontology for reuse, versioning, and comparison

technique:
  id: T026
  name: Information Architecture

  purpose: >
    Information Architecture (IA) is the practice of organizing, labeling, and structuring information
    in a way that supports usability, findability, traceability, and reuse—especially across documents,
    repositories, systems, and stakeholder interfaces.

  common_use_cases:
    - Structuring business analysis documentation and models
    - Designing storage and retrieval structures for BA artifacts
    - Supporting permissioning, access control, and versioning

  procedural_steps:
    - Step 1: Define content types, artifacts, or datasets that need to be organized.
    - Step 2: Identify logical groupings and categories for structure (folders, tags, domains).
    - Step 3: Define metadata and taxonomy rules for labeling and indexing content.
    - Step 4: Apply version control and access control where needed.
    - Step 5: Visualize the architecture and validate it with users or stakeholders.

  metadata_fields:
    - content_type (Step 1): The type of asset (requirement, model, KPI, template, etc.).
    - structure_category (Step 2): Grouping or classification logic (e.g., by phase, pillar, role).
    - taxonomy_terms (Step 3): Labeling terms used to tag and index content.
    - access_level (Step 4): Who can view, edit, or audit the content.
    - version_tag (Step 4): Document or asset version identifier.
    - validation_feedback (Step 5): Notes from stakeholder testing of usability or layout.

  inputs_required:
    - Inventory of existing and expected artifacts
    - User/stakeholder information needs
    - Governance or permissioning rules (optional)

  outputs_produced:
    - Information Architecture Map or Table
    - Metadata and Taxonomy Catalogue
    - Storage and Retrieval Guidance

  template_guidance:
    usage: >
      An IA template allows consistent tagging, versioning, and access structuring
      across all BA artifacts. It ensures everything is traceable and reusable.
    required_templates:
      - IA Structure Table
      - Metadata Schema Template
    structure_notes:
      - Fields: Item | Category | Tag(s) | Owner | Access | Phase | Pillar | Version | Link
      - Diagrams optional: content trees, access matrix, artifact flow
    vinci_automation:
      - GPT can auto-suggest structure based on artifact types
      - Identify gaps in labeling or versioning
      - Flag mismatches between governance plan and information structure

  vinci_extensions:
    - Link IA models to Vinci trace system for continuity and reuse
    - Use IA taxonomy to drive initiative planning and documentation generation
    - Support cross-client reuse through ontology alignment
    - Visualize architecture gaps that may cause delays, confusion, or risk

technique:
  id: T027
  name: Interviews

  purpose: >
    Interviews are structured or semi-structured conversations used to elicit information from stakeholders.
    They enable deep exploration of goals, needs, problems, risks, and opportunities in a flexible, context-aware format.

  common_use_cases:
    - Eliciting requirements, pain points, and success factors
    - Validating assumptions or solution concepts
    - Building stakeholder engagement and empathy

  procedural_steps:
    - Step 1: Define the objective and scope of the interview.
    - Step 2: Select interviewees based on roles, knowledge, or influence.
    - Step 3: Develop guiding questions tailored to context and persona.
    - Step 4: Conduct the interview, capture responses, and observe behaviors.
    - Step 5: Analyze and structure findings into usable outputs.

  metadata_fields:
    - interview_goal (Step 1): The topic or reason for the session.
    - participant_role (Step 2): Function or domain of the interviewee.
    - question_set (Step 3): Structured or semi-structured interview script.
    - key_findings (Step 5): Summarized outputs, pain points, ideas, or quotes.
    - trace_links (Step 5): Where responses influence requirements, risks, or models.

  inputs_required:
    - Stakeholder map or interview plan
    - Background context and objectives
    - Interview logistics and materials

  outputs_produced:
    - Interview Transcript or Summary
    - Interview Insights Table
    - Updated Models, Requirements, or Risk Logs

  template_guidance:
    usage: >
      Interview templates help ensure structure and consistency while still allowing for open-ended exploration.
      They support traceability, reuse, and insight extraction.
    required_templates:
      - Interview Guide
      - Interview Summary Template
    structure_notes:
      - Fields: Date | Interviewee | Role | Questions | Responses | Themes | Trace Notes
      - Can be paper-based, digital, or recorded (transcribed later)
    vinci_automation:
      - GPT can generate question sets from task or goal inputs
      - Summarize transcript into themes, quotes, or risk signals
      - Link findings to trace system or stakeholder models

  vinci_extensions:
    - Categorize interviews by Vinci Pillar or role
    - Identify contradiction or sentiment trends across interviewees
    - Feed results into stakeholder analysis or cultural assessment
    - Store quotes and themes for reuse in validation, workshops, or feedback loops

technique:
  id: T028
  name: Lessons Learned

  purpose: >
    The Lessons Learned technique captures insights, successes, failures, and improvement opportunities
    from completed work to inform future initiatives, reduce risk, and accelerate learning across the organization.

  common_use_cases:
    - Closing a phase or project with reflective evaluation
    - Improving Vinci methods or client engagement practices
    - Informing continuous improvement and reuse frameworks

  procedural_steps:
    - Step 1: Define scope and structure of the review (full project, single phase, role-based, etc.).
    - Step 2: Collect feedback from stakeholders, documents, and performance data.
    - Step 3: Identify key events, successes, failures, decisions, and patterns.
    - Step 4: Structure insights by theme (process, tool, team, governance, etc.).
    - Step 5: Document and share findings, then trigger changes to Vinci methods or assets if needed.

  metadata_fields:
    - review_scope (Step 1): What part of the project or lifecycle is being evaluated.
    - feedback_sources (Step 2): Who/what contributed observations or data.
    - lesson_type (Step 3): Success, issue, idea, risk, or workaround.
    - affected_area (Step 4): Process, role, pillar, deliverable, etc.
    - action_recommendation (Step 5): What should change or be repeated.

  inputs_required:
    - Stakeholder feedback (surveys, interviews, retrospectives)
    - Performance or risk logs
    - Project or initiative documentation

  outputs_produced:
    - Lessons Learned Log
    - Recommendations Table
    - Vinci Improvement Update or Flag

  template_guidance:
    usage: >
      Use a structured lessons learned template to capture insights consistently,
      especially when multiple contributors or long engagements are involved.
    required_templates:
      - Lessons Learned Capture Sheet
      - Recommendations Tracker
    structure_notes:
      - Fields: Topic | Lesson Type | Description | Impact | Recommendation | Owner | Trace Tag
      - Optional dashboards: thematic heatmaps, timeline-based trends
    vinci_automation:
      - GPT can extract lessons from retrospective transcripts or interviews
      - Suggest recommendations based on pattern recognition
      - Flag lessons for method updates, retraining, or template revision

  vinci_extensions:
    - Feed lessons into Vinci’s continuous improvement loop
    - Store themes and patterns in ontology for trend comparison
    - Use key lessons to generate warnings in similar future projects
    - Track lesson resolution or implementation status over time

technique:
  id: T029
  name: Metrics and KPIs

  purpose: >
    Metrics and Key Performance Indicators (KPIs) are quantifiable measures used to track performance,
    progress, outcomes, or alignment to strategic goals. They support decision-making, continuous improvement,
    and initiative validation.

  common_use_cases:
    - Defining success criteria for initiatives or capabilities
    - Monitoring performance against baselines or forecasts
    - Linking objectives to measurable outcomes in scorecards or dashboards

  procedural_steps:
    - Step 1: Define what needs to be measured and why.
    - Step 2: Identify appropriate metrics or KPIs aligned to that purpose.
    - Step 3: Specify targets, thresholds, and update frequencies.
    - Step 4: Document data sources, owners, and visualization preferences.
    - Step 5: Track and review performance periodically; refine as needed.

  metadata_fields:
    - metric_name (Step 2): The name of the KPI or metric.
    - kpi_purpose (Step 1): Strategic or operational reason for tracking.
    - unit_of_measure (Step 2): What the metric is measured in (e.g., %, days, €).
    - target_value (Step 3): Expected or desired level of performance.
    - update_frequency (Step 3): How often the metric is reviewed or refreshed.
    - data_source (Step 4): Where the data comes from.
    - owner (Step 4): Role responsible for monitoring or reporting.

  inputs_required:
    - Strategic goals or success criteria
    - Performance baselines or forecasts
    - Capability or initiative structure

  outputs_produced:
    - KPI Catalogue
    - Measurement Dashboard
    - Performance Reports or Scorecards

  template_guidance:
    usage: >
      Use KPI templates to maintain consistent definition, calculation logic, and traceability
      across all metrics—especially those used in alignment, planning, and develop phases.
    required_templates:
      - KPI Definition Table
      - Performance Dashboard Layout
    structure_notes:
      - Fields: Name | Purpose | Unit | Target | Actual | Frequency | Owner | Status | Trace Link
      - Visuals: Gauges, traffic lights, spark lines, bar graphs
    vinci_automation:
      - GPT can recommend KPIs by role, phase, or objective
      - Auto-flag stale, missing, or off-target KPIs
      - Suggest calculation logic or proxy metrics if needed

  vinci_extensions:
    - Encode KPIs in the Vinci ontology for traceability and performance analysis
    - Link metrics directly to initiatives, capability maps, or Pillars
    - Visualize trends across phases or clients
    - Trigger alerts, re-analysis, or improvement suggestions based on thresholds

technique:
  id: T030
  name: Mind Mapping

  purpose: >
    Mind Mapping is a visual technique used to organize thoughts, ideas, requirements, or problems
    around a central concept—making it easier to see connections, groupings, and emerging themes.

  common_use_cases:
    - Structuring early discovery findings or stakeholder input
    - Brainstorming and clustering solution ideas
    - Synthesizing complex information into a visual layout

  procedural_steps:
    - Step 1: Define the central theme, topic, or challenge.
    - Step 2: Branch out with related ideas, terms, categories, or problems.
    - Step 3: Expand secondary branches with examples, insights, or linked items.
    - Step 4: Group and color-code related elements for clarity.
    - Step 5: Convert branches into traceable categories, themes, or models.

  metadata_fields:
    - map_topic (Step 1): The central focus of the mind map.
    - branch_label (Step 2): Key idea or category extending from the center.
    - sub_branch (Step 3): Specifics or nested items beneath each category.
    - trace_tags (Step 5): Connections to Pillars, roles, or requirements.
    - visual_grouping (Step 4): Color or formatting used to represent clusters.

  inputs_required:
    - Raw ideas, notes, quotes, or unstructured feedback
    - Theme, domain, or problem focus
    - Optional tools (digital or physical whiteboard)

  outputs_produced:
    - Mind Map (digital or drawn)
    - Theme Clustering Table
    - Input Summary or Synthesis Report

  template_guidance:
    usage: >
      Use mind map templates to quickly collect, sort, and visually group ideas.
      It is especially useful in workshops, discovery, or post-interview synthesis.
    required_templates:
      - Mind Map Template (editable)
      - Idea Clustering Summary Sheet
    structure_notes:
      - Nodes: Topic | Branch | Sub-branch | Theme | Trace Link
      - Digital tools: Miro, XMind, Lucidchart, Draw.io (or GPT table output)
    vinci_automation:
      - GPT can generate mind maps from lists or bullet inputs
      - Auto-group items by Pillar, domain, or topic similarity
      - Convert mind map nodes into traceable requirements or risks

  vinci_extensions:
    - Link map clusters to Vinci Pillars, design inputs, or stakeholder roles
    - Use maps to detect complexity, blind spots, or duplication
    - Feed categorized nodes into initiative scoping or prioritization
    - Store maps in the ontology for reuse and reflection

technique:
  id: T031
  name: Observation

  purpose: >
    Observation involves watching users, teams, or processes in action to gather real-world data,
    uncover hidden behaviors, validate assumptions, and detect gaps that may not surface in interviews or documentation.

  common_use_cases:
    - Discovering workarounds or behavioral patterns in process execution
    - Validating documented procedures against actual practice
    - Uncovering tacit knowledge, friction points, or cultural dynamics

  procedural_steps:
    - Step 1: Define the observation goal and scope (task, role, or process).
    - Step 2: Select observation method (active participation or passive viewing).
    - Step 3: Prepare observation guide with prompts and focus areas.
    - Step 4: Conduct observation and capture notes or recordings.
    - Step 5: Analyze findings and translate into requirements, risks, or models.

  metadata_fields:
    - observation_target (Step 1): What activity, role, or task is being observed.
    - observation_type (Step 2): Passive, active, shadowing, or real-time.
    - behavior_patterns (Step 4): Repeated or notable actions by participants.
    - deviation_notes (Step 4): Gaps between expected and actual practice.
    - derived_findings (Step 5): Insights categorized as requirements, pain points, or risks.
    - trace_links (Step 5): Where the insight links to models, templates, or initiatives.

  inputs_required:
    - Access to teams, systems, or environments
    - Observation plan or research guide
    - Permission or briefing for observed participants

  outputs_produced:
    - Observation Notes or Logs
    - Behavior Pattern Summary
    - Model or Requirement Updates

  template_guidance:
    usage: >
      Use an observation template to capture observations consistently and ethically.
      Include timestamps, observer comments, and areas of focus aligned to Vinci Pillars or objectives.
    required_templates:
      - Observation Log Template
      - Behavior Pattern Summary Table
    structure_notes:
      - Fields: Time | Task | Actor | Behavior | Notes | Deviation | Risk | Trace Link
      - Optional visuals: swimlanes, screenshots, behavior heatmaps
    vinci_automation:
      - GPT can structure raw notes into tables and insights
      - Detect contradictions between observation and documentation
      - Suggest behavioral themes or risks based on findings

  vinci_extensions:
    - Tag behaviors by Vinci Pillars (e.g., Skills, Structure, Roles)
    - Use observation findings to challenge assumptions or refine elicitation plans
    - Feed deviation logs into governance or audit reports
    - Store anonymized logs for learning and reuse

technique:
  id: T032
  name: Organizational Modelling

  purpose: >
    Organizational Modelling is used to define and visualize the structure, roles, responsibilities,
    and reporting relationships within a business unit or enterprise. It supports role clarity, decision-making authority,
    communication flows, and strategic alignment.

  common_use_cases:
    - Defining or validating current or future organizational structures
    - Mapping roles to processes, systems, and capabilities
    - Identifying gaps, overlaps, or decision bottlenecks

  procedural_steps:
    - Step 1: Define the modeling scope (e.g., function, division, or enterprise-wide).
    - Step 2: Identify existing and proposed roles, reporting lines, and key interfaces.
    - Step 3: Map responsibilities, span of control, and role interactions.
    - Step 4: Visualize the structure and tag roles by purpose or pillar.
    - Step 5: Validate model with leadership and encode into trace models.

  metadata_fields:
    - model_scope (Step 1): What part of the organization is being modeled.
    - role_catalog (Step 2): List of functional roles or positions.
    - reporting_structure (Step 2): Hierarchical or matrix-based relationships.
    - role_responsibilities (Step 3): Key functions or deliverables per role.
    - interaction_map (Step 3): How roles collaborate or depend on one another.
    - validation_status (Step 5): Whether model is draft, proposed, or approved.

  inputs_required:
    - Org charts, process maps, or RACI models
    - Role descriptions or stakeholder feedback
    - Design requirements or target operating models

  outputs_produced:
    - Organizational Structure Diagram
    - Role Interaction Matrix
    - Role-to-Process or Role-to-Capability Map

  template_guidance:
    usage: >
      Use an org modeling template to align structures with Vinci Pillars, stakeholder maps, and capability models.
      It is essential in Discover, Design, and Align phases.
    required_templates:
      - Organizational Model Worksheet
      - Role Relationship Map
    structure_notes:
      - Fields: Role | Reports To | Responsibilities | Linked Processes | Pillar | Status | Owner
      - Visuals: hierarchy trees, swimlanes, cluster maps
    vinci_automation:
      - GPT can generate draft models from lists or descriptions
      - Suggest role overlaps, gaps, or bottlenecks
      - Auto-tag roles by Pillar or capability linkage

  vinci_extensions:
    - Encode role models into Vinci ontology for traceability and reuse
    - Use organizational models to structure collaboration, communication, and handoffs
    - Compare current vs future role sets for change planning
    - Feed roles into initiative planning and stakeholder engagement strategy

technique:
  id: T033
  name: Policy Review

  purpose: >
    Policy Review is the structured examination of rules, regulations, standards, or procedures
    that govern behavior, decision-making, compliance, or accountability in an organization.
    It ensures alignment, feasibility, and risk awareness during analysis or design.

  common_use_cases:
    - Identifying organizational constraints or governance requirements
    - Validating alignment between initiatives and policy frameworks
    - Surfacing policy-related risks, blockers, or conflicts

  procedural_steps:
    - Step 1: Identify relevant policies based on initiative scope or domain.
    - Step 2: Review content to extract requirements, constraints, or escalation rules.
    - Step 3: Classify policies by impact (e.g., design, operations, risk, governance).
    - Step 4: Document conflicts, misalignments, or policy gaps.
    - Step 5: Link findings to decisions, requirements, risks, or governance plans.

  metadata_fields:
    - policy_name (Step 1): Title or reference ID of the document.
    - governing_body (Step 1): Role, function, or external regulator responsible.
    - policy_requirement (Step 2): Specific rule, clause, or instruction extracted.
    - impacted_area (Step 3): What part of the initiative or business it affects.
    - risk_flag (Step 4): Whether the policy introduces constraint or threat.
    - trace_target (Step 5): What task, requirement, or decision is influenced.

  inputs_required:
    - Policy documents (internal or external)
    - Initiative scope, compliance areas, or design elements
    - Regulatory or contractual references (if applicable)

  outputs_produced:
    - Policy Impact Summary
    - Constraint and Risk Register
    - Compliance Alignment Record

  template_guidance:
    usage: >
      Policy review templates are used to extract, track, and interpret requirements from official documents
      to ensure compliance and traceability during initiative delivery.
    required_templates:
      - Policy Review Worksheet
      - Policy-Impact Mapping Table
    structure_notes:
      - Fields: Policy | Clause | Summary | Affected Area | Risk | Trace Link | Notes
      - Optional visuals: policy maps, compliance heatmaps
    vinci_automation:
      - GPT can extract clauses and summarize policy sections
      - Tag policies by Vinci Pillar, process, or role
      - Highlight conflicts or redundancies across policies

  vinci_extensions:
    - Map policy constraints to Vinci Pillars and decision logic
    - Use reviews to flag change requests, redesign needs, or escalation
    - Store policy links in ontology for reuse or audit
    - Track resolution or adaptation of flagged risks across phases

technique:
  id: T034
  name: Power/Interest Grids

  purpose: >
    Power/Interest Grids are visual stakeholder analysis tools used to categorize stakeholders
    based on their level of influence (power) and their level of concern or involvement (interest)
    in a given initiative. They guide engagement planning, communication, and governance strategy.

  common_use_cases:
    - Prioritizing stakeholder communication and involvement
    - Identifying hidden influencers, blockers, or passive risks
    - Structuring governance models or escalation paths

  procedural_steps:
    - Step 1: Identify all stakeholders linked to the initiative or domain.
    - Step 2: Assess each stakeholder’s level of power and interest.
    - Step 3: Plot stakeholders on a 2x2 grid (High/Low Power × High/Low Interest).
    - Step 4: Define engagement strategies for each quadrant.
    - Step 5: Review and adjust grid as relationships evolve or new actors emerge.

  metadata_fields:
    - stakeholder_name (Step 1): Person, role, or group evaluated.
    - power_rating (Step 2): Influence over decisions or outcomes.
    - interest_rating (Step 2): Degree of concern, involvement, or investment.
    - engagement_quadrant (Step 3): Grid position (e.g., Manage Closely, Keep Informed).
    - recommended_strategy (Step 4): Tailored communication or collaboration approach.

  inputs_required:
    - Stakeholder list or role map
    - Organizational hierarchy or governance documents
    - Stakeholder history or prior behavior (optional)

  outputs_produced:
    - Power/Interest Grid Visualization
    - Stakeholder Engagement Strategy
    - Communication and Escalation Plan

  template_guidance:
    usage: >
      Use this grid template to classify stakeholders and adapt communication and governance accordingly.
      Particularly useful in Engage, Align, and Plan phases.
    required_templates:
      - Power/Interest Matrix
      - Stakeholder Strategy Table
    structure_notes:
      - Fields: Name | Role | Power | Interest | Quadrant | Strategy | Notes
      - Visual: 2x2 matrix with quadrant labels and plotted stakeholders
    vinci_automation:
      - GPT can recommend strategies based on quadrant
      - Auto-tag stakeholders with communication formats and cadence
      - Flag power-interest conflicts or risks

  vinci_extensions:
    - Link grid outputs to stakeholder collaboration and governance plans
    - Feed into RACI matrices and role-based traceability models
    - Use quadrant shifts as triggers for re-engagement or risk review
    - Store stakeholder grids in the Vinci model for future engagements

technique:
  id: T035
  name: Prioritization

  purpose: >
    Prioritization is the process of ranking requirements, initiatives, issues, or opportunities
    based on agreed criteria such as value, risk, urgency, feasibility, or stakeholder preference.
    It supports sequencing and resource allocation decisions.

  common_use_cases:
    - Ordering initiatives or requirements during planning
    - Facilitating stakeholder trade-offs and scope decisions
    - Supporting backlog grooming or delivery roadmaps

  procedural_steps:
    - Step 1: Define the items to be prioritized and their scope.
    - Step 2: Choose prioritization method(s) (e.g., MoSCoW, Weighted Scoring, 100-Point, Forced Ranking).
    - Step 3: Define evaluation criteria and scoring scale (if applicable).
    - Step 4: Score or rank each item with stakeholder involvement.
    - Step 5: Review results, resolve conflicts, and finalize list.

  metadata_fields:
    - item_id (Step 1): Requirement, initiative, or concept being ranked.
    - prioritization_method (Step 2): Method used to sort or score items.
    - criteria_set (Step 3): The dimensions used (e.g., value, cost, urgency).
    - score_value (Step 4): The outcome of scoring or voting.
    - final_rank (Step 5): Resolved position in the overall list.
    - stakeholder_feedback (Step 5): Concerns, objections, or rationale captured during the process.

  inputs_required:
    - List of candidate items
    - Criteria and weighting logic (if applicable)
    - Stakeholder input and review

  outputs_produced:
    - Prioritization Table
    - Ranked Item List
    - Decision Rationale and Conflict Log

  template_guidance:
    usage: >
      Use prioritization templates to ensure transparency and comparability in scoring and ranking.
      Support repeatable decisions and stakeholder alignment across sessions or phases.
    required_templates:
      - Prioritization Scorecard
      - Ranking Grid or MoSCoW Table
    structure_notes:
      - Fields: Item | Criteria | Score(s) | Weight | Total | Rank | Status | Notes
      - Visuals: heatmaps, scatter plots, scoring bars
    vinci_automation:
      - GPT can auto-calculate rankings from weighted input
      - Highlight scoring inconsistencies across roles or sessions
      - Suggest follow-up when priorities conflict with strategic goals

  vinci_extensions:
    - Link priorities to trace models and initiative scope
    - Drive roadmap generation and capacity planning logic
    - Track changes in priority over time for retrospective learning
    - Use priority clusters to shape bundle proposals or MVP definitions

technique:
  id: T036
  name: Process Modelling

  purpose: >
    Process Modelling visually represents how work flows through an organization,
    showing steps, roles, systems, decisions, and outcomes. It improves clarity, enables analysis,
    and supports design, improvement, and traceability efforts.

  common_use_cases:
    - Documenting current and future state processes
    - Identifying inefficiencies, risks, or improvement opportunities
    - Supporting requirements elicitation and role alignment

  procedural_steps:
    - Step 1: Define process boundaries and scope.
    - Step 2: Identify steps, decision points, actors, and systems involved.
    - Step 3: Choose modeling notation (e.g., BPMN, swimlane, SIPOC, flowchart).
    - Step 4: Create and validate the model with stakeholders.
    - Step 5: Use model for analysis, trace linkage, and solution design.

  metadata_fields:
    - process_name (Step 1): Name or ID of the process being modeled.
    - process_scope (Step 1): Inputs, outputs, start, and end points.
    - actor_roles (Step 2): Who performs each step (linked to org model).
    - decision_points (Step 2): Conditional logic or choice moments.
    - system_interactions (Step 2): Tech elements that support or automate steps.
    - validation_status (Step 4): Stakeholder agreement and model maturity.
    - traceability_links (Step 5): Connection to requirements, gaps, or capabilities.

  inputs_required:
    - Process documentation, stakeholder input, or system outputs
    - Org charts or role models
    - Tools for visualization

  outputs_produced:
    - Process Flow Diagram or Map
    - Role and System Interaction Matrix
    - Process-Gap or Traceability Model

  template_guidance:
    usage: >
      Use process model templates to consistently capture task logic, actor involvement,
      and decision structures. Essential in discovery, design, and solution definition.
    required_templates:
      - Process Modelling Canvas (BPMN, flowchart, or swimlane)
      - Process Data Table (for traceable fields)
    structure_notes:
      - Fields: Step | Actor | Input | Output | System | Decision? | Notes | Trace ID
      - Visuals: swimlanes, value stream maps, SIPOC
    vinci_automation:
      - GPT can generate draft models from structured descriptions or role maps
      - Detect missing steps, circular logic, or handoff issues
      - Auto-map steps to Vinci Pillars and trace anchors

  vinci_extensions:
    - Link processes to role, capability, and system models
    - Track process variation across business units or phases
    - Use process gaps to define initiatives or change strategies
    - Store validated process maps in ontology for reuse and alignment

technique:
  id: T037
  name: Prototyping

  purpose: >
    Prototyping creates early representations of a solution—visual, functional, or interactive—
    to test concepts, gather feedback, reduce risk, and refine design before development or investment.

  common_use_cases:
    - Exploring and validating user interfaces, workflows, or report structures
    - Engaging stakeholders through visualization and simulation
    - Refining requirements and improving shared understanding

  procedural_steps:
    - Step 1: Define the goal and scope of the prototype (e.g., feature, interface, workflow).
    - Step 2: Choose fidelity level (low-fi wireframe to high-fi interactive).
    - Step 3: Design and build the prototype using appropriate tools.
    - Step 4: Test with users or stakeholders and capture feedback.
    - Step 5: Refine design and document traceable requirements or insights.

  metadata_fields:
    - prototype_scope (Step 1): What part of the solution is being prototyped.
    - fidelity_level (Step 2): Low, medium, or high fidelity.
    - design_elements (Step 3): Key features or screens represented.
    - test_feedback (Step 4): Insights from review or walkthrough.
    - trace_updates (Step 5): Requirements or decisions adjusted as a result.

  inputs_required:
    - Initial design or requirement concepts
    - Tools for visualization (e.g., Figma, Balsamiq, PowerPoint)
    - Stakeholder profiles and usage scenarios

  outputs_produced:
    - Prototype file or storyboard
    - Feedback summary or decision log
    - Updated requirements or trace records

  template_guidance:
    usage: >
      Prototyping templates help structure user flows, screen content, and interaction logic
      for feedback and reuse across stakeholders and delivery stages.
    required_templates:
      - Prototype Canvas or Frame Sheet
      - Feedback and Adjustment Tracker
    structure_notes:
      - Fields: Element | Function | Status | Feedback | Change Needed | Trace Link
      - Visuals: screen mockups, user journey flows, wireframes
    vinci_automation:
      - GPT can sketch low-fidelity wireframes from user stories
      - Auto-link feedback to requirement models or decisions
      - Track user sentiment and friction by feature or persona

  vinci_extensions:
    - Connect prototypes to design models, KPIs, or stakeholder maps
    - Use rapid iterations to reduce rework and clarify assumptions
    - Feed final prototype elements into capability and system blueprints
    - Store reviewed prototypes in the ontology as visual trace assets

technique:
  id: T038
  name: RACI Matrix

  purpose: >
    A RACI Matrix is a responsibility assignment tool that clarifies who is Responsible, Accountable,
    Consulted, and Informed for key activities, decisions, or deliverables in a project or business process.

  common_use_cases:
    - Clarifying stakeholder responsibilities during planning or delivery
    - Defining governance roles across tasks, phases, or initiatives
    - Preventing confusion or gaps in accountability

  procedural_steps:
    - Step 1: Identify key tasks, deliverables, or decisions requiring role clarity.
    - Step 2: List all stakeholders, roles, or functions involved.
    - Step 3: Assign R, A, C, or I to each stakeholder per activity.
    - Step 4: Review and resolve any conflicts, overloads, or gaps.
    - Step 5: Publish matrix and integrate into plans, communications, or governance flows.

  metadata_fields:
    - task_or_deliverable (Step 1): The item requiring accountability.
    - stakeholder_role (Step 2): Function or team represented.
    - raci_assignment (Step 3): Role per task (R, A, C, I).
    - overlap_flags (Step 4): Multiple R or A conflicts identified.
    - trace_link (Step 5): Where this matrix supports a model, phase, or template.

  inputs_required:
    - Task list or work breakdown structure
    - Stakeholder or role map
    - Governance or team expectations

  outputs_produced:
    - RACI Responsibility Matrix
    - Role Clarification Summary
    - Conflict or Gap Report (optional)

  template_guidance:
    usage: >
      Use a standard RACI template to visualize responsibilities and avoid ambiguity.
      This technique is essential in governance, stakeholder alignment, and phase transitions.
    required_templates:
      - RACI Assignment Table
      - Responsibility Coverage Map (optional)
    structure_notes:
      - Fields: Task | Role 1 | Role 2 | … | Notes
      - Values: R, A, C, I per cell
    vinci_automation:
      - GPT can generate RACI grids from stakeholder and task inputs
      - Flag overloaded roles or missing assignments
      - Convert matrices into stakeholder engagement summaries

  vinci_extensions:
    - Encode RACI links into Vinci’s stakeholder and role models
    - Use matrices to drive governance structure and communication planning
    - Compare across clients to refine templates or engagement design
    - Trigger alerts when tasks lack accountability or consensus

technique:
  id: T039
  name: Requirements Mapping

  purpose: >
    Requirements Mapping is the process of establishing and visualizing logical relationships
    between different types of requirements, their sources, and their impacts. It enables traceability,
    alignment, and gap detection across stakeholders, systems, and models.

  common_use_cases:
    - Linking stakeholder needs to functional, non-functional, or transitional requirements
    - Mapping requirements to business rules, processes, or capabilities
    - Supporting traceability, validation, and impact analysis

  procedural_steps:
    - Step 1: Catalog requirements by type, source, or domain.
    - Step 2: Define logical relationships (e.g., derives from, supports, enables, constrains).
    - Step 3: Map requirements to systems, processes, roles, or goals.
    - Step 4: Visualize or model the relationships in traceable form.
    - Step 5: Validate and maintain maps across phases or decisions.

  metadata_fields:
    - requirement_id (Step 1): Unique reference or label.
    - requirement_type (Step 1): Stakeholder, business, functional, non-functional, transitional.
    - relationship_type (Step 2): Nature of the link (e.g., supports, depends on).
    - source_reference (Step 1): Originating stakeholder, document, or goal.
    - target_element (Step 3): What the requirement is mapped to (e.g., system, capability).
    - trace_tag (Step 4): Linkage code for cross-reference in models or tools.

  inputs_required:
    - Validated requirement sets
    - System or process models
    - Stakeholder or goal documents

  outputs_produced:
    - Requirements Relationship Matrix
    - Visual Map or Diagram
    - Traceability Record

  template_guidance:
    usage: >
      Requirements mapping templates ensure structured connections between different levels
      or types of requirements, supporting analysis and audit throughout the lifecycle.
    required_templates:
      - Requirements Relationship Table
      - Mapping Diagram Template
    structure_notes:
      - Fields: Req ID | Type | Source | Target | Relation | Notes | Trace Link
      - Visuals: node-link diagrams, swimlanes, layered tables
    vinci_automation:
      - GPT can detect and suggest requirement links based on descriptions
      - Auto-tag types and sources using glossary or ontology
      - Generate exportable maps or documentation-ready tables

  vinci_extensions:
    - Store maps in the Vinci trace system for reuse, audit, or visualization
    - Use mappings to drive prioritization, impact analysis, and initiative scoping
    - Highlight unmapped or conflicting requirements during validation
    - Enable backward and forward trace through entire delivery lifecycle

technique:
  id: T040
  name: Retrospectives

  purpose: >
    Retrospectives are structured review sessions held at the end of a project phase, milestone,
    or iteration to reflect on what went well, what didn’t, and how the team or process can improve.
    They focus on learning, team alignment, and continuous improvement.

  common_use_cases:
    - Reviewing team performance or delivery methods after a phase
    - Identifying improvement areas for tools, communication, or processes
    - Enhancing collaboration and delivery quality across Vinci engagements

  procedural_steps:
    - Step 1: Schedule a safe, inclusive review session with relevant participants.
    - Step 2: Collect feedback using a structured framework (e.g., Start-Stop-Continue, 4Ls, Mad-Sad-Glad).
    - Step 3: Group themes and identify patterns or high-impact issues.
    - Step 4: Prioritize improvement ideas and assign ownership.
    - Step 5: Document outcomes and track follow-up actions across cycles.

  metadata_fields:
    - session_scope (Step 1): Project, team, phase, or milestone reviewed.
    - feedback_framework (Step 2): Structure used to collect input.
    - theme_clusters (Step 3): Grouped observations or trends.
    - improvement_actions (Step 4): Recommended changes or new practices.
    - owner_and_due_date (Step 4): Person responsible and target timeline.
    - implementation_status (Step 5): Progress on applying agreed actions.

  inputs_required:
    - Participation from delivery team and stakeholders
    - Performance or delivery records (optional)
    - Collaboration and governance logs

  outputs_produced:
    - Retrospective Summary Report
    - Action Plan or Improvement Log
    - Lessons Learned Tracker (optional link)

  template_guidance:
    usage: >
      Use retrospective templates to ensure structured, repeatable reviews and measurable improvements.
      Combine with Lessons Learned for broader continuous improvement.
    required_templates:
      - Retrospective Feedback Grid
      - Action Tracking Sheet
    structure_notes:
      - Fields: Topic | Comment | Category | Action | Owner | Status | Pillar Link
      - Visuals: radar charts, theme clouds, feedback bars
    vinci_automation:
      - GPT can group and synthesize feedback into themes
      - Track recurring issues across phases or teams
      - Generate improvement plans or follow-up nudges

  vinci_extensions:
    - Feed retrospective themes into Vinci’s improvement registry
    - Link outcomes to future planning, risk prevention, or stakeholder engagement
    - Measure recurring pain points across clients or delivery teams
    - Track improvement metrics over time in the Develop phase

technique:
  id: T041
  name: Reviews

  purpose: >
    Reviews are formal or informal evaluations of deliverables, decisions, designs, or requirements
    to ensure quality, accuracy, alignment, and readiness for progression. They support stakeholder validation,
    risk reduction, and traceability.

  common_use_cases:
    - Validating requirements before sign-off
    - Reviewing documents or models for completeness or compliance
    - Gaining stakeholder alignment on proposals or strategies

  procedural_steps:
    - Step 1: Define the review scope, objective, and participants.
    - Step 2: Prepare materials and distribute them ahead of time.
    - Step 3: Facilitate the review session or collect asynchronous input.
    - Step 4: Capture feedback, identify issues, and document decisions.
    - Step 5: Resolve comments and finalize the reviewed artifact.

  metadata_fields:
    - review_scope (Step 1): Deliverables or decisions being evaluated.
    - reviewer_roles (Step 1): Stakeholders or functions involved.
    - review_comments (Step 4): Feedback grouped by issue type or severity.
    - decisions_or_actions (Step 4): Agreed updates, rejections, or escalations.
    - resolution_status (Step 5): Open, resolved, deferred, or approved.
    - trace_tag (Step 5): Link to requirement, risk, or phase transition.

  inputs_required:
    - Artifact(s) for review (document, model, matrix, etc.)
    - Reviewer list and review plan
    - Governance or validation criteria (optional)

  outputs_produced:
    - Review Summary or Comment Log
    - Decision or Approval Record
    - Updated Deliverable or Change Log

  template_guidance:
    usage: >
      Review templates structure feedback collection, action resolution,
      and traceability of validation across stakeholders and decisions.
    required_templates:
      - Review Checklist or Comment Table
      - Review Action Tracker
    structure_notes:
      - Fields: Item | Comment | Type | Severity | Owner | Resolution | Date | Trace Link
      - Optional: review scorecards, alignment dashboards
    vinci_automation:
      - GPT can extract and organize review feedback into categories
      - Suggest resolution workflows or decision paths
      - Auto-tag trace links or unresolved items

  vinci_extensions:
    - Link review cycles to Vinci’s governance and traceability framework
    - Track issue density or patterns across project phases
    - Use unresolved items to trigger escalations or risk reviews
    - Version and archive review records for audit and reuse

technique:
  id: T042
  name: Risk Analysis

  purpose: >
    Risk Analysis identifies, evaluates, and prioritizes potential threats or uncertainties
    that could affect the success, quality, or value of a project, initiative, or decision.

  common_use_cases:
    - Assessing risks during planning or solution alignment
    - Preparing risk response strategies and contingency plans
    - Supporting prioritization, governance, or investment logic

  procedural_steps:
    - Step 1: Identify risks using structured techniques (e.g., brainstorming, interviews, checklists).
    - Step 2: Categorize risks (e.g., operational, strategic, technical, stakeholder).
    - Step 3: Assess likelihood and impact using scoring scales.
    - Step 4: Prioritize risks based on composite risk score.
    - Step 5: Recommend mitigation, monitoring, or escalation actions.

  metadata_fields:
    - risk_id (Step 1): Unique reference or label.
    - risk_category (Step 2): Domain of exposure (e.g., financial, cultural, regulatory).
    - likelihood_score (Step 3): Probability of occurrence (e.g., 1–5).
    - impact_score (Step 3): Magnitude of potential consequence (e.g., 1–5).
    - risk_rating (Step 4): Composite score (e.g., L × I).
    - mitigation_plan (Step 5): Suggested response or control strategy.
    - owner_and_status (Step 5): Role responsible and current handling state.

  inputs_required:
    - Initiative scope, stakeholder list, or assumptions
    - Historical risk data or sector-specific threats
    - Governance policies and decision logic

  outputs_produced:
    - Risk Register or Risk Matrix
    - Mitigation Action Log
    - Risk Summary Dashboard

  template_guidance:
    usage: >
      Use a risk analysis template to standardize identification, scoring, and tracking.
      Tailor categories, scales, and thresholds to Vinci’s Pillars and initiative type.
    required_templates:
      - Risk Register Template
      - Risk Heatmap or Scoring Grid
    structure_notes:
      - Fields: Risk | Category | Likelihood | Impact | Score | Plan | Owner | Status | Trace Link
      - Visuals: risk heatmaps, radar charts, impact-likelihood matrix
    vinci_automation:
      - GPT can suggest risk candidates by phase or stakeholder type
      - Auto-calculate composite scores and risk priorities
      - Highlight unmitigated risks or overdue responses

  vinci_extensions:
    - Tag risks by Vinci Pillars and initiative trace links
    - Use aggregated risk data for strategy and governance reviews
    - Track residual risk and re-score post-mitigation
    - Store cross-client risk patterns for predictive insights

technique:
  id: T043
  name: Root Cause Analysis

  purpose: >
    Root Cause Analysis (RCA) identifies the underlying causes of problems or failures
    rather than just treating symptoms. It enables targeted corrective actions that
    improve long-term performance and prevent recurrence.

  common_use_cases:
    - Analyzing pain points discovered in discovery or develop phases
    - Explaining low KPI performance or repeated delivery issues
    - Investigating process failures, rework, or stakeholder conflict

  procedural_steps:
    - Step 1: Define the problem clearly and specifically.
    - Step 2: Gather evidence and context from affected areas or stakeholders.
    - Step 3: Apply structured techniques (e.g., 5 Whys, Fishbone Diagram, Fault Tree).
    - Step 4: Identify root causes and distinguish them from symptoms.
    - Step 5: Recommend targeted actions or redesigns to resolve root issues.

  metadata_fields:
    - problem_statement (Step 1): Clearly defined issue under investigation.
    - symptom_list (Step 2): Observable effects or complaints.
    - root_cause_candidates (Step 4): Underlying causes uncovered.
    - evidence_sources (Step 2): Where data or observations came from.
    - selected_cause (Step 4): Final confirmed root cause.
    - recommended_fix (Step 5): Corrective action or change to address cause.

  inputs_required:
    - Performance data, feedback logs, or observed issues
    - Access to involved processes, roles, or systems
    - Contextual knowledge from stakeholders or SMEs

  outputs_produced:
    - Root Cause Map or Diagram
    - RCA Report or Summary
    - Action Plan to Address Root Cause

  template_guidance:
    usage: >
      RCA templates help ensure clear thinking, structured documentation, and targeted problem resolution.
      Should be used in conjunction with risk logs, KPI analysis, or performance reviews.
    required_templates:
      - Root Cause Worksheet (5 Whys, Fishbone, etc.)
      - Action Recommendation Tracker
    structure_notes:
      - Fields: Symptom | Why | Evidence | Root Cause | Fix | Owner | Trace Link
      - Visuals: Fishbone Diagram, Causal Chain, Fault Tree
    vinci_automation:
      - GPT can suggest likely causes based on problem statements
      - Auto-structure causal logic using 5 Whys
      - Link root causes to roles, systems, and Vinci Pillars

  vinci_extensions:
    - Feed RCA findings into continuous improvement registry
    - Tag root causes across clients to reveal common patterns
    - Link to initiative scoping and solution redesign
    - Store causal models in ontology for reuse and validation

technique:
  id: T044
  name: Scenario Analysis

  purpose: >
    Scenario Analysis evaluates multiple plausible future situations to understand their potential impact,
    test resilience, and support better strategic decision-making under uncertainty.

  common_use_cases:
    - Planning for risks or unknowns in strategy or initiative design
    - Comparing outcomes of alternative decision paths
    - Supporting alignment between value, risk, and readiness

  procedural_steps:
    - Step 1: Define the focal question or decision that needs to be tested.
    - Step 2: Identify the key uncertainties, drivers, or variables.
    - Step 3: Construct alternative scenarios based on combinations of assumptions.
    - Step 4: Analyze the implications of each scenario (e.g., cost, value, risk).
    - Step 5: Recommend actions, mitigations, or design shifts based on scenario insight.

  metadata_fields:
    - focal_decision (Step 1): The topic, plan, or risk being analyzed.
    - key_drivers (Step 2): Major variables that influence scenario outcomes.
    - scenario_names (Step 3): Labels for each tested case (e.g., Best Case, Disruption, Regulation Shift).
    - implications (Step 4): Outcomes or effects of each scenario.
    - preferred_strategy (Step 5): Chosen plan or safeguard based on scenario review.

  inputs_required:
    - Strategic assumptions, drivers, or trends
    - Risk register, forecasts, or planning documents
    - Stakeholder goals or constraints

  outputs_produced:
    - Scenario Matrix or Narrative Profiles
    - Risk-Adjusted Strategy Options
    - Recommendation Brief or Response Plan

  template_guidance:
    usage: >
      Scenario templates guide the logic, structure, and comparison of alternative futures.
      Use to test resilience and strengthen design in strategic and planning phases.
    required_templates:
      - Scenario Comparison Matrix
      - Scenario Planning Canvas
    structure_notes:
      - Fields: Scenario | Assumptions | Drivers | Risks | Outcomes | Implications | Strategy
      - Visuals: spider charts, heatmaps, future wheels
    vinci_automation:
      - GPT can generate scenarios from planning assumptions
      - Auto-score options by value, risk, or feasibility
      - Link scenario outcomes to initiative models or stakeholder risk profiles

  vinci_extensions:
    - Store tested scenarios in Vinci ontology for reference or reuse
    - Use scenario risk logic to influence prioritization or capability design
    - Trigger safeguards or fallbacks based on high-risk futures
    - Align scenario insights to Pillars and stakeholder planning

technique:
  id: T045
  name: Stakeholder Analysis

  purpose: >
    Stakeholder Analysis identifies individuals, groups, or roles affected by or capable of influencing
    a project, and analyzes their interests, expectations, influence, and potential engagement needs.

  common_use_cases:
    - Defining engagement strategies and communication plans
    - Detecting hidden risks, influencers, or blockers
    - Mapping alignment or conflict zones across the initiative

  procedural_steps:
    - Step 1: Identify all stakeholders based on initiative scope.
    - Step 2: Analyze each stakeholder’s interest, influence, expectations, and role.
    - Step 3: Categorize stakeholders into engagement segments.
    - Step 4: Define engagement strategies per segment.
    - Step 5: Monitor and update analysis as relationships or context evolve.

  metadata_fields:
    - stakeholder_name (Step 1): Role, person, team, or entity.
    - interest_level (Step 2): Degree of involvement or concern.
    - influence_level (Step 2): Ability to affect decisions, pace, or outcome.
    - expectations (Step 2): Goals, needs, or assumptions held by stakeholder.
    - engagement_segment (Step 3): Role-based grouping (e.g., champion, resistor, passive, neutral).
    - strategy_notes (Step 4): Actions, cadence, or formats to engage them.
    - status_monitoring (Step 5): Level of activity or shift in stance over time.

  inputs_required:
    - Stakeholder map, role model, or org structure
    - Initiative scope, design goals, and impact zones
    - Collaboration and communication records (if applicable)

  outputs_produced:
    - Stakeholder Analysis Table
    - Engagement Strategy Overview
    - Risk and Alignment Heatmap

  template_guidance:
    usage: >
      Use stakeholder analysis templates to consistently classify and manage relationships,
      especially in Engage, Align, and Plan phases.
    required_templates:
      - Stakeholder Profiling Table
      - Influence-Interest Map or Engagement Plan
    structure_notes:
      - Fields: Name | Role | Interest | Influence | Segment | Strategy | Trace Link
      - Visuals: power/interest grids, persona clusters, sentiment dashboards
    vinci_automation:
      - GPT can suggest engagement strategies by segment or stakeholder type
      - Track stakeholder movement across alignment or resistance zones
      - Auto-update stakeholder impact traces based on design or scope shifts

  vinci_extensions:
    - Store stakeholder models in the Vinci ontology with trace links
    - Link roles to communication cadence, governance plans, and capability maps
    - Highlight misalignment or neglect risks via status monitoring
    - Use sentiment and behavior patterns to adjust engagement in real time

technique:
  id: T046
  name: Stakeholder Mapping

  purpose: >
    Stakeholder Mapping visually organizes all individuals, groups, or systems involved in an initiative,
    showing relationships, roles, influence pathways, and relevance to deliverables or decisions.

  common_use_cases:
    - Structuring stakeholder engagement and collaboration plans
    - Revealing informal power networks or influence chains
    - Supporting governance, RACI, or communication models

  procedural_steps:
    - Step 1: Identify all stakeholders from the role model, org chart, or scope review.
    - Step 2: Classify stakeholders by role, function, or contribution.
    - Step 3: Define the relationships between stakeholders (influence, reporting, dependency).
    - Step 4: Visualize the network, clusters, and critical links.
    - Step 5: Validate the map and use it to align traceability, collaboration, and communication.

  metadata_fields:
    - stakeholder_name (Step 1): Individual, team, or system identifier.
    - stakeholder_role (Step 2): Function, title, or influence type (e.g., approver, blocker).
    - relationship_type (Step 3): Type of link (e.g., reports to, consults, collaborates with).
    - influence_level (Step 2/3): Degree of impact or authority.
    - trace_link (Step 5): Connection to models, initiatives, or governance flows.

  inputs_required:
    - Org model, stakeholder list, or collaboration logs
    - Governance structure or communication plans
    - Role expectations or documentation (e.g., charters, mandates)

  outputs_produced:
    - Stakeholder Network Diagram
    - Relationship and Influence Map
    - Stakeholder Engagement Tracker

  template_guidance:
    usage: >
      Mapping templates help organize complex stakeholder environments into visual,
      structured assets to support decision-making and role clarity.
    required_templates:
      - Stakeholder Mapping Canvas
      - Stakeholder Relationship Table
    structure_notes:
      - Fields: Name | Role | Reports To | Relationship | Influence | Cluster | Notes
      - Visuals: network graphs, swimlanes, layered pyramids
    vinci_automation:
      - GPT can auto-generate maps from RACI or engagement plans
      - Detect missing roles or broken communication lines
      - Suggest clusters or engagement priorities

  vinci_extensions:
    - Encode stakeholder maps into the Vinci ontology for reuse and traceability
    - Use influence paths to design escalation and communication structures
    - Map relationships to Pillars and capability alignment
    - Visualize collaboration friction or blind spots across phases

technique:
  id: T047
  name: State Modelling

  purpose: >
    State Modelling identifies and visualizes the distinct states an entity can exist in over time,
    and the transitions that cause movement between those states. It helps clarify rules, lifecycle behavior,
    and system interactions.

  common_use_cases:
    - Defining lifecycle rules for data, documents, roles, or products
    - Validating business rules or automation logic
    - Supporting process and system design where conditional transitions matter

  procedural_steps:
    - Step 1: Select the entity or object to be modeled (e.g., request, task, asset).
    - Step 2: Define all possible states it can occupy.
    - Step 3: Identify the triggers or events that cause transitions.
    - Step 4: Create a visual model showing states and transitions.
    - Step 5: Validate the logic and integrate into requirements or design documents.

  metadata_fields:
    - entity_name (Step 1): What is being modeled (e.g., Invoice, Case, User Role).
    - state_list (Step 2): All defined states (e.g., Draft, Submitted, Approved, Closed).
    - transition_events (Step 3): Conditions, actions, or triggers that move the state.
    - entry_criteria (Step 3): What allows a state to be entered.
    - exit_criteria (Step 3): What must be true to leave a state.
    - trace_usage (Step 5): Where state logic connects to system, process, or requirement.

  inputs_required:
    - Domain knowledge or stakeholder input
    - Process flows, rules, or data lifecycle descriptions
    - Automation or logic dependencies

  outputs_produced:
    - State Diagram (UML or custom notation)
    - State-Transition Table
    - Rule-Requirement Trace Map

  template_guidance:
    usage: >
      State models should be used when lifecycle behavior matters—especially in data-heavy, automated,
      or compliance-sensitive systems. A template helps ensure consistency and integration with trace models.
    required_templates:
      - State Transition Table
      - State Diagram Template
    structure_notes:
      - Fields: State | Description | Entry | Exit | Trigger | Next States | Notes
      - Visuals: circular, flow, or matrix-based diagrams
    vinci_automation:
      - GPT can suggest transitions from workflow or rule inputs
      - Highlight missing states or orphan transitions
      - Generate models from conditional logic in requirements

  vinci_extensions:
    - Store models in Vinci ontology for reference and alignment
    - Trace state logic to user stories, systems, and compliance conditions
    - Detect logic conflicts or unmet exit/entry criteria during validation
    - Use state transitions to drive monitoring, risk alerts, or user notifications

technique:
  id: T048
  name: Strategy Mapping

  purpose: >
    Strategy Mapping visually links business goals to the capabilities, initiatives, roles, and outcomes
    required to achieve them. It aligns operational efforts to strategic vision and facilitates prioritization,
    communication, and measurement.

  common_use_cases:
    - Translating strategy into traceable operational actions
    - Aligning initiatives with goals, KPIs, and Pillars
    - Identifying logical gaps or overlaps in planning

  procedural_steps:
    - Step 1: Define strategic goals or transformation themes.
    - Step 2: Identify the enabling capabilities, processes, or systems.
    - Step 3: Link initiatives or activities that build or activate those enablers.
    - Step 4: Map outcomes and KPIs associated with each path.
    - Step 5: Visualize the entire strategy map for alignment and review.

  metadata_fields:
    - strategic_goal (Step 1): Desired long-term outcome or value target.
    - enabling_element (Step 2): Capability, resource, or condition needed.
    - linked_initiative (Step 3): Project or effort supporting activation.
    - kpi_or_outcome (Step 4): Metric indicating success or progress.
    - trace_links (Step 5): Connections to roles, requirements, or governance.

  inputs_required:
    - Strategic plans or leadership goals
    - Capability maps, performance data, or KPIs
    - Initiative catalog or planning portfolio

  outputs_produced:
    - Strategy Map Diagram
    - Goal-to-Initiative Trace Table
    - KPI Alignment Summary

  template_guidance:
    usage: >
      Use strategy mapping templates to ensure traceability from abstract goals to executable actions,
      especially in the Align and Plan phases.
    required_templates:
      - Strategy Map Canvas
      - Strategic Alignment Table
    structure_notes:
      - Fields: Goal | Enabler | Initiative | Metric | Owner | Priority | Status
      - Visuals: layered cascade maps, tree structures, strategy ladders
    vinci_automation:
      - GPT can construct draft maps from strategic inputs and initiative lists
      - Flag unsupported goals or duplicate initiatives
      - Suggest metrics or Pillar connections for each branch

  vinci_extensions:
    - Encode strategy maps in Vinci ontology for traceable alignment
    - Use map gaps to drive initiative proposals or risk identification
    - Store maps for future reviews, cross-client reuse, and outcome tracking
    - Integrate maps into dashboards and executive briefings

technique:
  id: T049
  name: Surveys and Questionnaires

  purpose: >
    Surveys and Questionnaires are structured tools used to collect feedback, insights, or preferences
    from individuals or groups. They enable scalable data collection across stakeholder segments.

  common_use_cases:
    - Gathering input from distributed or time-constrained stakeholders
    - Assessing readiness, perception, or satisfaction
    - Quantifying stakeholder needs, concerns, or performance insights

  procedural_steps:
    - Step 1: Define the goal and scope of the survey.
    - Step 2: Choose question types (e.g., Likert, multiple choice, open-ended).
    - Step 3: Design and test the questionnaire.
    - Step 4: Distribute survey to selected participants and monitor responses.
    - Step 5: Analyze results and synthesize insights into actions or decisions.

  metadata_fields:
    - survey_goal (Step 1): The purpose of the survey or what it's intended to discover.
    - participant_group (Step 4): Target audience (e.g., leadership, users, clients).
    - question_type (Step 2): Format of each question.
    - response_data (Step 5): Collected answers and response metrics.
    - insights_summary (Step 5): Key findings or themes.
    - trace_links (Step 5): Where results affect requirements, risks, or models.

  inputs_required:
    - Defined objective or assumption to test
    - Stakeholder contact list or access permissions
    - Survey platform or tool (optional: Excel, Google Forms, Typeform)

  outputs_produced:
    - Survey Results Summary
    - Quantitative and Qualitative Data Tables
    - Stakeholder Insight Tracker

  template_guidance:
    usage: >
      Use standard survey templates with validated question types and response logic
      to ensure comparability, insight quality, and alignment to Vinci models.
    required_templates:
      - Survey Design Template
      - Results Analysis Grid
    structure_notes:
      - Fields: Question | Type | Target Role | Response | Theme | Action Link
      - Visuals: charts, sentiment graphs, response distributions
    vinci_automation:
      - GPT can generate or refine questions based on survey goals
      - Auto-cluster qualitative answers into themes
      - Generate summary slides or insight decks from raw responses

  vinci_extensions:
    - Map findings to Pillars and stakeholder alignment profiles
    - Store high-value response patterns in Vinci ontology for trend analysis
    - Use response rate and sentiment as friction signals
    - Trigger stakeholder re-engagement or design review based on insights

technique:
  id: T050
  name: SWOT Analysis

  purpose: >
    SWOT Analysis is a strategic planning tool used to identify and evaluate internal Strengths and Weaknesses,
    along with external Opportunities and Threats. It supports high-level assessment and decision alignment.

  common_use_cases:
    - Early-phase discovery or strategy formulation
    - Assessing capability health and market position
    - Supporting initiative alignment or prioritization

  procedural_steps:
    - Step 1: Define the focus area for the analysis (organization, capability, initiative, etc.).
    - Step 2: Identify internal strengths and weaknesses (resources, systems, talent, structure).
    - Step 3: Identify external opportunities and threats (market, trends, competition, regulation).
    - Step 4: Group and prioritize insights into key themes.
    - Step 5: Use findings to support strategy, design, or initiative planning.

  metadata_fields:
    - analysis_scope (Step 1): What is being evaluated (e.g., sales team, ERP capability).
    - strength_list (Step 2): Positive internal attributes or assets.
    - weakness_list (Step 2): Internal deficiencies or friction points.
    - opportunity_list (Step 3): External trends or openings to exploit.
    - threat_list (Step 3): External risks or challenges.
    - insight_clusters (Step 4): Themes derived from SWOT entries (e.g., scalability, tech dependency).
    - action_traces (Step 5): How each SWOT element informs plans or tasks.

  inputs_required:
    - Stakeholder input, discovery findings, or external research
    - Internal performance data or role feedback
    - Strategic assumptions or objectives

  outputs_produced:
    - SWOT Matrix or Summary Grid
    - Strategic Insight Summary
    - Trace Links to Plans, Risks, or Initiatives

  template_guidance:
    usage: >
      Use SWOT templates to structure group discussions or solo analysis sessions.
      Translate raw entries into Vinci-aligned insight clusters or actions.
    required_templates:
      - SWOT Matrix Template
      - SWOT Insight Tracker
    structure_notes:
      - Fields: Item | Category (S/W/O/T) | Description | Priority | Action | Trace Link
      - Visuals: 2x2 grid, bubble map, radar chart
    vinci_automation:
      - GPT can generate draft SWOT from transcripts or role feedback
      - Auto-suggest external threats from industry patterns
      - Highlight entries that lack traceability or actionable links

  vinci_extensions:
    - Map SWOT clusters to Vinci Pillars and capabilities
    - Use SWOT themes to prioritize initiatives or frame value maps
    - Store SWOT logs in the ontology for sector benchmarking
    - Feed high-risk or high-opportunity items into planning triggers

technique:
  id: T051
  name: Tailoring Guidelines

  purpose: >
    Tailoring Guidelines provide structured methods for adjusting tools, templates, techniques, and processes
    to fit the specific context, scale, and needs of a project, stakeholder group, or organization.

  common_use_cases:
    - Adjusting Vinci methods for public vs private sector engagements
    - Scaling deliverables for rapid or low-risk initiatives
    - Simplifying collaboration tools or governance based on team size

  procedural_steps:
    - Step 1: Assess the context (size, scope, risk, urgency, stakeholder availability).
    - Step 2: Identify which techniques, templates, or artifacts can be adjusted.
    - Step 3: Apply tailoring logic based on predefined Vinci thresholds or patterns.
    - Step 4: Document what has been tailored and why.
    - Step 5: Review tailoring outcomes during governance or retrospective cycles.

  metadata_fields:
    - tailoring_context (Step 1): The factors influencing the need to adapt (e.g., timeboxed project, startup client).
    - component_adjusted (Step 2): Artifact, process, or technique that is being modified.
    - tailoring_decision (Step 3): Rule or rationale behind the adjustment.
    - documentation_reference (Step 4): Where the adjustment is recorded.
    - validation_feedback (Step 5): Whether the tailoring was successful or needs correction.

  inputs_required:
    - Engagement brief or project charter
    - Vinci delivery framework and reference templates
    - Stakeholder availability, risk profile, or timeline expectations

  outputs_produced:
    - Tailoring Summary Log
    - Adjusted Templates or Process Maps
    - Governance Adjustment Tracker

  template_guidance:
    usage: >
      Tailoring templates help consistently manage adaptation decisions, ensuring traceability and quality
      while avoiding method erosion.
    required_templates:
      - Tailoring Decision Log
      - Component Adjustment Sheet
    structure_notes:
      - Fields: Item | Adjustment | Reason | Rule Applied | Owner | Trace Link | Outcome
      - Optional visuals: adjustment trees, delivery scale matrix
    vinci_automation:
      - GPT can recommend tailoring actions based on project profile
      - Auto-generate justification and template suggestions
      - Compare adjusted vs standard models and flag deviations

  vinci_extensions:
    - Encode tailoring logic into Vinci assistant workflows
    - Track usage of tailoring by sector, phase, or engagement type
    - Feed lessons from tailoring into continuous improvement
    - Maintain ontology links even across adjusted formats

technique:
  id: T052
  name: Traceability Mapping

  purpose: >
    Traceability Mapping ensures that all requirements, decisions, models, and deliverables are
    logically connected across the lifecycle—supporting alignment, validation, impact analysis, and auditability.

  common_use_cases:
    - Linking requirements to business needs, stakeholders, and test cases
    - Supporting impact analysis for change requests
    - Ensuring coverage and completeness across phases and roles

  procedural_steps:
    - Step 1: Define trace anchors (e.g., needs, goals, stakeholders, systems).
    - Step 2: Identify traceable items (requirements, decisions, risks, capabilities).
    - Step 3: Establish links between items and categorize relationship types.
    - Step 4: Visualize or structure trace matrix or network.
    - Step 5: Monitor and update trace links as changes occur.

  metadata_fields:
    - anchor_point (Step 1): The object of origin (e.g., stakeholder, goal, KPI).
    - traceable_item (Step 2): Requirement, task, decision, risk, or model element.
    - relationship_type (Step 3): Nature of connection (e.g., derived from, satisfies, conflicts with).
    - trace_strength (Optional): Confidence or criticality of link.
    - status_flag (Step 5): Whether the link is active, broken, or pending validation.

  inputs_required:
    - Validated requirements, models, and design elements
    - Role map, system map, or Pillar structure
    - Governance records and change logs

  outputs_produced:
    - Traceability Matrix
    - Relationship Map or Network Diagram
    - Impact and Coverage Report

  template_guidance:
    usage: >
      Traceability templates help standardize relationships, highlight gaps, and support change management.
      They are foundational to Vinci’s model-driven methodology.
    required_templates:
      - Traceability Matrix Template
      - Impact Analysis Grid
    structure_notes:
      - Fields: Anchor | Item | Type | Strength | Phase | Pillar | Status
      - Visuals: node-link graphs, heatmaps, coverage charts
    vinci_automation:
      - GPT can detect traceable pairs from task or requirement text
      - Highlight unlinked or broken trace paths
      - Auto-update matrices from decision logs or requirement changes

  vinci_extensions:
    - Store trace maps in the Vinci ontology for lifecycle reuse
    - Trigger trace validation checks during governance reviews
    - Drive initiative prioritization based on uncovered gaps
    - Enable forward and backward trace from any delivery point

technique:
  id: T053
  name: Trend Analysis

  purpose: >
    Trend Analysis is used to detect patterns, shifts, and recurring dynamics in data or behavior
    over time. It supports forecasting, risk anticipation, and strategic or performance alignment.

  common_use_cases:
    - Identifying performance improvement or degradation over time
    - Detecting cultural, stakeholder, or KPI behavior patterns
    - Supporting planning or strategy adjustment based on signals

  procedural_steps:
    - Step 1: Define what is being monitored and over what time horizon.
    - Step 2: Gather historical and current data across defined intervals.
    - Step 3: Visualize and analyze directional patterns or shifts.
    - Step 4: Interpret causes, risks, or opportunities suggested by the trends.
    - Step 5: Recommend adjustments, actions, or monitoring changes.

  metadata_fields:
    - trend_subject (Step 1): KPI, stakeholder, process, role, or sentiment.
    - time_window (Step 1): Interval size and time span analyzed.
    - data_source (Step 2): Where information originates.
    - trend_pattern (Step 3): Direction, intensity, or fluctuation type.
    - interpretation_notes (Step 4): Narrative insight or causal hypothesis.
    - response_action (Step 5): What’s being proposed based on insight.

  inputs_required:
    - Historical or current data (quantitative or qualitative)
    - KPI logs, feedback cycles, or role activity records
    - Visualization tools or analysis templates

  outputs_produced:
    - Trend Summary Report
    - Time-Based Visualization (e.g., charts, graphs)
    - Action or Adjustment Recommendations

  template_guidance:
    usage: >
      Trend templates help document findings, visualize patterns, and track decisions based on directional insight.
      Use for both operational and strategic evaluation across Vinci phases.
    required_templates:
      - Trend Analysis Sheet
      - Recommendation Log
    structure_notes:
      - Fields: Topic | Time | Value | Direction | Cause | Impact | Decision
      - Visuals: line charts, stacked bars, heat timelines
    vinci_automation:
      - GPT can highlight patterns in time-series data
      - Auto-generate visuals or detect inflection points
      - Suggest linked Pillars or roles based on signal origin

  vinci_extensions:
    - Track long-term movement across clients, phases, or domains
    - Feed trend insights into planning, risk, or stakeholder loops
    - Map trends to Pillars to guide continuous improvement
    - Trigger alerts when strategic thresholds are breached

technique:
  id: T054
  name: Use Case Walkthroughs

  purpose: >
    Use Case Walkthroughs are structured reviews of user interactions with a system or process,
    using narrative scenarios to validate requirements, clarify behavior, and uncover edge cases or gaps.

  common_use_cases:
    - Confirming functional requirements with stakeholders
    - Clarifying interface or workflow design logic
    - Testing completeness of requirements or business rules

  procedural_steps:
    - Step 1: Define the use case scope (actor, goal, system interaction).
    - Step 2: Write the scenario with preconditions, triggers, and step-by-step actions.
    - Step 3: Conduct the walkthrough with relevant stakeholders or testers.
    - Step 4: Collect questions, assumptions, or design conflicts raised during discussion.
    - Step 5: Update use case, requirements, or trace records based on feedback.

  metadata_fields:
    - use_case_id (Step 1): Unique reference or label.
    - actor (Step 1): Primary user or role performing the use case.
    - scenario_steps (Step 2): Action-by-action breakdown of flow.
    - exceptions (Step 2): Alternative or error paths.
    - review_feedback (Step 4): Issues, insights, or recommendations.
    - trace_links (Step 5): Connection to requirements, models, or Pillars.

  inputs_required:
    - Requirements or process logic
    - User or role definitions
    - Preconditions or assumptions

  outputs_produced:
    - Use Case Narrative
    - Walkthrough Feedback Log
    - Requirement Refinement Notes

  template_guidance:
    usage: >
      Use walkthrough templates to maintain clarity and traceability in each scenario,
      especially in complex flows or role-dependent systems.
    required_templates:
      - Use Case Description Template
      - Walkthrough Feedback Table
    structure_notes:
      - Fields: Use Case | Actor | Step | Condition | Outcome | Issue | Fix | Trace ID
      - Visuals: swimlanes, flowcharts, annotated screenshots
    vinci_automation:
      - GPT can generate use case flows from role and goal inputs
      - Extract feedback and auto-link to gaps or rework items
      - Suggest test scenarios from edge conditions

  vinci_extensions:
    - Encode use cases into Vinci trace model for reuse and simulation
    - Link to prototype validation, KPI impact, and risk detection
    - Feed scenarios into continuous improvement or regression analysis
    - Store structured walkthroughs in the ontology for training or documentation

technique:
  id: T055
  name: Use Cases and Scenarios

  purpose: >
    Use Cases and Scenarios describe how users interact with systems, processes, or services
    to achieve specific goals. They clarify functional expectations, boundaries, and context
    for requirement definition, design, and testing.

  common_use_cases:
    - Describing system-user interactions for solution design
    - Defining process requirements or exception handling
    - Aligning technical teams with business goals and user needs

  procedural_steps:
    - Step 1: Identify primary actors and define their goals.
    - Step 2: Write use case descriptions with main flows, alternate flows, and exceptions.
    - Step 3: Create supporting scenarios to illustrate variation, complexity, or stakeholder context.
    - Step 4: Validate scenarios with business and technical stakeholders.
    - Step 5: Link use cases and scenarios to requirements, roles, and system features.

  metadata_fields:
    - use_case_id (Step 1): Unique ID or label.
    - actor (Step 1): User or system interacting with the solution.
    - goal_statement (Step 1): Objective the actor is trying to achieve.
    - main_flow (Step 2): Standard step-by-step interaction.
    - alternate_flows (Step 2): Variations based on decisions or data.
    - exception_flows (Step 2): Error or non-happy path handling.
    - linked_requirements (Step 5): Requirement IDs or trace tags.

  inputs_required:
    - Stakeholder goals and business process knowledge
    - Initial or draft requirements
    - Access to actors, users, or simulations (optional)

  outputs_produced:
    - Use Case Narratives
    - Scenario Library
    - Requirement and System Trace Matrix

  template_guidance:
    usage: >
      Use a structured use case template with clear actor goals and flows
      to improve stakeholder validation and traceable design.
    required_templates:
      - Use Case Specification Template
      - Scenario Matrix or Narrative Table
    structure_notes:
      - Fields: Actor | Goal | Precondition | Steps | Outcome | Exception | Trace Link
      - Visuals: sequence diagrams, swimlanes, activity flows
    vinci_automation:
      - GPT can auto-generate use case outlines from goals or process input
      - Detect logic or completeness gaps
      - Auto-link steps to functional or non-functional requirements

  vinci_extensions:
    - Store use case models in Vinci ontology for reuse and alignment
    - Use scenarios to drive test case generation or prototyping
    - Link user flows to role models and cultural assessments
    - Feed into change impact and communication planning

technique:
  id: T056
  name: Value Mapping

  purpose: >
    Value Mapping visually links initiatives, capabilities, or deliverables to the business value
    they create—making it easier to prioritize, justify, and communicate impact across stakeholders.

  common_use_cases:
    - Aligning initiatives with strategic goals or KPIs
    - Prioritizing backlogs, requirements, or capabilities
    - Supporting business case justification and change strategy

  procedural_steps:
    - Step 1: Define the categories of value relevant to the initiative (e.g., efficiency, compliance, growth).
    - Step 2: Identify initiatives, requirements, or deliverables to be evaluated.
    - Step 3: Link each item to one or more value outcomes.
    - Step 4: Assess contribution strength or priority for each link.
    - Step 5: Visualize the value map and trace impact forward or backward.

  metadata_fields:
    - value_category (Step 1): Type of value (e.g., financial, customer, operational).
    - deliverable_or_item (Step 2): What is being evaluated.
    - contribution_type (Step 3): How the item supports value (e.g., enables, accelerates, protects).
    - contribution_strength (Step 4): Weight or score indicating impact level.
    - trace_link (Step 5): Connection to initiatives, Pillars, or KPIs.

  inputs_required:
    - Strategic goals, business case, or initiative list
    - KPI library or value dimensions
    - Stakeholder input (optional)

  outputs_produced:
    - Value Map Diagram or Table
    - Value Contribution Summary
    - Prioritization Input Sheet

  template_guidance:
    usage: >
      Value maps help ensure delivery efforts are always linked to value creation logic.
      Use these templates to inform backlog grooming, roadmap design, and executive reporting.
    required_templates:
      - Value Mapping Table
      - Value Contribution Matrix
    structure_notes:
      - Fields: Item | Value Type | Contribution | Strength | Trace ID | Pillar
      - Visuals: arrows, bubble maps, tiered ladders
    vinci_automation:
      - GPT can auto-link items to value types using requirement context
      - Suggest value categories by sector or strategy
      - Highlight weak or unsupported items

  vinci_extensions:
    - Encode value maps into Vinci’s trace model for prioritization
    - Use mapping gaps to trigger new discovery or design cycles
    - Visualize cumulative value contribution by Pillar or capability
    - Store maps for reuse across similar initiative types

technique:
  id: T057
  name: Value Stream Mapping

  purpose: >
    Value Stream Mapping (VSM) visualizes the steps, delays, and flows involved in delivering value
    to a customer or stakeholder. It helps identify waste, bottlenecks, and opportunities for improvement
    across processes or functions.

  common_use_cases:
    - Analyzing end-to-end value delivery processes
    - Identifying waste or inefficiency in current state operations
    - Supporting Lean, transformation, or capability enhancement initiatives

  procedural_steps:
    - Step 1: Define the scope and value being delivered (product, service, decision).
    - Step 2: Map each step of the process with actors, inputs, outputs, and timing.
    - Step 3: Capture delays, wait times, handoffs, and system interactions.
    - Step 4: Identify non-value-adding steps or friction zones.
    - Step 5: Recommend changes to improve flow, reduce waste, or accelerate outcomes.

  metadata_fields:
    - value_target (Step 1): What the stream delivers and for whom.
    - step_name (Step 2): Each discrete activity or handoff.
    - actor_or_role (Step 2): Who performs each step.
    - process_time (Step 3): Duration of active work.
    - lead_time (Step 3): Total elapsed time including wait.
    - waste_type (Step 4): Delay, defect, overprocessing, motion, etc.
    - improvement_recommendation (Step 5): Proposed fix or redesign.

  inputs_required:
    - Process maps, stakeholder interviews, or system data
    - Capability models or value definitions
    - Access to performance metrics or logs (optional)

  outputs_produced:
    - Current State Value Stream Map
    - Waste and Delay Analysis
    - Future State Flow and Recommendations

  template_guidance:
    usage: >
      Use VSM templates to standardize step logging, time tracking, and handoff clarity.
      Ideal for Discover, Align, and Develop phases.
    required_templates:
      - VSM Data Table
      - Value Stream Diagram Template
    structure_notes:
      - Fields: Step | Role | Input | Output | Time | Wait | System | Waste | Fix
      - Visuals: horizontal flowchart with icons for timing, delays, and metrics
    vinci_automation:
      - GPT can generate VSM tables from interview summaries or process models
      - Auto-calculate lead and process times if data is available
      - Highlight bottlenecks or delay clusters

  vinci_extensions:
    - Map VSM flows to Vinci Pillars and capabilities
    - Feed improvement actions into plan-phase initiative scoping
    - Compare current and future states to visualize transformation impact
    - Store stream models in the ontology for reuse and benchmarking

technique:
  id: T058
  name: Version Control

  purpose: >
    Version Control ensures that changes to documents, requirements, models, or deliverables
    are systematically tracked, referenced, and managed across iterations, reducing errors and enabling auditability.

  common_use_cases:
    - Managing updates to requirements, templates, or design artifacts
    - Supporting governance and review cycles
    - Enabling traceable collaboration across multiple roles or phases

  procedural_steps:
    - Step 1: Define what needs version control and the granularity required (document, field, model).
    - Step 2: Establish versioning rules and naming conventions.
    - Step 3: Track all changes and include version IDs, dates, and authors.
    - Step 4: Archive previous versions with clear rationale for changes.
    - Step 5: Use version records during validation, review, or audit.

  metadata_fields:
    - item_id (Step 1): Document, requirement, or model name.
    - version_number (Step 3): Identifier (e.g., v1.0, v2.3-draft).
    - change_description (Step 3): What was added, changed, or removed.
    - author_or_editor (Step 3): Who made the change.
    - change_date (Step 3): When the change was applied.
    - approval_status (Step 5): Draft, reviewed, approved, obsolete.

  inputs_required:
    - Editable documents or templates
    - Governance or quality requirements
    - Collaboration agreements (optional)

  outputs_produced:
    - Version Control Log
    - Archived Deliverable Versions
    - Change Summary for Review

  template_guidance:
    usage: >
      Use version control templates to manage evolution of Vinci deliverables across phases.
      Helps ensure audit trails, change justification, and team coordination.
    required_templates:
      - Version History Table
      - Change Log Form
    structure_notes:
      - Fields: Item | Version | Date | Author | Change Summary | Status | Notes
      - Visuals: change trees, history timelines (optional)
    vinci_automation:
      - GPT can track version evolution and auto-tag changes
      - Flag conflicting versions or skipped steps
      - Generate version summaries for governance review

  vinci_extensions:
    - Store version history in the Vinci trace model
    - Compare versions to detect inconsistencies or loss of trace links
    - Feed version patterns into lessons learned or retrospectives
    - Trigger stakeholder reviews on specific version events

technique:
  id: T059
  name: Visual Diagramming

  purpose: >
    Visual Diagramming is the use of structured graphics to represent relationships, flows, structures,
    or processes in a way that supports understanding, validation, decision-making, and traceability.

  common_use_cases:
    - Modeling processes, systems, roles, or data structures
    - Supporting workshops, stakeholder communication, or walkthroughs
    - Enhancing traceability and reducing cognitive complexity

  procedural_steps:
    - Step 1: Define what needs to be visualized (e.g., workflow, hierarchy, data model).
    - Step 2: Select the most appropriate diagram type (flowchart, swimlane, tree, network, matrix).
    - Step 3: Gather inputs and relationships needed to build the diagram.
    - Step 4: Build and label the diagram clearly and consistently.
    - Step 5: Review and validate the visual with stakeholders.

  metadata_fields:
    - diagram_type (Step 2): Style used (e.g., BPMN, tree, matrix).
    - model_scope (Step 1): What is being depicted (process, role, system, requirement).
    - elements (Step 3): Nodes, roles, steps, or objects represented.
    - relationships (Step 3): Arrows, flows, or connectors between elements.
    - status (Step 5): Draft, reviewed, validated, versioned.

  inputs_required:
    - Structured model content or source materials (e.g., process table, stakeholder map)
    - Design standards, Pillar relationships, or Vinci ontology references

  outputs_produced:
    - Completed Visual Diagram
    - Accompanying Metadata Table
    - Validated Traceable Model

  template_guidance:
    usage: >
      Use visual diagramming templates to structure layout, labels, and logic for Vinci-standard artifacts.
      Improves reuse and reduces modeling effort.
    required_templates:
      - Diagram Specification Template
      - Element Relationship Table
    structure_notes:
      - Fields: Element | Type | Position | Link | Label | Notes | Pillar
      - Visual formats: swimlane, tree, circular map, layered canvas
    vinci_automation:
      - GPT can suggest layout type and elements based on scope
      - Auto-generate labeled diagrams in markdown or compatible syntax
      - Flag inconsistencies or missing connections

  vinci_extensions:
    - Store diagrams in Vinci trace layer for reuse and collaboration
    - Use visuals to drive stakeholder walkthroughs and decision sessions
    - Link each node and line to model objects and phase deliverables
    - Trigger re-validation when model logic or roles change

technique:
  id: T060
  name: Walkthroughs

  purpose: >
    Walkthroughs are structured reviews where stakeholders step through a process, model, document, or design
    in a guided format to validate understanding, uncover gaps, and align interpretations.

  common_use_cases:
    - Validating complex documents or deliverables with diverse stakeholders
    - Revealing hidden assumptions, errors, or misalignments
    - Supporting onboarding, collaboration, or decision preparation

  procedural_steps:
    - Step 1: Define what will be reviewed and the outcome goals.
    - Step 2: Select the audience and assign facilitator and note-taker.
    - Step 3: Prepare materials and step-by-step walkthrough script.
    - Step 4: Conduct the walkthrough, pausing for questions, flags, or clarifications.
    - Step 5: Document findings, feedback, and decisions; adjust artifact as needed.

  metadata_fields:
    - walkthrough_topic (Step 1): Name and scope of what is reviewed.
    - participants (Step 2): Stakeholders by role, interest, or authority.
    - key_findings (Step 4): Issues surfaced or questions raised.
    - decision_points (Step 4): Items requiring action or sign-off.
    - outcome_status (Step 5): Approved, flagged, updated, or deferred.
    - trace_links (Step 5): Where the walkthrough links to models, requirements, or deliverables.

  inputs_required:
    - Prepared model, document, or output for review
    - Stakeholder invitation list
    - Optional facilitation tools or scripts

  outputs_produced:
    - Walkthrough Summary
    - Feedback and Decision Log
    - Revised Artifact (if needed)

  template_guidance:
    usage: >
      Use walkthrough templates to track structured input, questions, and consensus as you guide stakeholders
      through analysis or design outputs.
    required_templates:
      - Walkthrough Session Guide
      - Feedback Capture Table
    structure_notes:
      - Fields: Topic | Page/Step | Comment | Type | Owner | Action | Status
      - Visuals: annotated outputs, facilitation checklists
    vinci_automation:
      - GPT can generate walkthrough prompts and guide scripts
      - Auto-summarize discussion notes into issues or action items
      - Highlight patterns across multiple walkthroughs

  vinci_extensions:
    - Link walkthroughs to validation and governance trace logic
    - Store outputs in ontology for reuse, training, or audit
    - Track stakeholder involvement and response time
    - Trigger phase transitions or approvals based on walkthrough status

technique:
  id: T061
  name: Workshops

  purpose: >
    Workshops are facilitated group sessions designed to co-create, explore, validate, or decide on key elements
    of a project, model, or strategy. They accelerate alignment, surface diverse input, and support consensus-building.

  common_use_cases:
    - Eliciting requirements, risks, or opportunities across roles
    - Co-developing designs, maps, or solutions
    - Validating assumptions or reviewing deliverables in real time

  procedural_steps:
    - Step 1: Define workshop goal, scope, and success criteria.
    - Step 2: Identify and invite participants with relevant roles or perspectives.
    - Step 3: Design the workshop structure, activities, and materials.
    - Step 4: Facilitate the session, ensuring inclusive participation and active capture of insights.
    - Step 5: Document outputs, decisions, and follow-up actions.

  metadata_fields:
    - workshop_topic (Step 1): Theme or target outcome of the session.
    - participant_list (Step 2): Roles and functions invited.
    - activity_structure (Step 3): Agenda, methods (e.g., games, mapping, voting).
    - captured_insights (Step 4): Key feedback, ideas, or signals.
    - decision_log (Step 5): Resolutions or endorsed changes.
    - action_items (Step 5): Follow-up owners, tasks, and due dates.

  inputs_required:
    - Stakeholder map and session objective
    - Venue, platform, or facilitation toolkit
    - Draft materials or questions (if relevant)

  outputs_produced:
    - Workshop Summary or Minutes
    - Annotated Artifacts or Maps
    - Action Tracker and Trace Log

  template_guidance:
    usage: >
      Use workshop templates to guide design, facilitation, and output structuring.
      Helps ensure consistent capture and traceability of co-created content.
    required_templates:
      - Workshop Plan and Agenda Template
      - Insight Capture and Action Tracker
    structure_notes:
      - Fields: Topic | Activity | Insight | Owner | Action | Due | Trace Link
      - Visuals: boards, post-its, mind maps, journey maps
    vinci_automation:
      - GPT can design workshops based on goals, roles, and desired outputs
      - Auto-summarize insights and decisions into traceable formats
      - Suggest follow-ups and linked tasks by role or Pillar

  vinci_extensions:
    - Store workshop logs in Vinci trace layer and stakeholder records
    - Feed co-created models directly into initiative or capability design
    - Track patterns across sessions (e.g., role conflicts, recurring themes)
    - Enable retrospective linkage to impact and continuous improvement

