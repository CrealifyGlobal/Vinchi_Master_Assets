# RDF Graph Extraction Playbook — **Document-by-Document (Upload-First)**

_(Turn uploaded documents into clean Turtle & JSON-LD with a chat-guided wizard—no copy-pasting text.)_

## What we’re doing

You’ll upload one document at a time (PDF, DOCX, or TXT). I’ll extract the text for you, propose entities and relationships, and then generate **standards-compliant RDF** as **Turtle** and **JSON-LD**. You never paste raw text into prompts—the assistant reads the file you upload.

## What you’ll achieve

- Open **W3C-aligned** graph outputs (Schema.org, DCT, PROV).
    
- **Traceable provenance** from each triple back to its source document.
    
- Consistent, reusable **IRIs** and normalized data (dates, emails, etc.).
    
- Two output modes: **Standalone** (ready to save), and **Appendable** (safe to paste into a master TTL/JSON-LD).
    

---

## How to use (exact prompts to copy-paste)

> You’ll run this wizard **per document**. Upload a file when asked. I’ll confirm what I extracted before graphing.

### Step 0 — Start session & config

**You say:**

`You are an RDF data modeler. We will process uploaded documents (PDF, DOCX, TXT) one by one—no text pasting. Use Schema.org + DCT + PROV, with Roles for employments. Confirm or adjust the CONFIG, then wait for Step 1.  CONFIG (JSON): {   "document_mode": true,   "accepted_types": ["pdf", "docx", "txt"],   "base_iri": "https://example.org/id/",   "prefixes": {     "schema": "https://schema.org/",     "dct": "http://purl.org/dc/terms/",     "prov": "http://www.w3.org/ns/prov#",     "foaf": "http://xmlns.com/foaf/0.1/",     "skos": "http://www.w3.org/2004/02/skos/core#"   },   "slug_rules": "lowercase, ascii-only, hyphen-separated",   "date_format": "YYYY-MM-DD",   "role_modeling": "Use schema:Role for Person↔Organization employments",   "provenance": "Attach prov:wasDerivedFrom to all nodes; create a source CreativeWork per document",   "outputs": ["turtle", "json-ld"],   "output_mode": ["standalone", "appendable"],   "validate": true }`

### Step 1 — Upload a document

**You say:**

`STEP 1 — UPLOAD I am uploading the next document. Please ingest it and report: - file name, media type, page count (if PDF), character count extracted, - an MD5/SHA hash-based source_id you will use for the source node, - any extraction warnings (e.g., scanned PDF/low text quality).`

> **Action:** Upload your file via the chat UI.  
> **Assistant does:** Extracts text, creates a `source_id` from a hash of the file or its text, and reports the summary.

### Step 2 — Proposed entities (you approve/edit)

**Assistant** proposes a list of detected entities with types & properties.  
**You reply** with an edited/approved list in this exact shape:

`STEP 2 — ENTITIES (APPROVED) [   {     "id": "person/alice-loubser",     "type": "schema:Person",     "props": {       "schema:name": "Alice Loubser",       "schema:email": "mailto:alice@omni.io"     }   },   {     "id": "org/omni-io",     "type": "schema:Organization",     "props": { "schema:name": "Omni.io" }   },   {     "id": "place/amsterdam",     "type": "schema:Place",     "props": { "schema:name": "Amsterdam" }   } ]`

### Step 3 — Relationships & roles (prefer Role for employments)

**Assistant** proposes edges and roles.  
**You reply** with corrections/approval:

`STEP 3 — RELATIONSHIPS {   "edges": [     { "from": "person/alice-loubser", "to": "place/amsterdam", "pred": "schema:homeLocation" }   ],   "roles": [     {       "id": "role/cco-2022-03-12",       "type": "schema:Role",       "props": {         "schema:member": "person/alice-loubser",         "schema:affiliation": "org/omni-io",         "schema:roleName": "Chief Commercial Officer",         "schema:startDate": "2022-03-12",         "schema:location": "place/amsterdam"       }     }   ] }`

### Step 4 — Normalization & de-dup

**You say:**

`STEP 4 — NORMALIZATION {   "confirm_slugging": true,   "confirm_dates": true,   "confirm_mailto": true,   "merge_duplicates": [],   "notes": "Normalized per config." }`

### Step 5 — Generate outputs for this document

