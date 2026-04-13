generator_name: "Use Case Generator"
version: "2.0"
description: >
  This generator produces structured, machine- and human-readable use cases from a variety of inputs:
  value propositions, resumes, project documentation, and structured interviews. Each use case is output
  in a standardized YAML format designed for reuse across Vinchi's delivery, learning, and automation pipelines.

required_inputs:
  - name: "input_type"
    description: "Type of source to be used (e.g., 'Value Proposition', 'Resume', 'Project Documentation', 'Structured Interview')."
    format: "string"
    source: "manual"

  - name: "value_propositions"
    description: "User-provided value propositions (text, upload, or structured definitions) that guide categorization and scenario framing."
    format: "text | file"
    source: "manual"

  - name: "existing_use_cases"
    description: "Previously generated use cases to evaluate against for duplication, gaps, and pattern consistency."
    format: "yaml | markdown | plaintext"
    source: "manual"

  - name: "input_content"
    description: "The main input source: resume, document, narrative, or SME response."
    format: "text | file | interactive"
    source: "manual"

task_sequence:
  - 1.0.0
  - 1.1.0
  - 1.2.0
  - 1.3.0
  - 2.0.0

tasks:

  - task_id: "1.0.0"
    task_name: "Interpret Provided Value Propositions"
    goal: "Transform value proposition input (freeform, document, or description) into actionable classification guidance."
    context: >
      This ensures the generator can match content to the correct value proposition logic, even if the user redefines or customizes them.
    inputs:
      - name: "value_propositions"
        format: "text | file"
    output:
      format: "text"
      structure: "Parsed value proposition themes, delivery mechanisms, and known outcome types."
    notes:
      - "Do not assume a fixed list — always derive classification logic from user input."

  - task_id: "1.1.0"
    task_name: "Evaluate Existing Use Cases for Overlap"
    goal: "Review user-provided use cases to prevent duplication, identify patterns, and detect gaps."
    context: >
      This step improves output relevance, enhances variety, and prevents repetitive use case creation.
    inputs:
      - name: "existing_use_cases"
        format: "yaml | markdown | text"
    output:
      format: "yaml"
      structure: >
        reuse_flags, novelty_opportunities, gap_areas, related_tags
    notes:
      - "Must be run before generating new content from any input type."

  - task_id: "1.2.0"
    task_name: "Extract Use Case Candidates from Input Content"
    goal: "Identify one or more scenario-based use case candidates from the selected input source."
    context: >
      Branches by input_type:
        - For resumes: parse contributions and achievements per role
        - For documents: scan for intervention, value, and capability signals
        - For interviews: begin with open prompt and use dynamic follow-up questions
        - For VPs: simulate typical application cases based on delivery logic
    inputs:
      - name: "input_type"
        format: "string"
      - name: "input_content"
        format: "text | file | interactive"
    output:
      format: "yaml"
      structure: >
        code (placeholder), scenario, value_created, value_proposition (tentative), source,
        tags, initial value_drivers, candidate risks, observed waste
    notes:
      - "For interviews, always start with: 'Tell me about a project or challenge you worked on that had meaningful results.'"
      - "For resumes, build one use case per key project or achievement."
      - "Always connect use cases to the provided value propositions."

  - task_id: "1.3.0"
    task_name: "Enrich Use Case Metadata"
    goal: "Add classification metadata, risks, wastes, and implementation notes to each partial use case."
    context: >
      Metadata enhances discoverability, analysis, filtering, and reuse across platforms and formats.
    inputs:
      - name: "partial_use_case"
        format: "yaml"
    output:
      format: "yaml"
      structure: >
        tags, value_drivers, client_risks, client_wastes, research_articles, notes
    notes:
      - "Tag value_drivers from Vinchi's 7 value types."
      - "Select wastes based on Lean/Kaizen categories."
      - "Notes can contain delivery format, insights, or contextual tips."

  - task_id: "2.0.0"
    task_name: "Generate Final Use Case YAML Block"
    goal: "Compile complete, finalized use case with all required fields for deployment or documentation."
    context: >
      This task standardizes the use case so it can be consumed by GPTs, dashboards, documentation systems, or implementation playbooks.
    inputs:
      - name: "value_proposition"
        format: "text"
      - name: "enriched_use_case"
        format: "yaml"
    output:
      format: "yaml"
      structure: >
        code, name, value_proposition, source, scenario, value_created, tags,
        value_drivers, client_risks, client_wastes, research_articles, notes
    notes:
      - "Assign next-in-sequence use case code using format: VP:[prefix]-[number]-v1.0"
      - "Ensure source is one of: Value Proposition, Resume, Project Documentation, Structured Interview"
      - "Validate YAML structure before output"
