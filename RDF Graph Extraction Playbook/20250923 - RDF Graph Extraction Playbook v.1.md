# RDF Graph Extraction Playbook — v1 (Upload-First, Document-by-Document)

**What this does**  
Turn any uploaded document (PDF, DOCX, TXT) into a standards-compliant RDF graph (Turtle + JSON-LD) with traceable provenance and consistent IRIs — plus a **single-file knowledge pack** you can drop into most chat assistants.

**What you’ll get**

- **Open W3C-aligned graphs**: Schema.org + DCT + PROV (+ SKOS)
    
- **Traceable provenance**: every node links back to a **source** node (hash-based)
    
- **Consistent IRIs**: strict slug rules; normalized mailto:, dates, etc.
    
- **Two graph output modes**: **Standalone** and **Appendable**
    
- **RAG Pack** (Step 8): a single JSON/JSONC file with **R2** assistant instructions
    

> **Flow:** Step 0 (config) → Step 1 (ingest) → Step 2 (entities) → Step 3 (relationships/roles) → Step 4 (normalize/dedup) → Step 5 (generate outputs) → Step 6 (validate) → Step 7 (next) → **Step 8 (package KB)**.

---

## Step 0 — Start Session & Global Config

**You say (copy/paste):**

`You are an RDF data modeler. We will process uploaded documents (PDF, DOCX, TXT) one by one—no text pasting.  Use Schema.org + DCT + PROV (+ SKOS), and schema:Role for employments. Confirm or adjust CONFIG, then wait for Step 1.  CONFIG (JSON): {   "document_mode": true,   "accepted_types": ["pdf","docx","txt"],   "base_iri": "https://example.org/id/",   "prefixes": {     "schema": "https://schema.org/",     "dct": "http://purl.org/dc/terms/",     "prov": "http://www.w3.org/ns/prov#",     "foaf": "http://xmlns.com/foaf/0.1/",     "skos": "http://www.w3.org/2004/02/skos/core#"   },   "slug_rules": "lowercase, ascii-only, hyphen-separated",   "date_format": "YYYY-MM-DD",   "role_modeling": "Use schema:Role for Person↔Organization employments",   "provenance": "Attach prov:wasDerivedFrom to all nodes; create a source schema:CreativeWork per document",   "outputs": ["turtle","json-ld"],   "output_mode": ["standalone","appendable"],   "validate": true }`

**Assistant does:**  
Confirms config; remains idle until Step 1.

---

## Step 1 — Upload & Ingestion Summary

**You say:**  
`STEP 1 — UPLOAD` + “I am uploading the next document. Please ingest it and report: file name, media type, page count (if PDF), char count extracted, a hash-based **source_id**, and any extraction warnings.”

**Assistant does:**

- Extracts text.
    
- Computes **source_id** = `src-<sha256(text)[:12]>`.
    
- Reports: file name, media type, page count/length, char count, **source_id**, warnings (OCR, missing text, etc.).
    

---

## Step 2 — Proposed Entities (You Approve/Edit)

**Assistant does:**  
Proposes a **flat list** of entities detected (id, type, props). (No relationships yet.)

**You reply (exact shape):**

`STEP 2 — ENTITIES (APPROVED) [   {     "id": "person/alice-loubser",     "type": "schema:Person",     "props": { "schema:name": "Alice Loubser", "schema:email": "mailto:alice@omni.io" }   },   {     "id": "org/omni-io",     "type": "schema:Organization",     "props": { "schema:name": "Omni.io" }   },   {     "id": "place/amsterdam",     "type": "schema:Place",     "props": { "schema:name": "Amsterdam" }   } ]`

> IDs follow the slug rules. E-mails must be `mailto:`. Dates must be ISO (YYYY-MM-DD).

---

## Step 3 — Relationships & Roles

**Assistant does:**  
Proposes **edges** and **roles** using your approved entities:

- Non-employment links as edges with `pred` (e.g., `schema:homeLocation`, `schema:memberOf`, `schema:spatialCoverage`, `schema:mentions`, `schema:about`).
    