**You say:**

`STEP 5 — GENERATE Produce both: A) Standalone Turtle (.ttl) with prefixes and @base, and JSON-LD with @context. B) Appendable blocks:    - Turtle: triples only (no @prefix/@base), ready to paste into a master TTL.    - JSON-LD: an object with "@graph" only, ready to merge into a master JSON-LD file. Include a brief summary of nodes/edges created and the source node IRI.`

### Step 6 — Validate (optional but recommended)

**You say:**

`STEP 6 — VALIDATE Provide minimal SHACL shapes for schema:Person and schema:Role. Validate the current document’s graph; list violations and suggested fixes.`

### Step 7 — Next document (rinse & repeat)

**You say:**

`STEP 7 — NEXT I will upload the next document now. Repeat Steps 1–6 and keep IRIs consistent across documents.`

# Step 8 — Package Single-File Knowledge Base (“RAG Pack”) + R2 Instructions

**Purpose:**  
Create a one-file, drop-in knowledge pack that any chat assistant can load as custom knowledge. It embeds (a) distilled facts/passages, (b) entities, (c) RDF (JSON-LD + Turtle), (d) optional Q&A seeds, and (e) an **R2 instruction block** that tells the assistant exactly how to answer using only this file.

**You say:**

`STEP 8 — PACKAGE KB Please generate a single-file knowledge pack for this document using the config below. CONFIG (JSON): {   "filename": "kb-{slug}.json",                   // Output file name; {slug} = doc slug   "format": "jsonc",                              // "json" (strict) or "jsonc" (JSON with //comments)   "sections": {     "include_instructions": true,                 // Embed assistant instructions (R2)     "include_facts": true,                        // Distilled bullet facts     "include_passages": true,                     // Retrieval-friendly text chunks     "include_entities": true,                     // Key orgs/places with types     "include_qna_seed": true,                     // Optional starter Q&A     "include_rdf_jsonld": true,                   // JSON-LD graph     "include_rdf_turtle": true                    // Turtle triples   },   "comment_style": "block-headers",               // Adds clear // ===== headers to each section   "provenance": {     "include_source_list": true,     "source_label": "source: {file_name} (InnovationQuarter)" // Adjust publisher per doc   },   "instructions_id": "R2",                        // Identifier for the instruction block   "instructions_text": "You are a helpful assistant with direct access to an embedded single-file knowledge base. Always answer using ONLY the information in this file unless the user explicitly asks you to go beyond it. If a question is outside scope, say so. Prefer concise bullets. Ground claims in 'facts' and 'passages'. If numbers are requested, quote them if present; otherwise say the figure is not provided. Use 'entities' to name organizations/places with their types. You may reference the embedded RDF. When asked for provenance, cite the configured source label.",   "entity_emit_policy": "ids-and-names",          // "ids-only" | "ids-and-names" | "full" (with types)   "passage_granularity": "short",                 // "short" (~1–3 sentences) | "medium" | "long"   "qna_seed_style": "neutral",                    // "neutral" | "sales" | "technical"   "validate_schema": true                         // Ensure required top-level keys exist }`

**Assistant does:**

1. Builds a compact **RAG Pack** JSON/JSONC file with section headers and inline comments (if `format=jsonc`).
    
2. Inserts the **R2** instruction block verbatim and tags it with `instructions_id`.
    
3. Fills sections from prior steps:
    
    - `facts` & `passages`: distilled from this document’s extracted text.
        
    - `entities`: the approved Step 2 entity set (IDs, names, and types).
        
    - `rdf.jsonld` & `rdf.turtle`: the Step 5 graph outputs.
        
    - `qna_seed`: 3–6 likely question/answer pairs grounded in this doc.
        
4. Adds a `sources` list with file name, media type, and a provenance note.
    
5. Validates structure (`schema_version`, `kb_id`, `title`, `created_utc`, `sources`, `instructions_for_assistant` present).
    
6. Returns the **entire file inline** in the chat (and, if supported, as a downloadable attachment).
    

---

## Output shape (example, JSONC with explanatory headers)

