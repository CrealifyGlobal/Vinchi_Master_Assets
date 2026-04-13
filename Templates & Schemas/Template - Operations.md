operation:
  identity:
    operation_id: project.create
    name: Create Project
    short_description: Create a new project record
    domain: project_management
    status: active
    version: 1.0.0

  purpose:
    purpose: Create a new project within the platform
    business_outcome: Enables downstream planning, ownership assignment, and reporting
    execution_scope: composite
    scope_notes: Creates the project and initializes baseline structures

  classification:
    operation_type: command
    interaction_pattern: request_response
    state_effect: creates_records
    idempotency: false
    synchronous_or_asynchronous: synchronous

  relationships:
    relates_to_capabilities:
      - delivery management
    relates_to_processes:
      - project initiation
    relates_to_activities:
      - capture project metadata
      - assign project owner
    relates_to_entities:
      - project
      - user
    relates_to_policies: []
    relates_to_events: []
    relationship_notes: This operation supports project setup but is not subordinate to the business hierarchy

  conditions:
    preconditions:
      - user must have permission to create projects
      - project name must be unique
    postconditions:
      - project record exists
      - project owner is assigned
      - baseline metadata is initialized

  inputs:
    - name: project_name
      type: string
      required: true
      default: null
      validation:
        min_length: 3
        max_length: 120
      semantic_meaning: Human-readable name of the project
      example_value: Harbor Upgrade 2026

    - name: owner_id
      type: string
      required: true
      default: null
      validation: {}
      semantic_meaning: Unique identifier of the responsible owner
      example_value: usr_001

  outputs:
    - name: project_id
      type: string
      semantic_meaning: Unique identifier of the created project
      example_value: prj_001

    - name: status
      type: string
      semantic_meaning: Resulting status of the created project
      example_value: initialized

  output_behavior:
    result_type: success_payload
    human_readable_summary: true
    machine_readable_payload: true
    supported_output_modes:
      - human
      - json

  exposure:
    exposed_in:
      - gui
      - cli
      - agent
      - api

    cli_command:
      root: app
      path:
        - project
        - create
      command_notes: Standard command path for human terminal interaction

    agent_tool_name: project.create

    api_mapping:
      method: POST
      path: /projects
      api_notes: Creates a new project resource

    ui_labels:
      primary_button: Create Project
      menu_action: New Project
      ui_notes: Available from project administration screens

  governance:
    risk_level: medium
    destructive: false
    requires_confirmation: false
    requires_approval: false
    audit_required: true
    permissions_required:
      - project.create
    governance_notes: State-changing operation requiring permission and audit logging

  composition:
    composed_of:
      - project.validate_input
      - project.create_record
      - project.assign_owner
      - project.initialize_defaults
    calls_operations: []
    can_be_called_by:
      - project.initialize
    orchestration_notes: Can be used directly or as part of larger orchestration flows

  execution_guidance:
    execution_steps:
      - validate required inputs
      - create the project record
      - assign the owner
      - initialize baseline metadata
      - return identifiers and status
    error_conditions:
      - duplicate_project_name
      - missing_required_input
      - permission_denied
    recovery_notes: Failed initialization should return a structured error and avoid partial record creation where possible

  authoring_notes:
    assumptions:
      - project setup rules are already configured in the platform
    constraints:
      - required baseline fields must be available
    notes: This is a reusable command operation exposed across all major interaction modes