- Employment links via **schema:Role** nodes (preferred), e.g.:
    
    `{   "id": "role/cco-2022-03-12",   "type": "schema:Role",   "props": {     "schema:member": "person/alice-loubser",     "schema:affiliation": "org/omni-io",     "schema:roleName": "Chief Commercial Officer",     "schema:startDate": "2022-03-12",     "schema:location": "place/amsterdam"   } }`
    

**You reply (approve/fix):**

`STEP 3 — RELATIONSHIPS {   "edges": [     { "from": "person/alice-loubser", "to": "place/amsterdam", "pred": "schema:homeLocation" }   ],   "roles": [     {       "id": "role/cco-2022-03-12",       "type": "schema:Role",       "props": { "schema:member": "person/alice-loubser", "schema:affiliation": "org/omni-io", "schema:roleName": "Chief Commercial Officer", "schema:startDate": "2022-03-12", "schema:location": "place/amsterdam" }     }   ] }`

---

## Step 4 — Normalization & De-dup

**You say:**

`STEP 4 — NORMALIZATION {   "confirm_slugging": true,   "confirm_dates": true,   "confirm_mailto": true,   "merge_duplicates": [],   "notes": "Normalized per config." }`

**Assistant does:**

- Enforces slug rules (`lowercase-ascii-hyphen`).
    
- Normalizes dates (ISO-8601) and `mailto:`.
    
- Merges duplicates per your list; returns a brief delta report.
    

---

## Step 5 — Generate Graph Outputs

**You say:**  
`STEP 5 — GENERATE` + “Produce both: (A) Standalone Turtle & JSON-LD; (B) Appendable blocks (Turtle: triples only, JSON-LD: `@graph` only). Include a brief summary and the **source node IRI**.”

**Assistant does:**

- **Standalone**: includes `@base`, `@prefix`, and a **source** node (`schema:CreativeWork`) with `prov:wasDerivedFrom` attached to every node.
    
- **Appendable**: triples only (no prefixes/base) and JSON-LD `@graph` only.
    
- Reports node/edge counts + **source IRI** (`{base_iri}source/{source_id}`).
    

---

## Step 6 — Validate (SHACL, optional but recommended)

**You say:**  
`STEP 6 — VALIDATE` + “Provide minimal SHACL shapes for `schema:Person` and `schema:Role`. Validate and list violations + suggested fixes.”

**Assistant does:**

- Emits minimal SHACL shapes.
    
- Runs validation against current document’s graph.
    
- Lists violations + suggested fixes (missing name, email format, dates, etc.).
    

---

## Step 7 — Next Document (Rinse & Repeat)

**You say:**  
`STEP 7 — NEXT` + “I will upload the next document now. Repeat Steps 1–6 and keep IRIs consistent across documents.”

**Assistant does:**

- Loops back to Step 1; preserves IRIs across documents (do not re-mint if same real-world entity).
    

---

## Step 8 — Package Single-File Knowledge Base (“RAG Pack”) + R2 Instructions _(New)_

**Purpose**  
Produce a **one-file, drop-in knowledge pack** suitable for most chat assistants without a database/server. Includes **R2** assistant rules, distilled facts, passages, entities, optional Q&A seed, and embedded RDF (JSON-LD + Turtle).

**You say (copy/paste):**

`STEP 8 — PACKAGE KB Please generate a single-file knowledge pack for this document using the config below.  CONFIG (JSON): {   "filename": "kb-{slug}.json",   "format": "jsonc",   "sections": {     "include_instructions": true,     "include_facts": true,     "include_passages": true,     "include_entities": true,     "include_qna_seed": true,     "include_rdf_jsonld": true,     "include_rdf_turtle": true   },   "comment_style": "block-headers",   "provenance": {     "include_source_list": true,     "source_label": "source: {file_name} ({publisher})"   },   "instructions_id": "R2",   "instructions_text": "You are a helpful assistant with direct access to an embedded single-file knowledge base. Always answer using ONLY the information in this file unless the user explicitly asks you to go beyond it. If a question is outside scope, say so. Prefer concise bullets. Ground claims in 'facts' and 'passages'. If numbers are requested, quote them if present; otherwise say the figure is not provided. Use 'entities' to name organizations/places with their types. You may reference the embedded RDF. When asked for provenance, cite the configured source label.",   "entity_emit_policy": "ids-and-names",   "passage_granularity": "short",   "qna_seed_style": "neutral",   "validate_schema": true }`

