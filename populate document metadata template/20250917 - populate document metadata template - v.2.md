tool_name: "Document Metadata Populator"
version: "1.1.0"
purpose: "Extract metadata and section structure from a source document (PDF/DOCX/MD/TXT) and populate the 'Document Metadata Sheet' with validated fields, a Section Register, and Section Details. Always begin by asking the user which source document to apply the tool to."
language: "en"

# -----------------------------
# Inputs
# -----------------------------
inputs:
  - name: "target_document"
    type: "file"
    required: true
    description: "The user-confirmed file (PDF, DOCX, MD, or TXT) to which the tool will be applied. Selected explicitly during pre-flight."
  - name: "source_format"
    type: "string"
    required: true
    description: "One of: PDF , DOCX, MD, TXT."
  - name: "document_hints"
    type: "object"
    required: false
    description: "Optional hints such as language, title, created_date, authors, classification."
  - name: "registry_params"
    type: "object"
    required: false
    description: "Optional registry settings, e.g. assign_registry_id, namespace."
  - name: "output_file_names"
    type: "object"
    required: false
    description: "Optional explicit filenames for metadata_markdown and metadata_json."

# -----------------------------
# Parameters
# -----------------------------
parameters:
  section_discovery_mode:
    type: "enum"
    default: "hybrid"
    allowed: ["heuristic", "headings", "regex_map", "hybrid"]
    description: "Strategy for building the Section Register and extracting Section Details."
  auto_classify:
    type: "boolean"
    default: true
    description: "Attempt to classify document type/visibility and propose tags/categories."
  redact_pii:
    type: "boolean"
    default: true
    description: "If true, snippets in Section Details will redact likely PII as [[REDACTED]]."
  timezone:
    type: "string"
    default: "environment"
    description: "Timezone for date parsing and output formatting (e.g., 'Europe/Amsterdam')."
  determinism_seed:
    type: "number"
    default: 42
    description: "Seed for deterministic ordering and reproducibility."
  lang_detect_enabled:
    type: "boolean"
    default: true
    description: "Enable automatic language detection of the target document."
  lang_detect_conf_min:
    type: "number"
    default: 0.6
    description: "Minimum confidence for language detection before overriding provided hints."

# -----------------------------
# Uniqueness Keys
# -----------------------------
uniqueness_keys:
  - "registry_id"
  - "source_file_hash"

# -----------------------------
# Checks
# -----------------------------
checks:
  - id: "mimetype_validation"
    fail_policy: "hard"
    description: "Reject unsupported file types."
  - id: "text_extractability"
    fail_policy: "warn"
    description: "Warn if extracted text length is very low; suggest OCR."
  - id: "self_spec_guard"
    fail_policy: "hard"
    description: "Reject if the selected file appears to be this tool specification."
  - id: "plausibility_min_fields"
    fail_policy: "hard"
    description: "Require non-empty title, classification, and status in final outputs."

# -----------------------------
# Execution Quality
# -----------------------------
execution_quality:
  determinism: "seeded"
  resource_limits: "standard"
  monitoring: "basic"

# -----------------------------
# Outputs Quality
# -----------------------------
outputs_quality:
  plausibility_rules:
    - "metadata.title must not be empty"
    - "metadata.classification must not be empty"
    - "metadata.status must not be empty"
  redaction:
    policy: "snippets_only"
    replacement: "[[REDACTED]]"

# -----------------------------
# Method (Pipeline)
# -----------------------------
method:
  pipeline:
    - id: "pre_flight_select_document"   # NEW
      description: "Ask the user which document to process; list recent uploads if available; require explicit confirmation; stop if none chosen. Guard against selecting this tool spec."
      steps:
        - "Ask: 'Which document should I apply the metadata extractor to?'"
        - "If multiple candidates exist, present a short list and an 'Other/Upload' option."
        - "Confirm selection: 'Apply to <FILENAME>? (yes/no)' and proceed only on yes."
        - "Run self_spec_guard; if positive, stop with message: 'You selected the tool specification; please choose a different document.'"
        - "If none selected, stop with: 'No target document selected; please upload or pick a file.'"
    - id: "ingest"
      description: "Read target_document bytes, detect source_format, compute source_file_hash."
    - id: "extract_text_and_meta"
      description: "Extract text content and embedded metadata (title?, authors?, created?, modified?)."
    - id: "normalize"
      description: "Normalize whitespace, fix encoding, collapse hyphenation, and standardize headings."
    - id: "derive_ids"
      description: "Generate document_id and (if configured) registry_id."
    - id: "section_register"
      description: "Build the Section Register (see 'metadata_template.section_register')."
    - id: "auto_classification"
      description: "Assign classification, status, categories, tags (include confidence in diagnostics)."
    - id: "pii_redaction"
      description: "If enabled, redact PII only in Section Details snippets."
    - id: "assemble_markdown"
      description: "Assemble the 'Document Metadata Sheet' in Markdown using metadata_template."
    - id: "mirror_json"
      description: "Optionally generate a JSON mirror of the metadata."
    - id: "assert_and_postprocess"
      description: "Validate plausibility rules; apply output file names; finalize diagnostics."

