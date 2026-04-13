generator_name: "Semantic Political Document Comparison"
version: "1.0"
description: >
  A comprehensive generator for comparing two political documents using semantic modeling, divergence analysis, alignment mapping,
  strategic recommendation, and gap-closing strategy development. Designed for policy analysts, strategists, and researchers
  working with ideologically distinct texts.

required_inputs:
  - name: "primary_document"
    description: "The first political document to be analyzed (e.g., party platform, policy proposal)."
    format: "markdown | plaintext | PDF"
    source: "manual"
  - name: "secondary_document"
    description: "The second political document for comparison (e.g., motion, amendment, counter-policy)."
    format: "markdown | plaintext | PDF"
    source: "manual"

tasks:

  - task_id: "1.1"
    task_name: "Semantic Parse: Primary Document"
    goal: "Extract themes, values, logic, and stakeholder assumptions."
    instructions: |
      - Read the document section by section.
      - Extract key themes (e.g., cost, control, values).
      - Identify claims, proposed solutions, and assumptions.
      - Summarize the tone and framing (emotional, rational, moral).
    output: "semantic_model_primary.yaml"

  - task_id: "1.2"
    task_name: "Semantic Parse: Secondary Document"
    goal: "Repeat semantic extraction using the same categories."
    instructions: |
      - Apply the same methodology used in Task 1.1.
      - Ensure semantic frames are comparable.
      - Focus on claims, intents, evidence, and mechanisms.
    output: "semantic_model_secondary.yaml"

  - task_id: "2.1"
    task_name: "Analyze Semantic Divergences"
    goal: "Map out core differences between documents."
    instructions: |
      - Compare each document’s claims, justifications, and frames.
      - Categorize each divergence as:
        - Normative (values or principles)
        - Epistemic (truth claims or causes)
        - Procedural (how actions should occur)
        - Semantic (different meanings of same terms)
    output: "divergence_typology_map.yaml"

  - task_id: "2.2"
    task_name: "Map Alignment Opportunities"
    goal: "Identify shared goals and partially aligned frames."
    instructions: |
      - Highlight any shared concerns (e.g., affordability, effectiveness).
      - Document soft alignments or linguistic parallels.
      - Include all potential areas for reframing or negotiation.
    output: "alignment_matrix.yaml"

  - task_id: "3.1"
    task_name: "Generate Recommendation"
    goal: "Advise whether primary actor should support or oppose the secondary proposal."
    instructions: |
      - Use alignment and divergence data.
      - Evaluate based on ideological consistency and strategic risk.
      - State position (Support / Oppose / Modify).
    output: "support_recommendation.md"

  - task_id: "3.2"
    task_name: "Trace Recommendation to Evidence"
    goal: "Justify recommendation using precise analysis links."
    instructions: |
      - Reference divergence and alignment data explicitly.
      - Show where and why support would undermine or reinforce core narratives.
    output: "recommendation_justification_map.md"

  - task_id: "4.1"
    task_name: "Develop Bridging Strategy"
    goal: "Identify how positions could be brought closer together."
    instructions: |
      - Suggest framing alternatives, structural adjustments, or conditional support clauses.
      - Make strategy politically coherent with both positions.
    output: "gap_closing_strategy.md"

  - task_id: "4.2"
    task_name: "Write Action Plan"
    goal: "Describe specific steps to execute the bridging strategy."
    instructions: |
      - Include legislative actions, communication plans, stakeholder consultations.
      - Ensure clarity of sequence, responsible parties, and measurable outcomes.
    output: "bridging_action_plan.md"
