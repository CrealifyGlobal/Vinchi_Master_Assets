tool_name: "Build Location Schema"
version: "0.2.0"
summary: "Elicit and convert product location data into a hierarchy and RDF graph with adjacency, orientation, and openings."
purpose: "Use to define structured locations (areas, sections, blocks) for any product (e.g., house, vessel) and output a consistent hierarchy plus RDF triples for downstream reasoning and integration. The tool co-develops the minimal ontology during elicitation."

ownership:
  author: "Servanti Core"
  contact: "servanti@internal"
  source_repo: "TBD"

permissions:
  requires_network: false
  reads_files: false
  writes_files: false
  pii_allowed: false
  scopes: []

context:
  locale: "en-US"
  timezone: "Europe/Amsterdam"
  defaults:
    id_prefix: "loc:"
    base_iri: "https://example.org/location/"
    default_predicates:
      parent: "https://example.org/ontology/partOf"
      contains: "https://example.org/ontology/hasPart"
      located_in: "https://example.org/ontology/locatedIn"
      adjacent_to: "https://example.org/ontology/adjacentTo"
      connected_to: "https://example.org/ontology/connectedTo"
      connects_via: "https://example.org/ontology/connectsVia"
      has_opening: "https://example.org/ontology/hasOpening"
      opening_leads_to: "https://example.org/ontology/openingLeadsTo"
      boundary_of: "https://example.org/ontology/boundaryOf"
      on_side: "https://example.org/ontology/onSide"
      oriented_as: "https://example.org/ontology/orientedAs"
      right_of: "https://example.org/ontology/rightOf"
      left_of: "https://example.org/ontology/leftOf"
      in_front_of: "https://example.org/ontology/inFrontOf"
      behind: "https://example.org/ontology/behind"
      above: "https://example.org/ontology/above"
      below: "https://example.org/ontology/below"
    node_types:
      - "Building"
      - "Vessel"
      - "Deck"
      - "Block"
      - "Compartment"
      - "Room"
      - "Zone"
      - "Opening"     # doorway, hatch
      - "Boundary"    # wall, bulkhead
      - "Connector"   # corridor, stairs

inputs:
  - name: "product_type"
    type: "string"
    required: true
    description: "High-level class of the product (e.g., House, Yacht, Factory)."
    example: "Vessel"

  - name: "source_text"
    type: "string"
    required: false
    default: ""
    description: "Free text describing locations/sections (bullets/indentation allowed)."
    example: |
      Main Deck
      - Galley (right side)
      - Head (left side)
      Hull
      - Forward block F-L-01
      - Engine room (aft)

  - name: "seed_nodes"
    type: "array"
    required: false
    default: []
    description: "Predefined nodes with optional parent/labels/types."
    example:
      - { id: "", label: "Main Deck", type: "Deck", parent: "" }
      - { id: "", label: "Galley", type: "Room", parent: "Main Deck" }

  - name: "relations"
    type: "array"
    required: false
    default: []
    description: "Explicit containment/location relations (subject, predicate-key, object)."
    example:
      - { subj: "Galley", pred: "located_in", obj: "Main Deck" }

  - name: "spatial_relations"
    type: "array"
    required: false
    default: []
    description: "Perspective/topological relations between nodes."
    example:
      - { subj: "Galley", pred: "adjacent_to", obj: "Head" }
      - { subj: "Galley", pred: "right_of", obj: "Head" }
      - { subj: "Galley Right Bulkhead", pred: "boundary_of", obj: "Galley" }

  - name: "openings"
    type: "array"
    required: false
    default: []
    description: "Openings/doorways with placement and connectivity."
    example:
      - { label: "Galley–Head Door", kind: "Doorway",
          host_boundary: "Galley Right Bulkhead",
          connects: ["Galley", "Head"],
          leads_to: "Head" }

  - name: "ontology"
    type: "object"
    required: false
    default: {}
    description: "Optional custom ontology to seed classes/predicates."
    example:
      prefixes: { ex: "https://example.org/ontology/" }
      predicates: { partOf: "ex:partOf", hasPart: "ex:hasPart" }
      classes: { Room: "ex:Room", Deck: "ex:Deck" }

  - name: "naming_scheme"
    type: "string"
    required: false
    default: "slug"
    description: "How to mint IDs: slug | uuid | hash | label_code."
    example: "label_code"

  - name: "id_prefix"
    type: "string"
    required: false
    default: "loc:"
    description: "Prefix for minted local IDs."
    example: "str:"

  - name: "base_iri"
    type: "string"
    required: false
    default: "https://example.org/location/"
    description: "Base IRI used when emitting full IRIs."
    example: "https://str.boats/loc/"

  - name: "max_depth"
    type: "number"
    required: false
    default: 6
    description: "Maximum hierarchy depth to build."
    example: 7

  - name: "max_nodes"
    type: "number"
    required: false
    default: 500
    description: "Safety cap on total nodes generated."
    example: 300

  - name: "output_format"
    type: "string"
    required: false
    default: "turtle"
    description: "turtle | json-ld | both."
    example: "both"

  - name: "validate_shacl"
    type: "boolean"
    required: false
    default: false
    description: "If true, run minimal SHACL-like structural checks."

  - name: "orientation_frame"
    type: "string"
    required: false
    default: "cardinal"
    description: "How right/left/front are interpreted: cardinal|vessel|vehicle|local-room."
    example: "vessel"

  - name: "orientation_hints"
    type: "object"
    required: false
    default: {}
    description: "Anchors for orientation mapping."
    example:
      forward: "Bow"
      right: "Starboard"

