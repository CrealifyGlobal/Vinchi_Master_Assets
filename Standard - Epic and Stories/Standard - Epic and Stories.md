standard_name: Epic and Story Requirements Standard
standard_version: 1.0.0
standard_purpose: Define the canonical structure for organizing user requirements into epics and user stories in a way that is human-readable, machine-readable, and suitable for later translation into executable operations, workflows, or solution components.

governing_concepts:
  epic: A high-level organizational container used to group related user stories around a broader objective, theme, problem space, or outcome area.
  user_story: A structured representation of a user-centered requirement describing a desired outcome from the perspective of a role, actor, or stakeholder.
  epic_story_relationship: A many-to-one relationship in which multiple user stories may belong to a single epic.
  requirements_layer: A conceptual layer focused on capturing and organizing intent, needs, and expected outcomes, separate from execution, authorization, and runtime behavior.

design_principles:
  - Epics and user stories belong to the requirements layer, not the execution layer.
  - Epics are grouping objects, not executable objects.
  - User stories express intent and desired outcomes, not implementation details.
  - User stories should remain independent from access control and authorization logic.
  - User stories should be detailed enough to support later mapping to operations, workflows, interfaces, or backlog items.
  - The standard must support both human interpretation and machine-assisted reasoning.
  - Epics do not need to align rigidly to capabilities, business domains, or system modules.
  - The structure should allow flexibility in grouping while preserving traceability.

top_level_objects:
  - epic_registry
  - user_story_registry
  - relationship_rules
  - classification_guidance
  - authoring_guidance

epic_registry:
  object_name: epics
  object_purpose: Store and organize epics as high-level requirement containers.
  required_fields:
    - epic_id
    - name
    - description
    - status

  optional_fields:
    - summary
    - objective
    - problem_statement
    - desired_outcomes
    - related_domains
    - related_capabilities
    - related_entities
    - priority
    - owner_role
    - tags
    - notes

  epic_template:
    epic_id: epic.example_requirement_area
    name: Example Requirement Area
    summary: High-level grouping of related user needs.
    description: >
      This epic groups related user stories that together represent
      a broader objective, problem space, or thematic requirement area.
    objective: Define the broader purpose served by the epic.
    problem_statement: Describe the underlying problem or need being addressed.
    desired_outcomes:
      - outcome one
      - outcome two
    related_domains:
      - example_domain
    related_capabilities:
      - example_capability
    related_entities:
      - example_entity
    status: draft
    priority: medium
    owner_role: product_owner
    tags:
      - example
    notes: Additional contextual notes may be recorded here.

user_story_registry:
  object_name: user_stories
  object_purpose: Store structured user stories representing specific user-centered requirements.
  required_fields:
    - story_id
    - epic_id
    - role
    - narrative
    - desired_outcome
    - status

  optional_fields:
    - short_name
    - actor_type
    - context
    - trigger
    - related_entities
    - related_domains
    - related_capabilities
    - intent_type
    - priority
    - acceptance_criteria
    - assumptions
    - constraints
    - dependencies
    - notes
    - candidate_operations
    - mapping_notes
    - source_reference

  story_template:
    story_id: story.example_do_something
    epic_id: epic.example_requirement_area
    short_name: Do Something Important
    role: example_user
    actor_type: human
    narrative: >
      As an example user, I want to perform an example action
      so that I can achieve an example outcome.
    context: Describe the business or usage context in which this story applies.
    trigger: Describe what causes or initiates this requirement or need.
    desired_outcome: example_outcome_achieved
    related_entities:
      - example_entity
    related_domains:
      - example_domain
    related_capabilities:
      - example_capability
    intent_type: create
    status: draft
    priority: medium
    acceptance_criteria:
      - criterion one
      - criterion two
    assumptions:
      - assumption one
    constraints:
      - constraint one
    dependencies:
      - story.example_dependency
    notes: Additional story notes may be recorded here.
    candidate_operations: []
    mapping_notes: Optional future mapping notes to operations or workflows.
    source_reference: interview_001

relationship_rules:
  epic_to_story:
    definition: A single epic may contain multiple user stories.
    cardinality: one_to_many
    rule: Every user story must reference exactly one epic_id.
    note: Epics may exist before stories are fully defined, but stories should not exist without an associated epic in the registry.

  story_to_epic:
    definition: Each user story belongs to one epic for organizational traceability.
    cardinality: many_to_one

  story_to_story:
    definition: User stories may reference dependencies on other stories.
    cardinality: many_to_many
    note: Dependencies do not change epic ownership unless explicitly restructured.

