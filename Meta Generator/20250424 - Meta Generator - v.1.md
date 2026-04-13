generator_name: "Generator Builder"
version: "1.0"
description: >
  This generator transforms a detailed process description (e.g., PDF, markdown, or structured text) 
  into a task-based, machine-readable generator YAML file suitable for use by humans and language models.

required_inputs:
  - name: "process_document"
    description: "The full process or methodology to be translated into tasks."
    format: "markdown | pdf | plaintext"
    source: "manual"
  - name: "configuration_profile"
    description: "Optional config for naming, structure, or output constraints."
    format: "yaml"
    source: "optional"

task_sequence:
  - 1.1.0
  - 1.2.0
  - 1.3.0
  - 1.4.0
  - 2.0.0

output_format:
  type: "yaml"
  destination: "outputs/generated_generator.yaml"

tasks:

  - task_id: "1.1.0"
    task_name: "Extract Candidate Tasks"
    goal: "Identify and segment discrete activities, decisions, or operations from the source process document."
    context: "First step in converting unstructured content into a task structure."
    inputs:
      - name: "process_document"
        description: "Full content of the methodology or process."
        format: "markdown | plaintext"
        source: "manual"
    resources: []
    processing_steps:
      - "Read the process document sequentially."
      - "Detect and extract activity blocks using headings, steps, or bullet structure."
      - "Identify logical task boundaries based on verbs, deliverables, or transitions."
      - "Assign a temporary task ID for each detected task (e.g., TEMP-001)."
    output:
      format: "yaml"
      structure:
        description: "List of candidate task titles, purposes, and reference locations."
    validation:
      - "No empty tasks."
      - "All tasks must be clearly described with intent and scope."
    notes:
      - "Use NLP tagging for verbs and objects to improve boundary detection."

  - task_id: "1.2.0"
    task_name: "Convert Candidates to Formal Tasks"
    goal: "Convert each extracted task into a formally structured YAML block."
    context: "Transforms raw activities into precise specifications."
    inputs:
      - name: "candidate_tasks"
        description: "Task list from Task 1.1.0."
        format: "yaml"
        source: "calculated"
    resources: []
    processing_steps:
      - "Assign a final task_id based on process phase (e.g., 1.2.1, 2.1.0)."
      - "Define a task_name for each."
      - "Extract or infer a goal and context from document section."
      - "Assign placeholder inputs, resources, and outputs to be refined later."
    output:
      format: "yaml"
      structure:
        description: "List of structured task blocks with IDs, names, and context."
    validation:
      - "All task_ids must be unique and sequentially correct."
      - "No empty fields for task_name or goal."
    dependencies:
      - task_id: "1.1.0"
    notes:
      - "Where unclear, flag tasks for human review using status: needs_review."

  - task_id: "1.3.0"
    task_name: "Populate Input/Output Structures"
    goal: "Identify for each task the inputs it uses, the resources it references, and the outputs it produces."
    context: "Enhances task definitions with operational data."
    inputs:
      - name: "structured_tasks"
        description: "Partially complete task definitions from 1.2.0."
        format: "yaml"
        source: "calculated"
      - name: "process_document"
        description: "Original source for detail matching."
        format: "markdown"
        source: "manual"
    resources: []
    processing_steps:
      - "For each task, scan related paragraph(s) in the source document."
      - "List any required datasets, reports, or config files as inputs."
      - "Identify formulas, templates, or lookup tables as resources."
      - "Define output structure (table, markdown, json, etc.) with example field names."
    output:
      format: "yaml"
      structure:
        description: "Complete task blocks with IO definitions and formats."
    validation:
      - "All outputs must have a format and schema description."
      - "All inputs must have a source and data type."
    dependencies:
      - task_id: "1.2.0"
    notes:
      - "For any undocumented data type, insert `format: unknown` and flag."

  - task_id: "1.4.0"
    task_name: "Link Task Dependencies"
    goal: "Map out dependency relationships and order of execution."
    context: "Ensures tasks are executed in logical sequence with all prerequisites satisfied."
    inputs:
      - name: "task_definitions"
        description: "Task blocks with partial structure from 1.3.0."
        format: "yaml"
        source: "calculated"
    resources: []
    processing_steps:
      - "Analyze each task's input to identify upstream task outputs."
      - "For each identified link, insert into `dependencies` list."
      - "Resolve circular dependencies or flag for manual ordering."
    output:
      format: "yaml"
      structure:
        description: "Tasks now contain `dependencies` with proper execution flow."
    validation:
      - "No task may have unresolved or circular dependencies."
    dependencies:
      - task_id: "1.3.0"
    notes:
      - "If two tasks could be executed in parallel, mark with `parallel_group`."

  - task_id: "2.0.0"
    task_name: "Compile Final Generator YAML"
    goal: "Aggregate all task blocks into a final generator file with top-level metadata."
    context: "Completes the transformation into a deployable instruction file."
    inputs:
      - name: "final_tasks"
        description: "Fully defined task blocks from Tasks 1.1–1.4."
        format: "yaml"
        source: "calculated"
    resources: []
    processing_steps:
      - "Insert header metadata (generator_name, version, description)."
      - "Assemble task_sequence using task_ids in correct order."
      - "Validate YAML structure and indenting."
      - "Write to `outputs/generated_generator.yaml`."
    output:
      format: "yaml"
      structure:
        description: "Executable generator YAML ready for downstream interpretation."
    validation:
      - "YAML must pass schema validation and be executable in interpreter."
    dependencies:
      - task_id: "1.4.0"
    notes:
      - "Optionally emit markdown or HTML preview of generator structure."
