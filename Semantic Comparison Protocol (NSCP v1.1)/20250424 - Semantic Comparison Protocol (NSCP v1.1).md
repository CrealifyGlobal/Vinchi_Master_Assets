name: Nodet Semantic Comparison Protocol
version: 1.1
description: >
  A four-phase protocol for semantically modeling, analyzing, and comparing documents 
  using the Nodet system. Designed for high-transparency, evidence-traceable comparisons 
  suitable for expert review, audit, or adversarial examination.

phases:
  - phase: Document Parsing & Semantic Structuring
    id: phase_1
    description: >
      Converts unstructured documents into structured semantic formats using the Nodet schema.
    tasks:
      - id: 1.1_preprocessing
        name: Document Preprocessing
        description: >
          Prepare the document for semantic parsing. Clean, segment, and tag text.
        steps:
          - OCR or extract text from source file (PDF, scan, HTML, etc.)
          - Remove headers/footers, apply language detection
          - Segment by paragraphs or topic blocks
          - Extract and annotate document-level metadata (title, date, author, context)
        outputs:
          - clean_text_blocks
          - document_metadata

      - id: 1.2_entity_and_relationship_extraction
        name: Semantic Entity & Relationship Extraction
        description: >
          Identify and tag key entities, assertions, and relationships as semantic nodes and edges.
        steps:
          - Detect domain-specific concepts (e.g., systems, values, claims)
          - Classify concepts as classes, individuals, or SKOS concepts
          - Identify semantic relations (supports, contradicts, belongsTo, recommends, etc.)
          - Use Knodec, SKOS, Dublin Core, and RDF/Turtle standards
        outputs:
          - structured_semantic_nodes
          - structured_triples
          - nodet_semantic_model

  - phase: Document-Level Analysis & Interpretation
    id: phase_2
    description: >
      Derive grounded insights from each structured document prior to comparison.
    tasks:
      - id: 2.1_narrative_summary_layer
        name: Structured Narrative Summary
        description: >
          Generate a structured narrative summarizing key themes, intentions, and framings.
        steps:
          - Summarize document purpose and framing
          - Identify stakeholder roles and strategic positions
          - Summarize claimed problems and proposed solutions
        outputs:
          - document_summary_object
          - intent_and_theme_map

      - id: 2.2_semantic_lens_application
        name: Semantic Lens Analysis
        description: >
          Analyze all semantic entities through a consistent interpretive lens.
        lens_criteria:
          - intent: What does the concept try to achieve?
          - instrument: How does it claim to achieve it?
          - evidence: What justifies or supports it?
          - assumptions: What is presupposed or unstated?
        outputs:
          - semantic_lens_matrix

      - id: 2.3_semantic_certainty_tagging
        name: Semantic Heatmap (Optional)
        description: >
          Annotate claims based on certainty, evidence type, and contestability.
        classification_scheme:
          - asserted
          - implied
          - contested
          - supported by evidence
          - speculative
        outputs:
          - semantic_certainty_map

  - phase: Structured Comparative Analysis
    id: phase_3
    description: >
      Perform detailed and transparent comparison between two documents’ semantic models.
    tasks:
      - id: 3.1_concept_alignment_mapping
        name: Conceptual Alignment and Mapping
        description: >
          Align equivalent or related concepts across both documents using semantic proximity.
        steps:
          - Identify skos:exactMatch, closeMatch, broader/narrower
          - Map unmatched or conflicting concepts explicitly
        outputs:
          - aligned_concept_map
          - unmatched_concept_list

      - id: 3.2_comparative_claim_matrix
        name: Comparative Table Generation
        description: >
          Generate structured matrices comparing matched concepts by stance, method, and justification.
        comparison_attributes:
          - stance (support / reject / silence)
          - implementation method
          - type and quality of evidence
          - tone and framing
        outputs:
          - comparison_matrix
          - claim_alignment_table

      - id: 3.3_divergence_typology_analysis
        name: Conflict and Gap Analysis
        description: >
          Classify and explain the nature of divergences between documents.
        divergence_types:
          - epistemic (different evidence or knowledge sources)
          - normative (value differences)
          - procedural (means or mechanisms differ)
          - semantic (terms used differently)
        outputs:
          - divergence_typology_report

  - phase: Output, Traceability & Transparency
    id: phase_4
    description: >
      Package findings in transparent formats for validation, audit, and stakeholder review.
    tasks:
      - id: 4.1_provenance_trace_export
        name: Provenance-Aware Export
        description: >
          Generate semantic outputs with full traceability of each claim to its original source.
        formats:
          - `.nodet` file
          - Markdown report with semantic traceback
          - Audit map (claim → source paragraph → semantic triple)
        outputs:
          - nodet_export
          - markdown_summary
          - provenance_audit_log

      - id: 4.2_interactive_visual_matrix
        name: Interactive Concept Matrix (Optional)
        description: >
          Create a visual, navigable matrix of comparative concepts, clickable to show traceable links.
        outputs:
          - html_matrix_view
          - concept_explorer_json

      - id: 4.3_review_layer_outputs
        name: Layered Review Output Generation
        description: >
          Prepare outputs for multiple audiences: summary for execs, traceables for auditors, and visuals for presentation.
        layers:
          - Executive Summary
          - Visual Comparison Charts
          - Full Semantic Alignment Matrix
          - Divergence Explanations
        outputs:
          - exec_summary_report
          - visual_exports
          - reviewer_bundle

metadata:
  designed_by: ChatGPT for user Isaac
  designed_for: [semantic document analysis, comparative policy review, legal review preparation]
  integrity_features:
    - traceable-to-source
    - machine-readable and human-readable formats
    - divergence explanation typing
    - consistent interpretive lens
  version_notes: >
    Refined for completeness, transparency, and traceability based on user feedback from draft 1.0.