# -----------------------------
# Guardrails
# -----------------------------
guardrails:
  - "no_unfounded_values"       # Do not invent authors/dates; leave blank with a note.
  - "respect_template_contract" # Keep required fields and Section Register headings.
  - "confidentiality_default"   # Default classification='Internal' if unknown.
  - "do_not_process_self"       # Never process this tool spec as a target.

# -----------------------------
# Outputs
# -----------------------------
outputs:
  - name: "metadata_markdown"
    type: "file"
    description: "The completed 'Document Metadata Sheet' in Markdown format."
  - name: "metadata_json"
    type: "file"
    description: "JSON mirror of the metadata (optional)."
  - name: "diagnostics"
    type: "object"
    description: "Extraction counts, quality measures, warnings, issues, and confidences."

# -----------------------------
# Evaluation
# -----------------------------
evaluation:
  metrics:
    - "rejects when no target selected (pre-flight)"
    - "rejects when self-spec chosen"
    - "produces non-empty title, classification, status"
    - "redacts PII only in snippets"
  unit_tests:
    - id: "UT-001"
      expect: "E-NO-TARGET"
      when: "no file confirmed in pre-flight"
    - id: "UT-002"
      expect: "E-SELF-SPEC"
      when: "selected file matches spec heuristics"
    - id: "UT-003"
      expect: "OK"
      when: "typical PDF with headings and body text"
    - id: "UT-004"
      expect: "WARN(text_extractability)"
      when: "extracted chars < 250"

# -----------------------------
# Fields (for runtime record)
# -----------------------------
fields:
  warnings: []
  issues: []
  stats: []

# -----------------------------
# Error Messages (User-facing)
# -----------------------------
errors:
  E-NO-TARGET: "No target document selected; please upload or pick a file."
  E-SELF-SPEC: "The selected file is the tool specification; choose a different document."
  E-UNSUPPORTED: "Unsupported file type: {{ext}}."
  E-EMPTY-TEXT: "Couldn't extract meaningful text; consider OCR or a text-based version."