**Assistant does:**

- Generates a single JSON/JSONC file shaped like this (excerpt):
    
    - `schema_version`, `kb_id`, `title`, `created_utc`
        
    - `sources` (with `display_label` for provenance)
        
    - `instructions_id`, `instructions_for_assistant` (**R2**)
        
    - `facts`, `passages`, `entities`, `qna_seed`
        
    - `rdf.jsonld` and `rdf.turtle`
        
- Returns the **entire file inline** and/or as an attachment.
    
- Ensures presence of required top-level keys; reports any schema issues.
    

> **Tip:** Pick `format:"jsonc"` to keep explanatory `// comments` as section headers that human readers love (machines will often accept or can easily strip comments).

---

# Master File Strategy (No Format Headaches)

### A. Turtle Master (`master.ttl`)

- **At top (once):** all `@prefix` and `@base`.
    
- **Per document:** paste **Appendable Turtle** blocks (triples only).
    
- Use tools like `riot` (Apache Jena) or `rapper` to pretty-print/validate.
    

### B. JSON-LD Master (`master.jsonld`)

- Keep one top-level object with a single `@context` and an accumulating `@graph` array.
    
- **Per document:** append the **Appendable JSON-LD** `@graph` items.
    

---

# Quick “Autopilot” (Document Mode + Step 8)

**You say (one-shot):**

`Autopilot (Document Mode + Step 8): - I will upload one document now. Extract text, create a hash-based source_id, propose entities and relationships, ask me for approvals once, then output:   A) Standalone & Appendable Turtle/JSON-LD,   B) A single-file knowledge pack (Step 8) with R2 instructions, facts, passages, entities, optional Q&A seed, and embedded RDF,   C) A 5-line summary. - Use @base https://example.org/id/ and the Step 0 config. - After output, ask if I want to process the next document.`

---

## Appendices (Reference)

### A. Prefixes

`@base <https://example.org/id/> . @prefix schema: <https://schema.org/> . @prefix dct:    <http://purl.org/dc/terms/> . @prefix prov:   <http://www.w3.org/ns/prov#> . @prefix foaf:   <http://xmlns.com/foaf/0.1/> . @prefix skos:   <http://www.w3.org/2004/02/skos/core#> .`

### B. Slug Rules

- lowercase
    
- ASCII only
    
- words separated by hyphens
    
- categories (`person/`, `org/`, `place/`, `role/`, `creativework/`, `concept/`, `hotspot/`, `program/`, `cluster/`, `source/`)
    

### C. Date & Contact Normalization

- Dates: `YYYY-MM-DD`
    
- E-mail: `mailto:someone@example.com`
    
- URLs: absolute, `https://…`
    

### D. Minimal SHACL (for Step 6)

`@prefix sh: <http://www.w3.org/ns/shacl#> . @prefix schema: <https://schema.org/> . @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .  ex:PersonShape a sh:NodeShape ;   sh:targetClass schema:Person ;   sh:property [ sh:path schema:name ; sh:minCount 1 ; sh:datatype xsd:string ] .  ex:RoleShape a sh:NodeShape ;   sh:targetClass schema:Role ;   sh:property [ sh:path schema:member ; sh:minCount 1 ] ;   sh:property [ sh:path schema:affiliation ; sh:minCount 1 ] ;   sh:property [ sh:path schema:roleName ; sh:minCount 1 ; sh:datatype xsd:string ] .`

### E. Error Handling (Typical)

- **No text extracted / scanned PDF:** report OCR warning; optionally retry with OCR.
    
- **Ambiguous entities:** propose multiple candidates; ask for approval in Step 2.
    
- **Duplicate detections:** list candidates in Step 4; request merge confirmations.
    
- **Invalid dates/emails/IRIs:** flag in Step 6 with suggested fixes.