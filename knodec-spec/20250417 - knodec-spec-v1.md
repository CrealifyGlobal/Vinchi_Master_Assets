
# Knodec Language Specification — Version 1.0 (Draft)

*A semantic modeling language for structured, computable knowledge*

---

## SECTION 1: Introduction

### 1.1 What is Knodec?
**Knodec** is a human-readable, machine-readable semantic language used to define, structure, relate, and describe knowledge in the form of classes, relationships, properties, and instances. It is designed to serve as the canonical knowledge representation layer for semantic operating systems.

### 1.2 Design Philosophy
- Readable by humans (easy to author, inspect, and interpret)
- Executable by machines (parseable, inferable, translatable)
- Standard-compliant, leveraging:
  - OWL / Description Logic for formal structure
  - SKOS for conceptual hierarchies
  - Dublin Core for metadata and content description
  - RDF Turtle for serialization and triple-graph compatibility
- Modular and extensible for domain-specific applications

### 1.3 Primary Use Cases
- Structuring organizational knowledge
- Designing computable ontologies
- Automating reasoning and validation
- Indexing, querying, and retrieving semantically tagged content
- Translating knowledge into vector embeddings and execution graphs

---

## SECTION 2: Syntax Overview

### 2.1 File Format
Knodec is expressed in a Turtle-based DSL using prefixes, URIs, and triple syntax.

```
@prefix knodec: <http://knodec.org/schema#> .
@prefix skos:   <http://www.w3.org/2004/02/skos/core#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:   <http://www.w3.org/2000/01/rdf-schema#> .
```

### 2.2 Basic Triple Syntax
Each statement is a subject–predicate–object triple:

```
knodec:StrategicObjective a skos:Concept ;
  skos:prefLabel "Strategic Objective"@en ;
  skos:broader knodec:OrganizationalConcept ;
  knodec:measuredBy knodec:KPI .
```

---

## SECTION 3: Core Concepts

### 3.1 Classes
```
knodec:KPI a owl:Class ;
  rdfs:label "Key Performance Indicator"@en ;
  rdfs:subClassOf knodec:Metric .
```

### 3.2 Individuals
```
knodec:Q2RevenueGrowth a knodec:KPI ;
  dct:title "Q2 Revenue Growth" ;
  knodec:tracks knodec:RevenueObjective .
```

### 3.3 Properties

- Object properties:
```
knodec:measuredBy a owl:ObjectProperty ;
  rdfs:domain knodec:StrategicObjective ;
  rdfs:range knodec:KPI .
```

- Data properties:
```
knodec:targetValue a owl:DatatypeProperty ;
  rdfs:domain knodec:KPI ;
  rdfs:range xsd:decimal .
```

### 3.4 SKOS Terms
```
knodec:OperationalVision a skos:Concept ;
  skos:broader knodec:Vision ;
  skos:prefLabel "Operational Vision"@en .
```

### 3.5 Dublin Core Terms
```
knodec:InitiativeX dct:title "Digital Platform Upgrade" ;
  dct:creator "Vinchi Strategy Team" ;
  dct:created "2025-01-10"^^xsd:date .
```

---

## SECTION 4: Procedural Use for LLMs

### 4.1 Instructions for Structuring Content
1. Identify the semantic entities
2. Map entities to Knodec Classes or SKOS Concepts
3. Relate them using Knodec or standard properties
4. Add descriptions using Dublin Core terms
5. Ensure triples follow the Turtle format

### 4.2 Example Input + Output

Unstructured Input:
> “Our Q2 goal is to grow revenue by 10%, tracked via monthly sales reports.”

Structured Output:
```
knodec:Q2RevenueGrowth a knodec:KPI ;
  dct:title "Q2 Revenue Growth" ;
  knodec:targetValue "10"^^xsd:decimal ;
  knodec:measuredBy knodec:MonthlySalesReports ;
  skos:note "Tracks Q2 goal to increase revenue"@en .
```

---

## SECTION 5: Serialization and Integration

- Native: Turtle
- Also compatible with: JSON-LD, RDF/XML, N-Triples

---

## SECTION 6: Validation Rules (Draft)

- Classes must be defined before use
- All URIs must be fully qualified or prefixed
- Only use defined properties unless using `rdfs:seeAlso`
- Datatype properties must use correct `xsd` types

---

## SECTION 7: Planned Extensions

- Shape definitions via SHACL or similar
- Code generation (Knodec to Python/JSON)
- Content tagging pipeline
- Integration with vector embeddings, agents
- Project-specific modules (e.g. `vinchi:`, `projectX:`)

