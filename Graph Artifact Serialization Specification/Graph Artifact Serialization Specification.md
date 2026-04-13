specification:
  name: Graph Artifact Serialization Specification
  version: 0.1
  status: draft
  scope: Layer 2 – Graph Materialization (machine-first Markdown representation)

purpose:
  description: >
    Define a consistent, machine-first methodology for serializing canonical graph
    artifacts into Markdown files within a repository. The specification ensures
    that all graph objects are first-class, uniquely identifiable, interlinked,
    and traceable to their source.

principles:
  - one_artifact_one_file: Each first-class graph artifact MUST be serialized as a separate Markdown file.
  - canonical_identity: Every artifact MUST have a stable, immutable ID.
  - separation_of_concerns:
      - frontmatter: machine-readable canonical data
      - body: human-readable structured projection
  - explicit_linking: All relationships between artifacts MUST be represented via wikilinks.
  - provenance_required: Every artifact SHOULD reference its source (fragment, location, or document).
  - append_first: Updates SHOULD prefer superseding or linking over destructive edits.
  - type_strictness: Every artifact MUST declare its type explicitly.
  - graph_completeness: The serialized Markdown MUST be sufficient to reconstruct the full graph.

common_file_model:
  structure:
    - frontmatter: YAML block containing canonical fields
    - title: H1 heading representing human-readable label
    - sections: typed sections depending on artifact type

  required_frontmatter_fields:
    - id
    - type
    - label

  optional_common_fields:
    - aliases
    - source_fragments
    - source_locations
    - source_documents
    - confidence
    - status
    - created_at
    - updated_at

artifact_types:

  entity:
    description: A first-class node representing a real-world or conceptual object.
    frontmatter:
      required:
        - id
        - type: Entity
        - label
      optional:
        - aliases
        - source_fragments
    body_sections:
      - name: Types
        content: wikilinks to class/type nodes
      - name: Relationships
        content: wikilinks to relationship instance nodes
      - name: Properties
        content: wikilinks to property objects
      - name: Provenance
        content: wikilinks to fragments or locations
    example: |
      ---
      id: N-izak
      type: Entity
      label: Izak
      source_fragments:
        - [[F-001]]
      ---
      # Izak

      ## Types
      - [[Person]]

      ## Relationships
      - [[rel.izak-worksat-str-marine]]

      ## Properties
      - [[prop.izak-nationality]]

      ## Provenance
      - [[F-001]]

  predicate:
    description: A reusable relationship type.
    frontmatter:
      required:
        - id
        - type: Predicate
        - label
      optional:
        - normalized
        - inverse
    body_sections:
      - name: Meaning
        content: textual description
      - name: Inverse
        content: reference to inverse predicate
      - name: Used In
        content: wikilinks to relationship instances
    example: |
      ---
      id: P-worksAt
      type: Predicate
      label: works at
      normalized: works_at
      ---
      # works at

      ## Meaning
      Connects a person to an organization where they work.

      ## Used In
      - [[rel.izak-worksat-str-marine]]

  relationship_instance:
    description: A first-class edge representing a specific fact.
    frontmatter:
      required:
        - id
        - type: RelationshipInstance
        - subject
        - predicate
        - object
      optional:
        - source_fragments
        - confidence
        - status
    body_sections:
      - name: Triple
        content:
          - subject
          - predicate
          - object
      - name: Properties
        content: wikilinks to property objects
      - name: Provenance
        content: wikilinks to fragments or locations
      - name: Review
        content: confidence/status metadata
    example: |
      ---
      id: rel.izak-worksat-str-marine
      type: RelationshipInstance
      subject: [[N-izak]]
      predicate: [[P-worksAt]]
      object: [[N-str-marine]]
      source_fragments:
        - [[F-001]]
      confidence: 0.92
      ---
      # Izak —[works at]→ STR Marine

      ## Triple
      - Subject: [[N-izak]]
      - Predicate: [[P-worksAt]]
      - Object: [[N-str-marine]]

      ## Properties
      - [[prop.rel.izak-worksat-str-marine.since]]

      ## Provenance
      - [[F-001]]

  property:
    description: A first-class attribute attached to another artifact.
    frontmatter:
      required:
        - id
        - type: Property
        - applies_to
        - key
        - value
      optional:
        - source_fragments
    body_sections:
      - name: Applies To
        content: wikilink to target artifact
      - name: Property
        content:
          - key
          - value
    example: |
      ---
      id: prop.rel.izak-worksat-str-marine.since
      type: Property
      applies_to: [[rel.izak-worksat-str-marine]]
      key: since
      value: 2023
      ---
      # since = 2023

      ## Applies To
      - [[rel.izak-worksat-str-marine]]

  fragment:
    description: A precise extracted text span used as evidence.
    frontmatter:
      required:
        - id
        - type: Fragment
        - document
      optional:
        - location
        - text
        - char_start
        - char_end
    body_sections:
      - name: Source
        content: document reference
      - name: Text
        content: extracted text
    example: |
      ---
      id: F-001
      type: Fragment
      document: [[DOC-graph-concept-note]]
      text: Izak works at STR Marine since 2023.
      ---
      # Fragment F-001

      ## Text
      Izak works at STR Marine since 2023.

  source_document:
    description: Represents a source document.
    frontmatter:
      required:
        - id
        - type: SourceDocument
        - label
      optional:
        - path
        - source_type
    body_sections: []
    example: |
      ---
      id: DOC-graph-concept-note
      type: SourceDocument
      label: Graph Concept Note
      ---
      # Graph Concept Note

  source_location:
    description: Represents a structural position within a document.
    frontmatter:
      required:
        - id
        - type: SourceLocation
        - document
      optional:
        - location_type
        - section_label
        - block_index
    body_sections: []
    example: |
      ---
      id: LOC-doc-sec-2-p-3
      type: SourceLocation
      document: [[DOC-graph-concept-note]]
      location_type: paragraph
      ---
      # Location

linking_rules:
  - all_references_use_wikilinks: true
  - frontmatter_links_must_use_ids: true
  - body_links_should_be_human_readable: true
  - bidirectional_links_recommended: true

identity_rules:
  - ids_must_be_unique: true
  - ids_must_be_stable: true
  - labels_are_mutable: true
  - aliases_optional: true

provenance_rules:
  - artifacts_should_link_to_fragments: true
  - fragments_should_link_to_locations: true
  - locations_should_link_to_documents: true
  - minimal_fallback_allowed: document_only

serialization_invariants:
  - every_file_has_frontmatter: true
  - every_file_has_type: true
  - every_relationship_has_subject_predicate_object: true
  - every_property_has_key_value: true
  - graph_reconstructable_from_markdown: true