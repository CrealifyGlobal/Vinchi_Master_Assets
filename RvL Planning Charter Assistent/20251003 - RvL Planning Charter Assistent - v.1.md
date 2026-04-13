


#############################################
Innitiator


#############################################

schema_version: 0.1
schema_title: Project Planning Charter (Executable YAML Schema)
schema_description: >
  A descriptive, extraction-ready schema for creating a Project Planning Charter
  for a superyacht (or complex shipbuilding) program. Designed for LLMs to
  populate from unstructured content (meeting notes, specs, contracts,
  schedules). Each section defines explicit fields, guidance, acceptance
  criteria, quality checks, and cross-references. The schema aims to keep the
  planning strategy stable while allowing schedules and scenarios to change.

# -----------------------------
# GLOBAL CONFIG & ENUMS
# -----------------------------
config:
  language: en
  timezone: Europe/Amsterdam
  date_format: YYYY-MM-DD
  number_format: 1,234.56
  id_style: kebab-case
  allow_free_text: true

instructions_for_ai:
  summary: >
    Extract and normalize planning-relevant facts from provided sources.
    Prefer explicit statements over assumptions. If uncertain, mark the field
    with `confidence: low` and add a `notes` entry in `audit_trail`.
  priority_rules:
    - Phase and location codes MUST remain consistent once set.
    - Do not modify the logic or rules; only populate data.
    - If conflicting data appear, prefer the most recent signed baseline.
  conflict_resolution:
    order_of_precedence: [contract, signed-variation, approved-baseline, latest-approved-schedule, engineering-release, meeting-minutes]
    action_on_conflict: annotate
  output_style:
    keep_placeholders_for_missing: true
    include_audit_trail: true

globals:
  enums:
    phase_names:
      - 0-commercial-engagement
      - 1-design-and-engineering
      - 2-hull-and-structure
      - 3-pre-outfitting
      - 4-zone-outfitting
      - 5-integration-and-systems
      - 6-commissioning
      - 7-sea-trials-and-acceptance
      - 8-delivery-and-handover
    planning_levels:
      - program-master
      - area/location
      - system/trade
      - short-interval/weekly
      - day/shift
    role_types:
      - overall-program-planner
      - operational-planner
      - location-planner
      - engineering-coordinator
      - construction-manager
      - logistics-lead
      - subcontractor-coordinator
      - quality-inspector
      - hse-lead
      - pm
      - client-rep
    risk_categories:
      - schedule
      - design/engineering
      - resource/congestion
      - logistics/materials
      - subcontractor/interface
      - location/space
      - quality/rework
      - safety
      - regulatory/class
      - environment/weather
    change_types:
      - scope-change
      - late-engineering
      - resequencing
      - acceleration/deceleration
      - substitution
      - deviation/waiver
    location_code_scheme:
      pattern: >
        <BlockCode>-<DeckCode>-<ZoneCode>-<SpaceCode> (e.g., M-L-02-D03)
      description: >
        Hierarchical location coding used across phases and trades. Must be
        immutable once baselined.

quality_checks:
  - id: q1-no-empty-required
    description: Required fields may not be empty after initial population.
  - id: q2-code-consistency
    description: Location codes must match the configured scheme and remain stable.
  - id: q3-phase-sequence
    description: Phases must be ordered per `globals.enums.phase_names`.
  - id: q4-role-mapping
    description: All roles must map to `globals.enums.role_types` or provide a mapping.
  - id: q5-traceability
    description: Each populated field should have at least one source in `audit_trail`.

prompts:
  system_template: >
    You are an extraction and normalization engine. Populate the Project
    Planning Charter YAML from unstructured inputs. Do not invent details.
    Keep the strategy stable; capture uncertainties in `audit_trail`.
  section_prompt_template: >
    Populate the "{{section.name}}" section. Use the field definitions,
    acceptance_criteria, and cross_refs. For each field, extract value(s),
    set `confidence` (high/medium/low), and add `sources`.

