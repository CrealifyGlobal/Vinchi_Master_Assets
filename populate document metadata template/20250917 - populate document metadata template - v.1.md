tool_name: "Document Metadata Populator"
version: "1.0.0"
purpose: "Extract metadata and section structure from a source document (PDF/DOCX/MD/TXT) and populate the 'Document Metadata Sheet' template with validated fields and a section register + details."

inputs:
  - name: "document_blob"
    type: "file"
    required: true
    description: "The source file (PDF, DOCX, MD, or TXT)."
  - name: "source_format"
    type: "string"
    required: true
    description: "One of: PDF, DOCX, MD, TXT."
  - name: "document_hints"
    type: "object"
    required: false
    description: "Optional hints to guide extraction when fields are ambiguous."
    fields:
      title: { type: "string" }
      authors: { type: "array", items: { type: "string" } }
      publisher: { type: "string" }
      language: { type: "string", description: "BCP-47 code (e.g., en, nl-NL)" }
      creation_date: { type: "string", description: "ISO 8601" }
      categories: { type: "array", items: { type: "string" } }
      tags: { type: "array", items: { type: "string" } }
  - name: "registry_params"
    type: "object"
    required: false
    description: "Use if integrating with a registry."
    fields:
      registry_indexed: { type: "boolean", default: false }
      registry_id: { type: "string" }
  - name: "output_file_names"
    type: "object"
    required: false
    fields:
      markdown_file: { type: "string", description: "Desired output filename, e.g., '<slug>.md'." }

parameters:
  - name: "section_discovery_mode"
    type: "string"
    default: "hybrid"
    description: "heading_based | toc_based | hybrid"
  - name: "max_register_items"
    type: "integer"
    default: 200
  - name: "auto_classify"
    type: "boolean"
    default: true
    description: "Infer tags, categories, and classification from content."
  - name: "redact_pii"
    type: "boolean"
    default: false
  - name: "lang_detect_conf_min"
    type: "number"
    default: 0.6
  - name: "doc_id_strategy"
    type: "string"
    default: "hash"
    description: "hash | filename | provided"
  - name: "timezone"
    type: "string"
    default: "Europe/Amsterdam"

quality:
  data_contracts:
    - name: "supported_format"
      type: "rule"
      description: "Reject if source_format not in {PDF,DOCX,MD,TXT}."
    - name: "non_empty_file"
      type: "rule"
      description: "Reject if document_blob is empty or unreadable."
  inputs_quality:
    standards:
      completeness_threshold: 1.0
      uniqueness_keys: []
    checks:
      - name: "mimetype_validation"
        type: "code"
        fail_policy: "error"
      - name: "text_extractability"
        type: "code"
        fail_policy: "error"
  execution_quality:
    determinism: { seed: 42, report_seed: true }
    resource_limits: { max_runtime_ms: 45000, max_memory_mb: 1024 }
    monitoring:
      capture_intermediate: true
      trace_fields: ["pages","chars","headings_found","toc_found"]
  outputs_quality:
    plausibility_rules:
      - rule: "title must be non-empty; if not found, fall back to filename sans extension."
        severity: "warn"
      - rule: "date_created should not be in the future."
        severity: "warn"
      - rule: "At least one section in Section Register."
        severity: "error"
    redaction:
      pii_fields: ["authors","publisher","notes"]
      policy: "mask_if_requested"

method:
  pipeline:
    - id: "ingest"
      type: "code"
      spec:
        body: |
          # Load bytes; sniff mimetype; route to appropriate extractor.
    - id: "extract_text_and_meta"
      type: "code"
      spec:
        body: |
          # PDF: parse XMP/core properties; fall back to first-page heuristics.
          # DOCX: read core.xml (title, subject, creator, created/modified).
          # MD/TXT: parse front matter if present; else heuristics.
          # Collect raw text, headings, page count; detect language.
    - id: "normalize_dates_lang"
      type: "code"
      spec:
        body: |
          # Normalize dates to ISO 8601; choose language by detector (>lang_detect_conf_min) else use hint/default.
    - id: "derive_ids_and_paths"
      type: "code"
      spec:
        body: |
          # Compute document_id by doc_id_strategy; set source_file name; propose markdown_file name if not provided.
    - id: "section_register"
      type: "code"
      spec:
        body: |
          # Identify sections via Heading regex (H1/H2/H3) and/or ToC.
          # Produce hierarchical IDs (1, 2, 2.1, 2.2, 3, ...), titles, short descriptions (first 1–2 sentences), tags.
          # Clamp to max_register_items.
    - id: "auto_classification"
      type: "llm"
      when: "parameters.auto_classify == true"
      spec:
        prompt: |
          You are classifying a document for a metadata sheet. From the content, infer:
          - tags (5–12 concise tokens),
          - categories (2–6),
          - classification (one label like 'Public', 'Internal', 'Confidential', or 'Restricted'—choose carefully).
          Output strict JSON.
    - id: "pii_redaction"
      type: "code"
      when: "parameters.redact_pii == true"
      spec:
        body: |
          # Mask emails/phones in notes and section bodies using reversible tokens.
    - id: "assemble_markdown"
      type: "llm"
      spec:
        prompt: |
          Using the “Document Metadata Sheet” template structure, populate:
          - Document Metadata block (document_id, title, description, version, language, authors[], publisher, date_created, date_received, date_converted, source_format, source_file, markdown_file, tags[], categories[], classification, status, related_documents[], registry_indexed, registry_id, notes).
          - Section Register (hierarchy).
          - Section Details (each section with section_id, parent_id, title, description, tags + extracted text).
          Keep field names and ordering exactly as in the template. Where a field is unknown, set to an empty string or empty list (do NOT invent).
          Use triple-dash '---' separators exactly as in the template.
    - id: "mirror_json"
      type: "code"
      spec:
        body: |
          # Produce a JSON mirror of the populated fields for programmatic use.
    - id: "assert_and_postprocess"
      type: "rule"
      spec:
        body: |
          # Enforce plausibility rules; ensure markdown compiles; trim whitespace; stable ordering.

guardrails:
  - name: "no_unfounded_values"
    rule: "Never fabricate dates, versions, or authors. If not present and no hint, leave blank."
  - name: "respect_template_contract"
    rule: "Do not rename or reorder top-level headings or keys from the template."
  - name: "confidentiality_default"
    rule: "If classification cannot be inferred with confidence, set to 'Internal'."

outputs:
  - name: "metadata_markdown"
    type: "string"
    description: "A complete Markdown document populated in the exact template format."
  - name: "metadata_json"
    type: "object"
    description: "JSON mirror of all populated fields and sections."
  - name: "diagnostics"
    type: "object"
    fields:
      warnings: { type: "array", items: { type: "string" } }
      issues: { type: "array", items: { type: "string" } }
      stats: { type: "object", description: "pages, chars, headings_found, lang_confidence, elapsed_ms" }

evaluation:
  metrics: ["field_completeness","section_alignment","latency"]
  unit_tests:
    - name: "docx_core_props"
      input:
        source_format: "DOCX"
      expect: "title and authors populated when DOCX core.xml has values"
    - name: "pdf_xmp_dates"
      input:
        source_format: "PDF"
      expect: "date_created set from XMP or CreationDate"
    - name: "md_front_matter_passthrough"
      input:
        source_format: "MD"
      expect: "front matter values override heuristics"