`{   // ============================================================   //  METADATA   //  - Identifies this file as a self-contained knowledge pack   // ============================================================   "schema_version": "1.0",   "kb_id": "kb-iq-infographic-maritime-eng",   "title": "West Holland Maritime & Offshore — Single-File Knowledge Base",   "created_utc": "2025-09-23T00:00:00Z",    // ============================================================   //  SOURCES (PROVENANCE)   //  - Where this knowledge was distilled from   // ============================================================   "sources": [     {       "source_id": "src-4b1c9f18d9a2",       "title": "IQ-infographic-Maritime-ENG.pdf",       "publisher": "InnovationQuarter",       "media_type": "application/pdf",       "provenance_note": "All facts distilled from the source infographic.",       "display_label": "source: IQ-infographic-Maritime-ENG.pdf (InnovationQuarter)"     }   ],    // ============================================================   //  R2: ASSISTANT INSTRUCTIONS (SYSTEM-STYLE RULES)   //  - Copy changes here if you want a different behavior profile   // ============================================================   "instructions_id": "R2",   "instructions_for_assistant": "You are a helpful assistant with direct access to an embedded single-file knowledge base. Always answer using ONLY the information in this file unless the user explicitly asks you to go beyond it. If a question is outside scope, say so. Prefer concise bullets. Ground claims in 'facts' and 'passages'. If numbers are requested, quote them if present; otherwise say the figure is not provided. Use 'entities' to name organizations/places with their types. You may reference the embedded RDF. When asked for provenance, cite the configured source label.",    // ============================================================   //  FACTS (BULLETS)   //  - Canonical short items; optimized for quick grounding   // ============================================================   "facts": [     "West Holland (greater Rotterdam–The Hague–Delft) is a hotspot for the Maritime & Offshore industry.",     "The Maritime Delta is described as the largest maritime hotspot in Europe and spans Hoek van Holland to Drechtsteden and Werkendam.",     "Rotterdam is the largest port in Europe, spanning 12,500 hectares, handling ~465 million tonnes and ~7.3 million containers annually, with ~30,000 seagoing and 110,000 inland vessels per year.",     "The regional maritime cluster covers design, shipbuilding, maintenance, asset owners, suppliers, and maritime services.",     "Innovation hotspots include RDM Rotterdam, YES!Delft, Buccaneer Delft, and CIC Rotterdam; programs include PortXL, Port Innovation Lab, Our Oceans Challenge, and Buccaneer Development Program.",     "Showcase companies cited: Allseas (Pioneering Spirit), Van Oord (Palm Jumeirah dredging), Ampelmann (offshore access systems).",     "Education & research: Delft University of Technology, Erasmus University Rotterdam, Rotterdam University of Applied Sciences, The Hague University of Applied Sciences, STC – Netherlands Maritime University, and MARIN.",     "Training: numerous offshore safety and skills programs (e.g., STC-KNRM, DOB-Academy)."   ],    // ============================================================   //  PASSAGES (CHUNKS)   //  - Short paragraphs that preserve context for answers   // ============================================================   "passages": [     { "id": "p1", "text": "10 reasons why West Holland is a maritime & offshore hotspot; home to the Maritime Delta in the greater Rotterdam area with a diverse cluster and strong innovation culture." },     { "id": "p2", "text": "Rotterdam: largest port in Europe (12,500 ha). Annual throughput ~465 million tonnes and 7.3 million TEU; ~30k seagoing and 110k inland vessels." },     { "id": "p3", "text": "Innovation hotspots: RDM Rotterdam, YES!Delft, Buccaneer Delft, CIC Rotterdam. Programs: PortXL, Port Innovation Lab, Our Oceans Challenge, Buccaneer Development Program." },     { "id": "p4", "text": "Success stories: Allseas Pioneering Spirit (single-lift platform install/removal); Van Oord Palm Jumeirah dredging; Ampelmann motion-compensated offshore access." },     { "id": "p5", "text": "Education & research: TU Delft, Erasmus, Rotterdam UAS, The Hague UAS, STC – Netherlands Maritime University, MARIN. Training providers include STC-KNRM and DOB-Academy." }   ],    // ============================================================   //  ENTITIES (STRUCTURED REFERENCES)   //  - Helpful for naming and type-aware answers   // ============================================================   "entities": [     { "id": "org/innovationquarter", "type": "schema:Organization", "name": "InnovationQuarter" },     { "id": "cluster/maritime-delta", "type": "schema:Organization", "name": "Maritime Delta" },     { "id": "place/west-holland", "type": "schema:Place", "name": "West Holland" },     { "id": "place/rotterdam", "type": "schema:Place", "name": "Rotterdam" },     { "id": "org/allseas", "type": "schema:Organization", "name": "Allseas" },     { "id": "org/van-oord", "type": "schema:Organization", "name": "Van Oord" },     { "id": "org/ampelmann", "type": "schema:Organization", "name": "Ampelmann" }   ],    // ============================================================   //  Q&A SEED (OPTIONAL)   //  - Pre-baked Q&A to steer early responses   // ============================================================   "qna_seed": [     { "q": "What is the Maritime Delta?", "a": "A major maritime cluster in West Holland spanning Hoek van Holland to Drechtsteden and Werkendam; described as the largest maritime hotspot in Europe." },     { "q": "Why is West Holland attractive for maritime & offshore?", "a": "It combines the largest European port (Rotterdam), a diversified supply chain, strong innovation hotspots and accelerators, and world-class education and research institutions." },     { "q": "Name three innovation hotspots mentioned.", "a": "RDM Rotterdam; YES!Delft; Buccaneer Delft (also CIC Rotterdam)." },     { "q": "Give an example of a flagship project from the region.", "a": "Allseas’ Pioneering Spirit single-lift platform vessel; Van Oord’s Palm Jumeirah dredging; Ampelmann’s motion-compensated offshore access system." }   ],    // ============================================================   //  RDF GRAPH (JSON-LD + TURTLE)   //  - Full fidelity semantic graph for advanced use   // ============================================================   "rdf": {     "jsonld": {       "@context": {         "@base": "https://example.org/id/",         "schema": "https://schema.org/",         "dct": "http://purl.org/dc/terms/",         "prov": "http://www.w3.org/ns/prov#",         "foaf": "http://xmlns.com/foaf/0.1/",         "skos": "http://www.w3.org/2004/02/skos/core#"       },       "@graph": [         {           "@id": "source/src-4b1c9f18d9a2",           "@type": "schema:CreativeWork",           "schema:name": "IQ-infographic-Maritime-ENG.pdf",           "dct:format": "application/pdf"         },         {           "@id": "creativework/iq-infographic-maritime-eng",           "@type": "schema:CreativeWork",           "schema:name": "10 Reasons Why West Holland is the Hotspot for Maritime & Offshore Industry",           "schema:creator": { "@id": "org/innovationquarter" },           "schema:inLanguage": "en",           "schema:about": { "@id": "concept/maritime-and-offshore-industry" },           "prov:wasDerivedFrom": { "@id": "source/src-4b1c9f18d9a2" }         },         { "@id": "cluster/maritime-delta", "@type": "schema:Organization", "schema:name": "Maritime Delta" },         { "@id": "org/innovationquarter", "@type": "schema:Organization", "schema:name": "InnovationQuarter" },         { "@id": "place/west-holland", "@type": "schema:Place", "schema:name": "West Holland" },         { "@id": "place/rotterdam", "@type": "schema:Place", "schema:name": "Rotterdam" },         { "@id": "place/the-hague", "@type": "schema:Place", "schema:name": "The Hague" },         { "@id": "place/delft", "@type": "schema:Place", "schema:name": "Delft" },         { "@id": "org/allseas", "@type": "schema:Organization", "schema:name": "Allseas" },         { "@id": "org/van-oord", "@type": "schema:Organization", "schema:name": "Van Oord" },         { "@id": "org/ampelmann", "@type": "schema:Organization", "schema:name": "Ampelmann" },         { "@id": "concept/maritime-and-offshore-industry", "@type": "skos:Concept", "skos:prefLabel": "Maritime & Offshore Industry" },         { "@id": "concept/additive-manufacturing", "@type": "skos:Concept", "skos:prefLabel": "Additive Manufacturing" }       ]     },     "turtle": "@base <https://example.org/id/> .\\n@prefix schema: <https://schema.org/> .\\n@prefix dct: <http://purl.org/dc/terms/> .\\n@prefix prov: <http://www.w3.org/ns/prov#> .\\n@prefix skos: <http://www.w3.org/2004/02/skos/core#> .\\n\\nsource/src-4b1c9f18d9a2 a schema:CreativeWork ;\\n  schema:name \\\"IQ-infographic-Maritime-ENG.pdf\\\" ;\\n  dct:format \\\"application/pdf\\\" .\\n\\ncreativework/iq-infographic-maritime-eng a schema:CreativeWork ;\\n  schema:name \\\"10 Reasons Why West Holland is the Hotspot for Maritime & Offshore Industry\\\" ;\\n  schema:creator org/innovationquarter ;\\n  schema:inLanguage \\\"en\\\" ;\\n  schema:about concept/maritime-and-offshore-industry ;\\n  prov:wasDerivedFrom source/src-4b1c9f18d9a2 ."   } }`