# ===================================================================
# METADATA TEMPLATE (Output Schema + Markdown Scaffold)
# ===================================================================
metadata_template:
  document_metadata:
    fields:
      - key: "document_id"
        type: "string"
        required: false
      - key: "title"
        type: "string"
        required: true
      - key: "description"
        type: "string"
        required: false
      - key: "version"
        type: "string"
        required: false
      - key: "language"
        type: "string"
        required: false
      - key: "authors"
        type: "list[string]"
        required: false
      - key: "publisher"
        type: "string"
        required: false
      - key: "date_created"
        type: "date"
        required: false
      - key: "date_received"
        type: "date"
        required: false
      - key: "date_converted"
        type: "date"
        required: false
      - key: "source_format"
        type: "string"
        required: true
      - key: "source_file"
        type: "string"
        required: true
      - key: "markdown_file"
        type: "string"
        required: false
      - key: "tags"
        type: "list[string]"
        required: false
      - key: "categories"
        type: "list[string]"
        required: false
      - key: "classification"
        type: "string"
        required: true
      - key: "status"
        type: "string"
        required: true
      - key: "related_documents"
        type: "list[string]"
        required: false
      - key: "registry_indexed"
        type: "boolean"
        required: false
      - key: "registry_id"
        type: "string"
        required: false
      - key: "notes"
        type: "string"
        required: false

  section_register:
    # Canonical register used to build Section Details
    items:
      - section_id: "0"
        title: "Pre-flight"
        description: "Target selection and confirmation (ask user)."
        tags: []
      - section_id: "1"
        title: "Tool Header"
        description: "Name, version, purpose block at the top."
        tags: []
      - section_id: "2"
        title: "Inputs"
        description: "Input parameters and their types."
        tags: []
      - section_id: "3"
        title: "Parameters"
        description: "Processing parameters such as discovery mode, classification, and timezone."
        tags: []
      - section_id: "4"
        title: "Uniqueness Keys"
        description: "Uniqueness constraints for registry usage."
        tags: []
      - section_id: "5"
        title: "Checks"
        description: "Validation checks and fail policies."
        tags: []
      - section_id: "6"
        title: "Execution Quality"
        description: "Determinism seed, resource limits, and monitoring instructions."
        tags: []
      - section_id: "7"
        title: "Outputs Quality"
        description: "Plausibility rules and redaction policy."
        tags: []
      - section_id: "8"
        title: "Method: Pipeline"
        description: "Step-by-step pipeline."
        tags: []
      - section_id: "9"
        title: "Guardrails"
        description: "Behavioral and confidentiality rules."
        tags: []
      - section_id: "10"
        title: "Outputs"
        description: "List of produced artifacts."
        tags: []
      - section_id: "11"
        title: "Evaluation"
        description: "Metrics and unit tests."
        tags: []
      - section_id: "12"
        title: "Fields & Stats"
        description: "warnings, issues, stats fields."
        tags: []

  section_details_scaffold:
    # Markdown scaffold used to render the Section Details nicely
    template: |
      # Document Metadata
      - document_id:
      - title:
      - description:
      - version:
      - language:
      - authors: []
      - publisher:
      - date_created:
      - date_received:
      - date_converted:
      - source_format:
      - source_file:
      - markdown_file:
      - tags: []
      - categories: []
      - classification:
      - status:
      - related_documents: []
      - registry_indexed: false
      - registry_id:
      - notes:

      ---
      # Section Register
      - section_id: 0
        parent_id:
        title: Pre-flight
        description: Target selection and confirmation (ask user).
        tags: []
      - section_id: 1
        parent_id:
        title: Tool Header
        description: Name, version, purpose block at the top.
        tags: []
      - section_id: 2
        parent_id:
        title: Inputs
        description: Input parameters and their types.
        tags: []
      - section_id: 3
        parent_id:
        title: Parameters
        description: Processing parameters such as discovery mode, classification, and timezone.
        tags: []
      - section_id: 4
        parent_id:
        title: Uniqueness Keys
        description: Uniqueness constraints for registry usage.
        tags: []
      - section_id: 5
        parent_id:
        title: Checks
        description: Validation checks and fail policies.
        tags: []
      - section_id: 6
        parent_id:
        title: Execution Quality
        description: Determinism seed, resource limits, and monitoring instructions.
        tags: []
      - section_id: 7
        parent_id:
        title: Outputs Quality
        description: Plausibility rules and redaction policy.
        tags: []
      - section_id: 8
        parent_id:
        title: Method: Pipeline
        description: Step-by-step pipeline.
        tags: []
      - section_id: 9
        parent_id:
        title: Guardrails
        description: Behavioral and confidentiality rules.
        tags: []
      - section_id: 10
        parent_id:
        title: Outputs
        description: List of produced artifacts.
        tags: []
      - section_id: 11
        parent_id:
        title: Evaluation
        description: Metrics and unit tests.
        tags: []
      - section_id: 12
        parent_id:
        title: Fields & Stats
        description: warnings, issues, stats fields.
        tags: []

      ---
      # Section Details
      ## 0 Pre-flight
      **Parent:** -
      **Description:** Target selection and confirmation (ask user).
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 1 Tool Header
      **Parent:** -
      **Description:** Name, version, purpose block at the top.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 2 Inputs
      **Parent:** -
      **Description:** Input parameters and their types.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 3 Parameters
      **Parent:** -
      **Description:** Processing parameters such as discovery mode, classification, and timezone.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 4 Uniqueness Keys
      **Parent:** -
      **Description:** Uniqueness constraints for registry usage.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 5 Checks
      **Parent:** -
      **Description:** Validation checks and fail policies.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 6 Execution Quality
      **Parent:** -
      **Description:** Determinism seed, resource limits, and monitoring instructions.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 7 Outputs Quality
      **Parent:** -
      **Description:** Plausibility rules and redaction policy.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 8 Method: Pipeline
      **Parent:** -
      **Description:** Step-by-step pipeline.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 9 Guardrails
      **Parent:** -
      **Description:** Behavioral and confidentiality rules.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 10 Outputs
      **Parent:** -
      **Description:** List of produced artifacts.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 11 Evaluation
      **Parent:** -
      **Description:** Metrics and unit tests.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

      ## 12 Fields & Stats
      **Parent:** -
      **Description:** warnings, issues, stats fields.
      **Tags:** []
      **Extracted Text:**
      ```

      ```

# End of specification (v1.1.0)