# -----------------------------
# SECTIONS
# -----------------------------
sections:
  - id: purpose-and-scope
    name: Purpose & Scope
    purpose: Define why this charter exists and what it covers (and does not).
    fields:
      purpose_statement:
        type: text
        required: true
        guidance: One paragraph that states the intent of this charter.
      scope_includes:
        type: list[text]
        required: true
        guidance: Bulleted list of inclusions (phases, planning practices, roles, interfaces).
      scope_excludes:
        type: list[text]
        required: false
        guidance: Bulleted list of exclusions to avoid scope creep.
      assumptions:
        type: list[text]
        required: false
        guidance: Assumptions underpinning this charter.
      constraints:
        type: list[text]
        required: false
        guidance: Hard limits (legal, spatial, environmental, client-imposed).
    acceptance_criteria:
      - Intent is unambiguous and actionable.
      - Inclusions reference recognized phases and planning levels.
    cross_refs: [project-context-objectives, phases-of-construction, grc]

  - id: project-context-objectives
    name: Project Context & Objectives
    purpose: Capture top-level context, objectives, and milestones.
    fields:
      project_overview:
        type: text
        required: true
        guidance: Short description of vessel type, size, and unique constraints.
      key_milestones:
        type: list[object]
        required: true
        item_shape:
          code: string
          name: string
          target_date: date
          description: string
        guidance: Contract signing to delivery; include phase gates and baselines.
      objectives:
        type: list[text]
        required: true
        guidance: SMART-style objectives (time, quality, cost, safety, risk posture).
      success_criteria:
        type: list[text]
        required: true
        guidance: Observable conditions for success (e.g., zero hot work clashes on X decks).
    acceptance_criteria:
      - Milestones are dated and ordered.
      - Success criteria are measurable.
    cross_refs: [phases-of-construction, grc]

  - id: phases-of-construction
    name: Phases of Construction
    purpose: Define the phase model and what planning evolves or locks at each phase.
    fields:
      phase_model:
        type: list[object]
        required: true
        item_shape:
          id: enum(globals.enums.phase_names)
          name: string
          description: string
          entry_criteria: list[text]
          exit_criteria: list[text]
          frozen_elements: list[text]
          unlocked_elements: list[text]
          primary_outputs: list[text]
        guidance: Align to globals.enums.phase_names; describe gate criteria and freezes.
      phase_location_matrix:
        type: table
        required: false
        guidance: Rows=phases, Cols=location levels/zones; mark active work areas.
    acceptance_criteria:
      - Phases align to global order.
      - Entry/exit criteria are testable.
    cross_refs: [location-and-practices, methodologies-and-prioritization, grc]

  - id: location-and-practices
    name: Location & Practices
    purpose: Establish location codes, zoning, and location-based planning rules.
    fields:
      location_code_policy:
        type: object
        required: true
        properties:
          scheme_description: string
          code_pattern: string
          examples: list[string]
          immutability_rule: string
      locations_catalog:
        type: list[object]
        required: true
        item_shape:
          code: string
          name: string
          type: enum([block, deck, zone, space, workshop, dock, yard-area, vendor-site])
          parent: string|null
          description: string
      location_rules:
        type: list[text]
        required: true
        guidance: Density limits, trade stacking rules, hot/cold work separations.
    acceptance_criteria:
      - Codes match pattern and hierarchy is valid.
      - Rules cover safety, quality, and productivity constraints.
    cross_refs: [resource-and-location-management, methodologies-and-prioritization, grc]

  - id: methodologies-and-prioritization
    name: Methodologies & Prioritization Rules
    purpose: Declare the planning logic and prioritization algorithms.
    fields:
      planning_method:
        type: enum([location-based, system-based, hybrid])
        required: true
      sequencing_logic:
        type: list[text]
        required: true
        guidance: E.g., progressive outfitting, block-first, outfit-first, pull vs push.
      prioritization_criteria:
        type: list[text]
        required: true
        guidance: Critical path, outfitting readiness, stability/weight, vendor lead times.
      standards_referenced:
        type: list[text]
        required: false
        guidance: Yard standards, class requirements, internal SOPs.
    acceptance_criteria:
      - Logic is coherent with phases and locations.
      - Criteria are rankable and can be computed.
    cross_refs: [phases-of-construction, planning-levels-and-roles, grc]

  - id: planning-levels-and-roles
    name: Planning Levels & Roles
    purpose: Define who plans what, at what horizon, and with what authority.
    fields:
      levels:
        type: list[object]
        required: true
        item_shape:
          level: enum(globals.enums.planning_levels)
          horizon: string
          cadence: string
          planning_outputs: list[string]
      roles:
        type: list[object]
        required: true
        item_shape:
          role: enum(globals.enums.role_types)
          responsibilities: list[string]
          authority_limits: string
          rasi: object
          # RASI = Responsible, Accountable, Support, Informed
          # Keys: responsible, accountable, support, informed (list of role enums)
      decision_rights:
        type: list[text]
        required: true
        guidance: Who may re-sequence, approve changes, commit resources.
    acceptance_criteria:
      - Each level has clear outputs and cadence.
      - Roles have non-overlapping accountabilities.
    cross_refs: [grc, subcontractor-integration]

  - id: subcontractor-integration
    name: Subcontractor Integration
    purpose: Define how vendor/subcontractor deliverables are planned and controlled.
    fields:
      vendor_strategy:
        type: text
        required: true
        guidance: Embedded vs offsite, modularization, interface control.
      deliverable_register:
        type: list[object]
        required: true
        item_shape:
          id: string
          name: string
          supplier: string
          due: date
          interface_points: list[string]
          acceptance_criteria: list[string]
      interface_management:
        type: list[text]
        required: true
        guidance: Handover documents, inspections, data exchange, escalation path.
    acceptance_criteria:
      - Each deliverable has dates and acceptance tests.
      - Interfaces name locations and roles.
    cross_refs: [resource-and-location-management, grc]

  - id: resource-and-location-management
    name: Resource & Location Management
    purpose: Control workforce, shared facilities, logistics, and material flow.
    fields:
      workforce_allocation_rules:
        type: list[text]
        required: true
        guidance: Max crew per zone, concurrency limits, night-shift rules.
      shared_facilities:
        type: list[object]
        required: true
        item_shape:
          name: string
          type: enum([crane, dock, workshop, laydown, warehouse, tool, test-stand])
          capacity: string
          booking_rules: string
      logistics_practices:
        type: list[text]
        required: true
        guidance: Material kitting, JIT windows, staging areas, line-of-balance ties.
    acceptance_criteria:
      - Rules prevent clashes and congestion.
      - Facilities have clear booking logic.
    cross_refs: [location-and-practices, methodologies-and-prioritization, grc]

  - id: best-practices-and-guiding-principles
    name: Best Practices & Guiding Principles
    purpose: Codify stable do’s/don’ts and learning loops.
    fields:
      golden_rules:
        type: list[text]
        required: true
        guidance: Immutable principles for planners and supervisors.
      lessons_learned_process:
        type: text
        required: true
        guidance: How learnings are captured and updated into this charter.
      prohibited_patterns:
        type: list[text]
        required: false
        guidance: Known anti-patterns (e.g., unbounded resequencing, overstacking trades).
    acceptance_criteria:
      - Golden rules tie back to risk and change control.
    cross_refs: [grc]

  - id: grc
    name: Governance, Risk & Controls (GRC)
    purpose: Provide the umbrella for decision-making, risk posture, and change control.
    fields:
      governance:
        type: object
        required: true
        properties:
          decision_checkpoints: list[string]
          reporting_cadence: string
          steering_bodies: list[string]
          approvals_required: list[string]
      risk_management:
        type: object
        required: true
        properties:
          risk_register:
            type: list[object]
            item_shape:
              id: string
              title: string
              category: enum(globals.enums.risk_categories)
              description: string
              cause: string
              impact: string
              probability: enum([low, medium, high])
              severity: enum([low, medium, high])
              owner_role: enum(globals.enums.role_types)
              mitigation: string
              contingency: string
              status: enum([open, monitoring, mitigated, closed])
              review_cadence: string
          risk_appetite_statement: string
          thresholds_and_triggers: list[string]
      controls:
        type: object
        required: true
        properties:
          change_management:
            type: object
            properties:
              types_allowed: list[enum(globals.enums.change_types)]
              change_board: list[enum(globals.enums.role_types)]
              approval_flow: list[string]
              documentation_required: list[string]
              resequencing_authority: list[enum(globals.enums.role_types)]
          scenario_planning:
            type: list[object]
            item_shape:
              scenario_id: string
              description: string
              trigger: string
              recovery_logic: list[string]
              buffers_used: list[string]
          buffers_and_contingency:
            type: list[object]
            item_shape:
              name: string
              type: enum([time, capacity, cost, material])
              allocation_rule: string
              release_conditions: string
    acceptance_criteria:
      - Risks are categorized, owned, and reviewed.
      - Change control defines who can approve and resequence.
      - Scenarios include triggers and clear recovery logic.
    cross_refs: [planning-levels-and-roles, methodologies-and-prioritization]

  - id: annexes
    name: Annexes
    purpose: Provide supporting references and definitions.
    fields:
      glossary:
        type: list[object]
        required: false
        item_shape:
          term: string
          definition: string
      examples_and_templates:
        type: list[object]
        required: false
        item_shape:
          name: string
          description: string
          link_or_path: string
      references:
        type: list[text]
        required: false
    acceptance_criteria:
      - Terms align with usage across the charter.
    cross_refs: []

# -----------------------------
# AUDIT & TRACEABILITY
# -----------------------------
audit_trail:
  sources: []  # LLM to populate: list of {id, type, title, date, snippet, link_or_path}
  notes: []    # LLM to populate: uncertainties, conflicts, rationale

# -----------------------------
# RENDERING HINTS (OPTIONAL)
# -----------------------------
rendering:
  toc: true
  show_section_ids: true
  collapse_annexes_by_default: true