---

### “Quick Autopilot” variant including Step 8

If you prefer minimal back-and-forth for a single file, use this one-shot prompt:

`Autopilot (Document Mode + Step 8): - I will upload one document now. Extract text, create a hash-based source_id, propose entities and relationships, ask me for approvals once, then output:   A) Standalone & Appendable Turtle/JSON-LD,   B) A single-file knowledge pack (Step 8) with R2 instructions, facts, passages, entities, optional Q&A seed, and embedded RDF,   C) A 5-line summary. - Use @base https://example.org/id/ and the Step 0 config. - After output, ask if I want to process the next document.`


---

## Master file strategy (no format headaches)

**Turtle master (`master.ttl`)**

- At the very top (once): all `@prefix` declarations and `@base`.
    
- For each document: paste the **Appendable Turtle** block (triples only).
    
- If you ever need to reformat, tools like `riot` (Apache Jena) or `rapper` can pretty-print/validate.
    

**JSON-LD master (`master.jsonld`)**

- Keep one top-level object with a **single `@context`** and an accumulating **`@graph`** array.
    
- For each document: append the **Appendable JSON-LD** `@graph` items.
    

---

## Optional helper script (local, for your own pipeline)

If you prefer to **extract text locally** from files before the chat (or to replicate the same hashing and file naming):

