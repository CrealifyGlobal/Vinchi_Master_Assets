generator_name: "LinkedIn Experience Entry Builder"
version: "1.0"
description: >
  This generator transforms structured inputs (existing LinkedIn content, resume data, and user notes)
  into a professional, standardized LinkedIn Experience entry consisting of a company overview,
  responsibilities section (capability-aligned), and a key achievements section.

required_inputs:
  - name: "linkedin_existing"
    description: "Current LinkedIn experience text for this role"
    format: "plaintext"
    source: "manual"
  - name: "resume_relevant"
    description: "Relevant content extracted from user’s resume"
    format: "plaintext"
    source: "manual"
  - name: "verbal_notes"
    description: "Contextual notes and role-specific insights provided by user"
    format: "plaintext"
    source: "manual"

output_format:
  type: "markdown"
  destination: "outputs/linkedin_experience_entry.md"

task_sequence:
  - 1.1.0
  - 1.2.0
  - 1.3.0
  - 2.0.0

tasks:

  - task_id: "1.1.0"
    task_name: "Extract Capability Areas"
    goal: "Identify and confirm which capabilities (competencies) the role reflects"
    context: "Supports structuring the responsibilities and aligning the tone"
    inputs:
      - name: "verbal_notes"
        format: "plaintext"
    processing_steps:
      - "Scan user notes for keywords or direct capability listings"
      - "Normalize capability terms using pre-approved labels (e.g., Strategy, PPM, GRC)"
    output:
      format: "yaml"
      structure:
        - capability_label
        - justification_snippet
      validation: "Must contain at least one capability"

  - task_id: "1.2.0"
    task_name: "Structure Responsibilities by Capability"
    goal: "Generalize role responsibilities into capability-aligned objectives"
    context: "Responsibility bullets reflect role mandates, not tasks"
    inputs:
      - name: "resume_relevant"
        format: "plaintext"
      - name: "capability_labels"
        format: "yaml"
        source: "calculated"
    processing_steps:
      - "Group input responsibilities or actions under each capability"
      - "Rephrase them as outcome-driven general mandates"
    output:
      format: "markdown"
      structure:
        - bullet_list: capability_label: responsibility_statement
      validation: "Each capability must have at least one responsibility"

  - task_id: "1.3.0"
    task_name: "Select and Refine Key Achievements"
    goal: "Compile outcome-driven, high-impact achievements that match the responsibilities"
    context: "Bullet list of quantifiable or structural outcomes"
    inputs:
      - name: "resume_relevant"
        format: "plaintext"
      - name: "verbal_notes"
        format: "plaintext"
    processing_steps:
      - "Extract unique achievements from resume and notes"
      - "Remove any bullets already implied by responsibilities"
      - "Refine language for clarity, remove formatting (bold, hyphens)"
    output:
      format: "markdown"
      structure:
        - bullet_list: achievement_statement
      validation: "8–10 high-value achievements maximum"

  - task_id: "2.0.0"
    task_name: "Compile LinkedIn Experience Entry"
    goal: "Assemble final entry with consistent formatting and clean structure"
    context: "Combines overview, responsibilities, and achievements into LinkedIn-ready output"
    inputs:
      - name: "linkedin_existing"
        format: "plaintext"
      - name: "responsibilities"
        format: "markdown"
        source: "calculated"
      - name: "achievements"
        format: "markdown"
        source: "calculated"
    processing_steps:
      - "Retain company and role overview if strong, otherwise rewrite"
      - "Insert responsibilities section in bullet format"
      - "Insert achievements section in bullet format"
    output:
      format: "markdown"
      structure:
        - section: Company & Role Overview
        - section: Responsibilities (bulleted, capability-aligned)
        - section: Key Achievements (bulleted)
      validation: "No bold text, no personal pronouns, all content past-tense"