parameters:
  - name: "max_steps"
    type: "integer"
    default: 10
    description: "Safety cap on loop/branch expansions"

  - name: "allow_cycles"
    type: "boolean"
    default: false
    description: "If false, prevent cycles in part-of graph."

  - name: "canonicalize_labels"
    type: "boolean"
    default: true
    description: "Lowercase/trim and de-duplicate labels."

  - name: "infer_relations"
    type: "boolean"
    default: true
    description: "Derive hasPart/partOf from indentation, headings, or cues."

  - name: "emit_paths"
    type: "boolean"
    default: true
    description: "Include full breadcrumb path for each node."

  - name: "infer_inverse_relations"
    type: "boolean"
    default: true
    description: "Emit symmetric/inverse triples (e.g., adjacentTo symmetric; rightOf ↔ leftOf)."

  - name: "enforce_symmetry"
    type: "boolean"
    default: true
    description: "For symmetric predicates like adjacent_to, assert both directions."

  - name: "resolve_perspective"
    type: "boolean"
    default: true
    description: "Normalize 'right wall', 'front bulkhead' into boundary + on_side relations."

preconditions:
  - "All required inputs present"
  - "Inputs conform to schema"
  - "If validate_shacl=true and ontology provided, ontology must include class/predicate IRIs"
  - "max_nodes and max_depth within limits"

outputs:
  - name: "nodes"
    type: "array"
    description: "Canonical node list (id, label, type, parent?, path?)."
  - name: "edges"
    type: "array"
    description: "Triples as subject/predicate/object (IDs or IRIs)."
  - name: "connectivity_edges"
    type: "array"
    description: "Derived path/door/connector edges (room ↔ room via opening/connector)."
  - name: "openings_nodes"
    type: "array"
    description: "Minted Opening entities with host boundaries and leads_to relations."
  - name: "rdf_turtle"
    type: "string"
    description: "RDF serialization in Turtle (if requested)."
  - name: "rdf_jsonld"
    type: "string"
    description: "RDF serialization in JSON-LD (if requested)."
  - name: "constraints"
    type: "array"
    description: "Consistency notes (symmetry enforced, unresolved references, defaults applied)."
  - name: "issues"
    type: "array"
    description: "Schema/validation issues encountered."
  - name: "diagnostics"
    type: "object"
    description: "Non-functional telemetry (warnings, timing, counters)"
    fields:
      warnings: { type: "array", items: { type: "string" } }
      issues:   { type: "array", items: { type: "string" } }
      timings_ms: { type: "object" }