**requirements.txt**

`rdflib==7.0.0 pdfminer.six==20231228 python-docx==1.1.2 chardet==5.2.0`

**doc_text_extract.py**

`# Usage: python doc_text_extract.py path/to/file.(pdf|docx|txt) import sys, hashlib, json, chardet from pathlib import Path  def read_txt(p: Path) -> str:     raw = p.read_bytes()     enc = chardet.detect(raw).get("encoding") or "utf-8"     return raw.decode(enc, errors="replace")  def read_docx(p: Path) -> str:     from docx import Document     doc = Document(str(p))     return "\n".join([para.text for para in doc.paragraphs])  def read_pdf(p: Path) -> str:     from pdfminer.high_level import extract_text     return extract_text(str(p))  if __name__ == "__main__":     if len(sys.argv) != 2:         print("Provide a single file path (pdf|docx|txt).")         sys.exit(1)     path = Path(sys.argv[1])     if not path.exists():         print("File not found.")         sys.exit(1)     ext = path.suffix.lower()     if ext == ".txt":         text = read_txt(path)         mtype = "text/plain"     elif ext == ".docx":         text = read_docx(path)         mtype = "application/vnd.openxmlformats-officedocument.wordprocessingml.document"     elif ext == ".pdf":         text = read_pdf(path)         mtype = "application/pdf"     else:         print("Unsupported extension.")         sys.exit(1)      h = hashlib.sha256(text.encode("utf-8", errors="replace")).hexdigest()[:12]     out = {         "file_name": path.name,         "media_type": mtype,         "chars": len(text),         "source_id": f"src-{h}",         "text_preview": text[:1000]     }     print(json.dumps(out, indent=2))`

> This script **does not** replace the chat wizard; it just mirrors the extraction summary you’ll see in Step 1 (including a stable `source_id` you can reuse).

---

## Quick “Autopilot (Document)” prompt

When you want minimal back-and-forth for a single file:

`Autopilot (Document Mode): - I will upload one document now. Extract text, create a source CreativeWork node with a hash-based source_id,   propose entities and relationships, ask me for approvals once,   then output both Standalone and Appendable Turtle/JSON-LD, plus a 5-line summary. - Use base IRI https://example.org/id/ and the config from Step 0. - After output, ask me if I want to process the next document.`

---

If you’re ready, **upload your first document** and say “Start Step 1”. I’ll ingest it and guide you through the approvals—one document at a time.