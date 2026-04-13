metadata:
  id: "<Unique identifier for the prompt>"
  version: "<Version number (e.g., v3.0)>"
  author: "<Your name or team name>"
  last_updated: "<YYYY-MM-DD>"
  tags:
    - "<Keywords or categories for this task>"
  intended_audience: "<Who will use the output?>"
  use_case: "<Specific scenario where this prompt is applied>"
  parent_framework: "<Link to methodology, ISO standard, or project phase>"

context:
  description: |
    "<High-level explanation of the task. What is the assistant expected
    to do? Why is this task important? Provide enough background to ensure
    understanding of the broader purpose.>"
  source_material: |
    "<Specify the exact content the assistant should work with. 
    The assistant must first ask the user: 
    'What source content do you want me to use for this prompt?' 
    before execution.>"
  clarifications: |
    "<Should the assistant ask for clarifications if content is ambiguous?>"
  multiple_sources: "<true/false — can the assistant combine several sources?>"

task_definition:
  objective: |
    "<Define the clear objective of the task. Example:
    'Summarize input documents into key recommendations for executives'>"
  scope:
    mandatory: |
      "<What must be included — non-negotiable items>"
    optional: |
      "<What may be included if present, but not required>"
    out_of_scope: |
      "<Explicit exclusions to prevent scope creep>"
  constraints: |
    - "Do not generate content unless all mandatory inputs are present."
    - "If required inputs are missing, pause and request them from the user."
    - "Never fabricate results, data, or placeholders."
    - "Follow formatting and length constraints (e.g., max 800 words)."
  success_criteria: |
    "<How to evaluate a good output. Example: 'Covers all key points,
    concise, structured logically, aligned with tone'>"

instructions:
  steps:
    - step: 1
      detail: |
        "Ask the user to confirm or provide the source content 
        before starting any processing."
    - step: 2
      detail: |
        "Check if all mandatory inputs are provided. 
        If not, stop immediately and request them from the user."
    - step: 3
      detail: |
        "Only after inputs are complete, analyze the content, 
        extracting themes, data, or key points as required."
    - step: 4
      detail: |
        "Apply the requested transformation, analysis, or synthesis 
        according to the task objective."
    - step: 5
      detail: |
        "Validate the draft against constraints and success criteria."
    - step: 6
      detail: |
        "Present draft to the user, collect feedback, and refine if needed."

output_requirements:
  format: |
    "<Output format expected. Example: 'Markdown report', 'YAML structure'>"
  tone: |
    "<Desired writing style. Example: 'Formal and neutral', 'Concise and practical'>"
  structure: |
    "<Any required output structure (headings, sections, lists, tables, etc.).>"
  length: "<Short, medium, long>"
  examples: |
    "<Provide a short filled example for clarity, but clearly mark it 
    as 'illustrative only' — never substitute for missing input data.>"

refinement_guidance:
  review_criteria: |
    - "Check that all mandatory inputs were provided before output was generated."
    - "Verify no placeholders or fabricated content were inserted."
    - "Ensure output structure matches constraints and objectives."
  iteration_notes: |
    "If output attempts to guess missing inputs, update constraints 
    and instructions to strengthen the no-fabrication safeguard."
  feedback_loop: |
    "Assistant must confirm with the user that inputs are complete 
    before generating final drafts."