quality:
  data_contracts:
    - name: "input_schema_valid"
      type: "rule"
      description: "Reject if required inputs missing or malformed"
  inputs_quality:
    checks:
      - name: "schema_validation"
        type: "rule"
        fail_policy: "error"
  execution_quality:
    resource_limits:
      max_runtime_ms: 15000
      max_memory_mb: 256
    determinism:
      seed: 42
      report_seed: true
  outputs_quality:
    plausibility_rules:
      - rule: "nodes non-empty if source_text or seed_nodes present and no fatal issues"
        severity: "error"
      - rule: "No cycles when allow_cycles=false"
        severity: "error"
      - rule: "Edge predicates resolve to IRIs (built-in or provided ontology)"
        severity: "error"
      - rule: "adjacent_to must be symmetric when enforce_symmetry=true"
        severity: "error"
      - rule: "opening_leads_to object must exist as a node"
        severity: "error"
      - rule: "boundary_of must reference a valid container node"
        severity: "error"

method:
  pipeline:

    # 0) Ontology elicitation & seeding
    - id: "elicit_ontology"
      type: "activity"
      kind: "llm|rule"
      spec:
        body: |
          # From product_type + source_text, propose minimal classes/predicates.
          # Merge with provided 'ontology' and 'default_predicates'.
          # If missing essentials (Room, Deck, Boundary, Opening, partOf/hasPart), mint neutral IRIs under https://example.org/ontology/.
          # Produce an 'ontology_active' map (prefixes, classes, predicates) for downstream steps.
      on_fail: "abort"

    # 1) Obtain/confirm inputs
    - id: "get_inputs"
      type: "activity"
      kind: "rule"
      spec:
        body: |
          assert product_type and isinstance(product_type, str)
          assert max_nodes <= 5000 and max_nodes > 0
          assert 1 <= max_depth <= 20
          assert output_format in {"turtle","json-ld","both"}
      on_fail: "abort"

    # 2) Validate / normalize inputs
    - id: "validate_inputs"
      type: "activity"
      kind: "rule"
      spec:
        body: |
          # Canonicalize labels/IDs if requested; trim strings.
          # Merge 'relations' with defaults; validate predicate keys exist in ontology_active.
          # Prepare predicate IRI map and class IRI map.
      on_fail: "abort"

    # 3) Candidate extraction from text + seed nodes
    - id: "extract_candidates"
      type: "activity"
      kind: "llm|code"
      spec:
        body: |
          # Extract candidate locations from source_text (indentation/bullets imply containment).
          # Combine with seed_nodes; de-duplicate on canonical label.
          # Enforce max_nodes and max_depth caps.
      on_fail: "fallback:process_safe"

    # 4) Build hierarchy and mint IDs
    - id: "structure_hierarchy"
      type: "activity"
      kind: "code"
      spec:
        body: |
          # Build tree using inferred/explicit parents (relations).
          # Mint stable IDs via naming_scheme + id_prefix.
          # Compute breadcrumb paths if emit_paths=true.
      on_fail: "fallback:process_safe"

    # 5) Normalize perspective (boundaries, sides, orientation)
    - id: "normalize_perspective"
      type: "activity"
      kind: "rule|code"
      spec:
        body: |
          # Interpret orientation_frame (cardinal/vessel/vehicle/local-room).
          # Expand "Right Wall" → Boundary node + (boundary_of) + (on_side "right"|"starboard"|...).
          # Apply orientation_hints (e.g., right→Starboard).
      on_fail: "continue"

    # 6) Integrate spatial relations (adjacency, right_of/left_of, etc.)
    - id: "integrate_spatial_relations"
      type: "activity"
      kind: "code"
      spec:
        body: |
          # Merge explicit spatial_relations with inferred ones.
          # Enforce symmetry for adjacent_to; add inverses if infer_inverse_relations=true.
          # For right_of/left_of, assert inverse pairings.
          # Validate references; mint missing nodes cautiously (within caps).
      on_fail: "fallback:process_safe"

    # 7) Model openings/connectors and connectivity graph
    - id: "model_openings"
      type: "activity"
      kind: "code"
      spec:
        body: |
          # For each opening:
          #   - Mint Opening node (Doorway/Hatch) with label/kind.
          #   - Attach to host boundary via has_opening or connects_via.
          #   - Emit connected_to edges between connected spaces.
          #   - Emit opening_leads_to where specified.
          # Populate connectivity_edges.
      on_fail: "fallback:process_safe"

    # 8) Derive RDF edges for containment and spatial predicates
    - id: "derive_edges"
      type: "activity"
      kind: "code"
      spec:
        body: |
          # Emit triples:
          # - child partOf parent; parent hasPart child
          # - located_in/adjacent_to/right_of/left_of/in_front_of/behind/above/below
          # - boundary_of, on_side, connected_to, connects_via, has_opening, opening_leads_to
          # Validate no cycles if allow_cycles=false.
      on_fail: "fallback:process_safe"

    # 9) SHACL-like validation (optional)
    - id: "validate_graph"
      type: "activity"
      kind: "rule"
      when: "validate_shacl == true"
      spec:
        body: |
          # Checks:
          # - Every node has {id,label,type}
          # - partOf acyclic if allow_cycles=false
          # - Predicates are valid IRIs in ontology_active
          # - Openings connect ≥ 2 spaces; referenced nodes exist
      on_fail: "continue"

    # 10) Serialize RDF
    - id: "serialize_rdf"
      type: "activity"
      kind: "code"
      spec:
        body: |
          # Build graph in-memory and serialize to Turtle and/or JSON-LD per output_format.
      on_fail: "fallback:process_safe"

    # 11) Post-process & format outputs
    - id: "format_outputs"
      type: "activity"
      kind: "rule|code"
      spec:
        body: |
          # Map in-memory structures to outputs; sort nodes by path, edges by (subj,pred,obj).
          # Populate diagnostics (counts, timings, warnings).
          # Emit constraints: symmetry applied, unresolved refs, default mappings used.

    # Safe fallback
    - id: "process_safe"
      type: "activity"
      kind: "rule|code"
      when: "previous_step_failed == true"
      spec:
        body: |
          # Conservative fallback: emit flat nodes from seed_nodes + top-level from source_text.
          # Only identity edges; include issues explaining which step failed.