classification_guidance:
  epic_definition_guidance:
    - Use an epic to group related stories around a broader objective, theme, or problem area.
    - Do not use an epic as an executable object.
    - Do not force epics to mirror technical modules unless that grouping is genuinely useful.
    - Keep epic descriptions broad enough to contain multiple stories but specific enough to be meaningful.

  story_definition_guidance:
    - Use a user story to describe a specific user-centered need or intended outcome.
    - Keep the narrative focused on who wants what and why.
    - Do not embed technical implementation details directly in the story narrative.
    - Do not embed authorization logic directly in the story definition.
    - Use acceptance criteria to clarify expected success conditions.
    - Use intent_type to support future mapping and automation.

  intent_type_definitions:
    allowed_values:
      - create
      - read
      - update
      - delete
      - assign
      - approve
      - search
      - compare
      - calculate
      - monitor
      - control
      - orchestrate
      - configure
      - communicate
      - decide
      - other
    note: intent_type is a requirements-side classification of user intent, not an execution-side operation type.

authoring_guidance:
  epic_authoring_rules:
    - Write epic names as concise labels for the requirement area.
    - Write epic descriptions in natural language.
    - Use objective and problem_statement to ground the epic in real need.
    - Use desired_outcomes to clarify intended value.

  story_authoring_rules:
    - Prefer the standard narrative form where appropriate.
    - Narrative pattern: As a [role], I want to [need], so that [outcome].
    - Where needed, supplement the narrative with context, trigger, constraints, and assumptions.
    - Write desired_outcome as the outcome to be achieved, not the solution to be built.
    - Write acceptance criteria as observable success conditions.
    - Keep candidate_operations optional so that requirements remain separate from execution.

status_model:
  epic_statuses:
    allowed_values:
      - draft
      - proposed
      - active
      - refined
      - approved
      - deprecated
      - retired

  story_statuses:
    allowed_values:
      - draft
      - proposed
      - active
      - refined
      - approved
      - in_progress
      - completed
      - deprecated
      - retired

traceability_guidance:
  purpose: Preserve the ability to connect requirements to downstream design, execution, and validation artifacts without collapsing them into one model.
  traceable_links:
    - epic to user_story
    - user_story to acceptance_criteria
    - user_story to candidate_operations
    - user_story to workflows
    - user_story to test_cases
    - user_story to solution_components
  rule: Traceability links may be added, but the requirements object remains distinct from the execution object.

recommended_terms:
  standard: Epic and Story Requirements Standard
  top_container: epic
  requirement_unit: user_story
  relationship: epic_story_relationship
  optional_mapping_field: candidate_operations

example_registry:
  epics:
    - epic_id: epic.project_setup
      name: Project Setup
      summary: Organize requirements related to establishing a new project.
      description: >
        This epic groups the user stories required to initiate and structure
        a new project so that planning and delivery can begin.
      objective: Enable users to establish a valid new project structure.
      problem_statement: Users need a consistent way to initialize projects.
      desired_outcomes:
        - projects can be initiated consistently
        - baseline project information is captured
      related_domains:
        - project_management
      related_capabilities:
        - delivery_management
      related_entities:
        - project
      status: active
      priority: high
      owner_role: product_owner
      tags:
        - onboarding

  user_stories:
    - story_id: story.project_create
      epic_id: epic.project_setup
      short_name: Create Project
      role: project_manager
      actor_type: human
      narrative: >
        As a project manager, I want to create a new project
        so that I can start planning and tracking work.
      context: Applies when a new initiative is formally started.
      trigger: A new project request is approved.
      desired_outcome: new_project_established
      related_entities:
        - project
      related_domains:
        - project_management
      related_capabilities:
        - delivery_management
      intent_type: create
      status: active
      priority: high
      acceptance_criteria:
        - a project can be created with required baseline fields
        - the created project is available for downstream planning
      assumptions:
        - the initiating user has the right business context
      constraints:
        - required fields must be completed
      dependencies: []
      notes: Initial creation only, not full initialization.
      candidate_operations:
        - project.create
      mapping_notes: May later map to a simple creation operation.
      source_reference: workshop_01