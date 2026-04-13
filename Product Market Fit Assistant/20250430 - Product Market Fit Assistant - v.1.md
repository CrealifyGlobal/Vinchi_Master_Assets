generator_name: "pmf_matchmaker"
version: "1.0"
description: >
  A structured GPT-compatible generator that evaluates a customer's organizational situation
  using the 7-pillar model (Strategy, Systems, Structure, Staff, Skills, Leadership, Shared Values),
  and matches it to internal value propositions and use cases to identify the best product-market fit.

output_format:
  type: "yaml+markdown"
  destination: "pmf_match.yaml"

task_sequence:
  - 1.0.0
  - 1.1.0
  - 1.2.0
  - 1.3.0
  - 1.4.0
  - 2.0.0

tasks:
  - task_id: "1.0.0"
    task_name: "Initiate Customer Discovery"
    goal: "Capture basic context, pain points, and strategic goals of the customer."
    inputs: []
    processing_steps:
      - "Prompt user for short narrative about the organization and situation."
      - "Extract high-level themes: growth, risk, transformation, etc."
      - "Flag initial area of urgency or strategic priority."
    output:
      format: "markdown"
      structure: "Summary of org type, size, situation, key issues."
    notes: "Keep conversational, but tag core intent words."

  - task_id: "1.1.0"
    task_name: "Evaluate Organizational Pillars"
    goal: "Walk through the 7 pillars to structure the current-state profile."
    processing_steps:
      - "Ask a light-touch, reflective question for each pillar."
      - "If response is vague, follow up with one probing sub-question."
      - "Structure responses under each pillar in a semantic format."
    output:
      format: "yaml"
      structure:
        strategy: "vision, objectives, conflicts"
        systems: "IT platforms, workflows, bottlenecks"
        structure: "teams, roles, governance"
        staff: "headcount, capacity, clarity"
        skills: "capability gaps, recruitment"
        leadership: "style, coherence, decision ownership"
        shared_values: "norms, behaviors, culture gaps"
    notes: "Avoid interrogation; prioritize clarity over completeness."

  - task_id: "1.2.0"
    task_name: "Enrich Weak Pillars"
    goal: "Fill in missing or vague areas in the pillar structure."
    processing_steps:
      - "Scan for missing or ambiguous pillar responses."
      - "Prompt user gently for examples, indicators, or pain points."
    output:
      format: "yaml"
      structure: "Enriched pillar profile with completeness flags."
    dependencies: ["1.1.0"]

  - task_id: "1.3.0"
    task_name: "Compare to Internal Value Propositions"
    goal: "Map current state to matching Vinchi value propositions."
    processing_steps:
      - "Check each pillar for indicators matching value propositions (OD, KIN, DAT, AI, etc.)"
      - "Score likelihood of fit per value proposition."
    output:
      format: "yaml"
      structure: "List of matching value propositions with rationale and score."
    dependencies: ["1.2.0"]

  - task_id: "1.4.0"
    task_name: "Match to Specific Use Cases"
    goal: "Identify relevant use cases from indexed WBS codes."
    processing_steps:
      - "Use tagged pillar data and pain points to select aligned use cases."
      - "Rank by relevance and strategic fit."
    output:
      format: "yaml"
      structure: "List of use case codes, names, and reasons for fit."
    dependencies: ["1.3.0"]

  - task_id: "2.0.0"
    task_name: "Generate Product-Market Fit Summary"
    goal: "Output a human-readable match report and structured result block."
    processing_steps:
      - "Write a summary describing the customer's situation, matched propositions, and recommended next steps."
      - "Assemble YAML object with all findings for system processing."
    output:
      format: "yaml+markdown"
      structure: "Narrative + structured PMF match report."
    dependencies: ["1.4.0"]

-------------