decision_logic: |
  # If output_format == "both", emit both serializations.
  # If infer_relations=false, only use explicit relations and seed hierarchy.
  # If resolve_perspective=false, skip boundary/on_side expansion.

control_flow:
  retries:
    policy: "exponential_backoff"
    max_retries: 2
    base_delay_ms: 200
  idempotency:
    strategy: "hash_inputs"
  concurrency:
    enabled: false

guardrails:
  - name: "no_pii_leak"
    rule: "Never emit emails/phones unless pii_allowed=true"
  - name: "bounded_search"
    rule: "Any input lookup must finish under max_runtime_ms"
  - name: "acyclic_partOf"
    rule: "Disallow cycles in partOf unless allow_cycles=true"
  - name: "node_cap"
    rule: "Do not exceed max_nodes; truncate with warning"
  - name: "resolve-references"
    rule: "Do not emit triples with unknown subjects/objects; log issues instead."

postconditions:
  - "All declared outputs present and typed correctly"
  - "diagnostics.timings_ms includes total and per-step timing"
  - "If output_format includes turtle/json-ld, corresponding string is non-empty"

evaluation:
  metrics: ["latency","failure_rate","contract_adherence"]
  unit_tests:
    - name: "house_minimal"
      input:
        product_type: "House"
        source_text: |
          Ground Floor
          - Kitchen
          - Bathroom
          First Floor
          - Bedroom
        spatial_relations:
          - { subj: "Kitchen", pred: "adjacent_to", obj: "Bathroom" }
        openings:
          - { label: "Kitchen–Bathroom Door", kind: "Doorway",
              host_boundary: "Kitchen Right Wall",
              connects: ["Kitchen", "Bathroom"],
              leads_to: "Bathroom" }
        output_format: "both"
      expect:
        nodes: "@nonempty"
        rdf_turtle: "@nonempty"
        rdf_jsonld: "@nonempty"
        connectivity_edges: "@nonempty"

    - name: "vessel_orientation"
      input:
        product_type: "Vessel"
        orientation_frame: "vessel"
        source_text: |
          Main Deck
          - Galley (right side)
          - Head (left side)
        output_format: "turtle"
      expect:
        nodes: "@nonempty"
        edges: "@nonempty"
        issues: []

  property_tests:
    - name: "idempotent_rerun"
      mutate: "run twice with same inputs"
      expect: "identical outputs"

  metamorphic_tests:
    - name: "scaling_input_size"
      transform: "increase locations 10x within max_nodes"
      expect: "runtime increases sublinearly and output schema holds"

provenance:
  prompt_hash: ""           # if LLM used
  code_versions:
    - component: "pipeline"
      version: "0.2.0"
