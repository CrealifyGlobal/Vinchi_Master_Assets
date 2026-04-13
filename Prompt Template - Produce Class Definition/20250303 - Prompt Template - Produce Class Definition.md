**Objective:**  
_"Define an ontology class using a structured format optimized for AI-driven reasoning, graph retrieval, and NLP-based text parsing. The class should include structured relationships, attributes, examples, rules, external ontology links, and natural language pattern detection to ensure accurate AI understanding."_

---

### **Prompt Format:**

_Use the following template to define each ontology class:_

---

## **Ontology Class Definition: [Insert Class Name]**

### **1. Class Type**

_(Choose one: Concept, Entity, Event, Process, Rule, Role, Capability, Flow, Object, Decision, State, Metric, Classification System)_

### **2. Synonyms / Alternative Labels**

_(List terms that could be used interchangeably)_

### **3. Class Description**

_"Provide a structured, precise definition of the class, highlighting its purpose, function, and how it fits within the ontology. Ensure it distinguishes itself from similar concepts and is aligned with organizational knowledge architecture."_

### **4. Subclasses (if applicable)**

_"List and briefly define the subcategories of this class."_

### **5. Object Properties (Relationships to Other Classes)**

|**Property Name**|**Domain**|**Range**|**Description**|**Relationship Strength**|
|---|---|---|---|---|
|`Property Name`|[Class]|[Related Class]|[Explain the connection between classes]|(Strong / Weak / Optional)|

### **6. Data Properties (Literal Attributes of the Class)**

|**Property Name**|**Type**|**Description**|**Importance Level**|
|---|---|---|---|
|`Attribute Name`|(String / Integer / Date / List)|[Describe what this attribute represents]|(High / Medium / Low)|

### **7. Instances (Real-world Examples)**

_"Provide at least two examples of real-world instances belonging to this class. Each instance should have a name, assigned class, and attributes."_

### **8. Rules and Axioms (Logical Constraints and Reasoning)**

_"List rules that govern this class, ensuring structured AI reasoning and avoiding hallucinations."_

### **9. Linked Ontologies**

_"Identify external ontology standards (e.g., SKOS, BFO, ArchiMate) that map to this class for interoperability."_

### **10. Inherited Properties**

_"List properties this class inherits from a broader category or superclass."_

### **11. Example Queries (AI Retrieval Optimization)**

_"Provide 3-5 example questions that should retrieve this class when searching in an AI-driven system."_

### **12. Graph Query Patterns (SPARQL / Cypher / LLM Queries)**

#### **SPARQL Query Example (For RDF triple stores)**

sparql

CopyEdit

`SELECT ?class ?related WHERE {   ?class rdf:type :[Class Name] .   ?class :[Property] ?related . }`

#### **Cypher Query Example (For Neo4j graph databases)**

cypher

CopyEdit

`MATCH (c:[Class])-[:RELATES_TO]->(r:[RelatedClass]) RETURN c, r`

#### **LLM Query Prompt Example (For hybrid RAG models)**

css

CopyEdit

`"Retrieve all [Class Name] instances related to [specific topic]."`

### **13. Pattern Recognition in Unstructured Text**

#### **Common Phrases & Sentence Starters (Indicators of this Class in Text Data)**

- `"Example sentence structures that indicate the presence of this class"`
- `"Typical phrases people use when referring to this concept"`

#### **Semantic & Contextual Indicators (Words Commonly Found in Class-Related Texts)**

- `"List words and verbs often used in this domain"`

#### **Machine Learning Heuristics (For AI Parsing This Class in Free Text)**

- `"Define NLP-based rules for AI to detect implicit references to this class in unstructured data."`

---

### **How to Use This Prompt**

1. **Replace** `[Insert Class Name]` with the class you want to define.
2. **Follow the structured sections**, filling in detailed definitions, relationships, attributes, rules, and queries.
3. **Ensure completeness** – Each section should be logically connected to enhance AI-driven understanding.
4. **Apply the same format consistently** for **every new class in the ontology** to maintain standardization.