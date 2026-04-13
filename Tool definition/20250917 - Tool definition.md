# =========================================
# 🧩 SELF-EXECUTING TOOL DEFINITION (YAML)
# =========================================
# Paste this into a fresh GPT chat. The assistant should follow the
# "instructions_for_gpt" sequence to collect inputs, validate them,
# execute the pipeline, apply guardrails, and return structured outputs.

instructions_for_gpt: |
  You are an assistant that helps populate and execute this tool template.
  When this file is given to you, do the following:
  1) Read the "purpose" and explain to the user what the tool does.
  2) Guide the user step-by-step to provide all required "inputs".
  3) Validate inputs against "quality.inputs_quality" rules.
  4) If inputs are invalid, prompt the user again until valid.
  5) Once inputs are complete, run through "method.pipeline" in order.
  6) Enforce "decision_logic" and "guardrails".
  7) Apply "quality.outputs_quality" rules before presenting results.
  8) Present outputs in the structure defined under "outputs".
  Always log checks and decisions into "diagnostics". Never skip sections;
  if something is missing, ask the user to fill it.

tool_name: "<set-a-clear-tool-name>"
version: "0.1.0"
purpose: >
  Briefly describe what the tool does, the problem it solves, and its scope.
  Keep this concrete and user-facing.

# -----------------
# INPUTS & OUTPUTS
# -----------------
inputs:
  # Declare each required input with type and description.
  # Example:
  - name: "<input_name>"
    type: "string|number|integer|boolean|date|enum|object|array"
    required: true
    description: "What this input represents and how to provide it."
    example: "<example-value>"
  # Add more inputs as needed...

outputs:
  # Define the tool’s returned structure. Use field-by-field clarity.
  - name: "<output_name>"
    type: "object|array|string|number|boolean"
    description: "What this output is and how to interpret it."

parameters:
  # Optional runtime/config knobs with defaults.
  - name: "<param_name>"
    type: "string|number|boolean|enum"
    default: "<default>"
    description: "How this parameter changes behavior."

# -------------
# QUALITY BLOCK
# -------------
quality:
  data_contracts:
    # Enumerate any upstream data contracts (if referencing external data).
    - name: "<contract_name>"
      type: "schema|api|file|table"
      description: "What is guaranteed by the source."
    # Add more as needed...

  input_schema: {}     # JSONSchema-like object if you want formal schema.
  output_schema: {}    # JSONSchema-like object for outputs.

  reference_sets:
    - name: "<refset_name>"
      source: "<doc|url|standard>"
      allowed_values: ["<value1>", "<value2>"]

  inputs_quality:
    standards:
      completeness_threshold: 1.0         # 0.0–1.0
      freshness_max_age: "P30D"           # ISO8601 duration or ""
      uniqueness_keys: []                 # e.g., ["id"]
    allowed_ranges:
      - field: "<field_name>"
        rule: "min<=x<=max | regex | enum"
    cross_field_rules:
      - rule: "if <fieldA>, then <fieldB> required"
        severity: "error"   # error|warn
    checks:
      - name: "schema_validation"
        type: "rule"        # code|rule
        fail_policy: "error" # error|warn|coerce
    missing_data_policy:
      strategy: "reject"     # reject|impute|drop_fields
      imputation:
        method: "none"       # mean|median|mode|llm|ffill|bfill|none
        fields: []
        max_impute_ratio: 0.1

  execution_quality:
    determinism:
      seed: 42
      report_seed: true
    invariants:
      - "no_cycles_in_graph"
      - "durations_non_negative"
    numeric_stability:
      epsilon: 1e-9
      divide_by_zero: "return_null_and_flag"
      overflow_policy: "clip_and_flag"
    resource_limits:
      max_runtime_ms: 30000
      max_memory_mb: 512
      timeout_policy: "fail_with_partial_diagnostics"
    monitoring:
      capture_intermediate: true
      trace_fields: ["step_id","row_counts","nan_counts"]

  outputs_quality:
    plausibility_rules:
      - rule: "<describe-expected-output-bounds-or-logic>"
        severity: "warn"     # error|warn
    reconciliation:
      - check: "<describe-cross-check-or-aggregation>"
        tolerance: 1e-6
    uncertainty_reporting:
      enabled: false
      fields: []
    redaction:
      pii_fields: []
      policy: "remove"        # remove|mask|hash
    formatting:
      rounding:
        - field: "<numeric_field>"
          decimals: 2
      ordering: "stable"       # stable|by_field:<name>
      locale: "en-US"

# -------------
# METHOD & FLOW
# -------------
method:
  pipeline:
    - id: "validate_inputs"
      type: "rule"
      spec:
        body: |
          # Validate required fields, types, ranges, enums, and cross-field rules.
          # Emit issues to diagnostics if anything fails.
    - id: "core_logic"
      type: "llm"   # llm|code
      spec:
        prompt: |
          # If using LLM: Provide deterministic, stepwise instructions to transform inputs into outputs.
          # Be explicit about any formulas, scoring, or mapping to reference_sets.
        body: |
          # If using code (pseudocode):
          # 1) Transform inputs
          # 2) Compute key results
          # 3) Assemble outputs structure
    - id: "assert_invariants"
      type: "rule"
      spec:
        body: |
          # Re-check execution invariants (e.g., non-negative durations, graph acyclicity).
    - id: "postprocess_outputs"
      type: "rule"
      spec:
        body: |
          # Apply formatting, rounding, redaction, plausibility checks, reconciliation.

  decision_logic: |
    # Describe thresholds/branches. Example:
    # If risk_score >= 0.7 then set decision="high_risk" else "standard".

  guardrails:
    - name: "disallow_empty_results"
      rule: "outputs must contain at least one record or field as defined."
    - name: "respect_reference_sets"
      rule: "Any enum/classification must be within allowed_values."

# ----------------
# EVALUATION SUITE
# ----------------
evaluation:
  metrics: ["accuracy","consistency","robustness","latency"]
  unit_tests:
    - name: "example_case"
      input:
        "<input_name>": "<example>"
      expect:
        "<output_name>": "<expected>"
  property_tests:
    - name: "idempotence"
      mutate: "shuffle-nonsemantic-ordering"
      expect: { "outputs_identical": true }
  metamorphic_tests:
    - name: "scale_invariance_small"
      transform: "multiply_continuous_inputs_by: 1.01"
      expect: { "relative_change_bounded": 0.02 }

# -----------
# DIAGNOSTICS
# -----------
diagnostics:
  emit: ["warnings","issues","intermediate_stats","seed","timings_ms"]

provenance:
  prompt_hash: ""   # Assistant should populate a hash of the prompt/template it used.
  code_versions:
    - component: "core_logic"
      version: "v0"
  data_lineage:
    - input: "<input_name>"
      checksum: "<optional-checksum>"
