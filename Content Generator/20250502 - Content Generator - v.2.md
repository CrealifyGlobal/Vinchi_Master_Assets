Improvements
- [ ] asking for VP and use cases at the right time in the process below
- [ ] Reminding users to copy and past output - example where there is no campaign. the user should 
- [ ] Campaign schedule 
- [ ] Offer users shot cuts: I want to jump straight to training script
- [ ] Adding next step proposals
- [ ] Staying and aligning with existing plan/schedule or curriculum
- [ ] please do not do this "launched — but", people do not write like this!

# ================================
# VINCI CONTENT GENERATOR (v2.0)
# ================================
# This generator formalizes the Vinci content creation system. It is self-contained and includes all tasks, artefact templates, and writing formulas. It can be used by humans or software systems to reliably generate strategic, scalable content for multiple formats.
# =================================
# Walkthrough

This document shows you **exactly how to use** the Vinci Content Generator if you've never used a system like this before.

---
## Step 1: Drop the Generator File Into GPT

To begin, just **upload the file** called:
Drop this file into ChatGPT (or your assistant tool of choice) and say:

> "Please use this file and help me create a new piece of content."

GPT will read the YAML and **activate the Vinci generator system**.

---

## Step 2: The Generator Will Now Guide You

You’ll be guided through a structured conversation, powered by the generator. The first question will be:

> **“Do you already have a campaign this content belongs to?”**

From there, you’ll move step-by-step through the system.
**You don’t need to know how it works. Just answer each question honestly.** The generator takes care of structure, tone, formulas, and writing.
## What the Generator Will Help You Do
It will guide you through 5 stages:
### 1. Campaign Setup
- Name your campaign
- Choose the goal (educate, convert, inspire, etc.)
- Define your audience and theme
### 2. Strategy Inputs
- Pick a format (LinkedIn post, blog, training video…)
- Define the audience
- Describe the goal
- Share any notes or documents you have
### 3. Content Formula
- The system suggests a structure (like “AIDA” or “Use Case Article”)
- You pick or combine one
### 4. Drafting & Outlining
- The generator builds an outline for you
- You fill it in (with help) one section at a time
- You pick the tone and writing style
### 5. Final Packaging
- You get a title, CTA, and social media post
- The system checks that everything is ready
- Done!
## At the End, You Will Have:

- A **full piece of content**, ready to publish  
- A **formula** to reuse again  
- Tags and metadata for future repurposing  
- Optionally: a shareable LinkedIn or Instagram post
## Example Prompts to Start

Once you've uploaded the YAML file, you can say:

> “Hi GPT, I want to create a new blog post for my product launch campaign. Please use the Vinci Generator YAML I gave you.”

or

> “Please help me write a LinkedIn post based on this YAML generator. I don’t have a campaign yet.”

The assistant will then walk you through the process using the generator logic.

## Tips for New Users

- You can use this system **even if you have no content yet**.
- It will ask you the right questions to get started.
- You can stop and come back later.
- You don’t have to be a strategist — the structure is built in.
- You can create content **faster and better than ever**.
## Want to Reuse the Generator?

You can reuse the same YAML file over and over. Just:
- Drop it into a new GPT session
- Say what you want to create
- Follow the steps

If you're working as a freelancer, assistant, or internal comms writer — this is your **repeatable content system**.
# =================================

# THE GENERATOR CODE STARTS HERE

# =================================

generator:
  id: vinci.content.generator
  name: "Vinci Content Generator"
  version: "1.0"
  description: >
    A modular content generation engine built to support strategy-aligned, multi-format content creation.
    Combines campaign planning, audience profiling, formula-based structuring, and AI-augmented drafting
    into a repeatable system for scalable content production.

  tasks:
    - id: vinci.check_campaign_plan
      name: "Check for existing campaign plan"
      description: "Determine if the content belongs to an existing campaign or needs a new one."
      input_fields: [content_id]
      output_fields: [campaign_status]
      category: "briefing"
      requirements: []
      next_tasks: [vinci.generate_campaign_brief]

    - id: vinci.generate_campaign_brief
      name: "Generate campaign brief"
      description: "Collect input to define a structured campaign plan."
      input_fields:
        - campaign_title
        - theme_id
        - goal
        - audience_segments
        - strategic_message
        - timeline
        - cadence
        - channel_types
      output_fields: [campaign_brief_id]
      category: "briefing"
      requirements: [campaign_status == 'new']
      next_tasks: [vinci.select_content_channel]

    - id: vinci.select_content_channel
      name: "Select content channel"
      description: "Choose the format and platform for content delivery."
      input_fields: [campaign_brief_id]
      output_fields: [content_channel_type]
      category: "briefing"
      requirements: [campaign_brief_id]
      next_tasks: [vinci.define_target_audience, vinci.check_special_dependencies]

    - id: vinci.define_target_audience
      name: "Define target audience"
      description: "Specify the persona and contextual traits."
      input_fields: [content_channel_type]
      output_fields: [persona_id, audience_context]
      category: "briefing"
      requirements: [content_channel_type]
      next_tasks: [vinci.clarify_content_goal]

    - id: vinci.clarify_content_goal
      name: "Clarify content goal"
      description: "State the intent: educate, convert, inspire, etc."
      input_fields: [persona_id]
      output_fields: [content_goal]
      category: "briefing"
      requirements: [persona_id]
      next_tasks: [vinci.input_materials]

    - id: vinci.input_materials
      name: "Input source materials"
      description: "Gather notes, research, transcripts, or docs."
      input_fields: [content_id, campaign_brief_id]
      output_fields: [content_inputs]
      category: "briefing"
      requirements: [content_goal]
      next_tasks: [vinci.check_special_dependencies]

    - id: vinci.check_special_dependencies
      name: "Check for special dependencies"
      description: "Identify format-specific needs like curriculum or scripts."
      input_fields: [content_channel_type, content_inputs]
      output_fields: [dependency_list]
      category: "briefing"
      requirements: [content_channel_type]
      next_tasks: [vinci.suggest_and_select_formula]

    - id: vinci.suggest_and_select_formula
      name: "Suggest and select formula"
      description: "Recommend a content formula based on inputs."
      input_fields: [content_goal, persona_id, content_channel_type]
      output_fields: [formula_selection]
      category: "generation"
      requirements: [dependency_list]
      next_tasks: [vinci.preview_formula_structure]

    - id: vinci.preview_formula_structure
      name: "Preview formula structure"
      description: "Show formula outline, tone, and sections."
      input_fields: [formula_selection]
      output_fields: [formula_structure]
      category: "generation"
      requirements: [formula_selection]
      next_tasks: [vinci.generate_outline]

    - id: vinci.generate_outline
      name: "Generate outline"
      description: "Generate a structured content outline from the formula."
      input_fields: [formula_structure, content_inputs]
      output_fields: [content_outline]
      category: "generation"
      requirements: [formula_structure]
      next_tasks: [vinci.customize_outline]

    - id: vinci.customize_outline
      name: "Customize outline"
      description: "Edit or expand the outline with data or stories."
      input_fields: [content_outline]
      output_fields: [final_outline]
      category: "generation"
      requirements: [content_outline]
      next_tasks: [vinci.section_drafting]

    - id: vinci.section_drafting
      name: "Section-by-section drafting"
      description: "Guide the user through composing the content."
      input_fields: [final_outline, persona_id, content_inputs]
      output_fields: [raw_draft]
      category: "generation"
      requirements: [final_outline]
      next_tasks: [vinci.tone_style_selection]

    - id: vinci.tone_style_selection
      name: "Select tone and style"
      description: "Refine tone for target audience and context."
      input_fields: [raw_draft, persona_id]
      output_fields: [styled_draft]
      category: "generation"
      requirements: [raw_draft]
      next_tasks: [vinci.generate_headline]

    - id: vinci.generate_headline
      name: "Generate headline and subtitle"
      description: "Create attention-grabbing title and subtitle."
      input_fields: [styled_draft, content_goal]
      output_fields: [title_block]
      category: "packaging"
      requirements: [styled_draft]
      next_tasks: [vinci.generate_cta_and_social]

    - id: vinci.generate_cta_and_social
      name: "Generate CTA and social wrapper"
      description: "Craft a CTA and preview post for sharing."
      input_fields: [styled_draft, title_block]
      output_fields: [cta_block, social_post_preview]
      category: "packaging"
      requirements: [title_block]
      next_tasks: [vinci.final_review]

    - id: vinci.final_review
      name: "Final review and packaging"
      description: "Run a QA checklist and confirm readiness for publishing."
      input_fields: [styled_draft, title_block, cta_block, social_post_preview]
      output_fields: [approved_content_package]
      category: "packaging"
      requirements: [styled_draft, title_block]
      next_tasks: []

  artefacts:
    - id: artefact.theme_sheet
      name: "Theme Sheet"
      description: "Defines narrative focus, beliefs, messages, and risks."
      structure:
        - title
        - belief
        - strategic_role
        - messages
        - personas
        - use_cases
        - campaigns
        - risks

    - id: artefact.campaign_brief
      name: "Campaign Brief"
      description: "Links a campaign to themes, audiences, and outputs."
      structure:
        - campaign_name
        - theme
        - goal
        - audience_segments
        - key_message
        - timeline
        - cadence
        - channel_types

    - id: artefact.persona_profile
      name: "Persona Profile"
      description: "Summarizes a target persona’s context and needs."
      structure:
        - label
        - goals
        - challenges
        - tone_fit
        - maturity
        - formats
        - platforms

    - id: artefact.use_case_playbook
      name: "Use Case Playbook"
      description: "Outlines how to execute content for a specific business case."
      structure:
        - use_case
        - purpose
        - best_formats
        - recommended_formulas
        - suggested_channels
        - pitfalls
        - success_metrics

    - id: artefact.brief_output
      name: "Brief Output"
      description: "Snapshot of all logical inputs before writing begins."
      structure:
        - type
        - campaign
        - theme
        - persona
        - use_case
        - goal
        - inputs
        - formula
        - tone

    - id: artefact.content_template
      name: "Content Format Template"
      description: "Reusable format-specific shell for structuring content."
      structure:
        - template_title
        - structure
        - formula_fit
        - tone_style
        - example

  formulas:
    - id: formula.aida
      name: "AIDA"
      purpose: "Move audience through sequential attention → action."
      writing_style: "Persuasive, direct"
      key_traits: ["Sequential", "CTA-focused"]
      structure: ["Attention", "Interest", "Desire", "Action"]
      use_cases: ["Ads", "Sales pages"]
      audience_fit: ["Buyers", "General consumers"]

    - id: formula.pas
      name: "PAS"
      purpose: "Trigger action via problem-framing and relief."
      writing_style: "Urgent, clear"
      key_traits: ["Problem", "Agitate", "Solution"]
      structure: ["Problem", "Agitate", "Solution"]
      use_cases: ["Emails", "Webinars"]
      audience_fit: ["Problem-aware leads"]

        - id: formula.fab
      name: "FAB"
      purpose: "Translate product features into clear customer benefits."
      writing_style: "Structured, technical–to–practical"
      key_traits: ["Feature", "Advantage", "Benefit"]
      structure: ["Feature", "Advantage", "Benefit"]
      use_cases: ["Product pages", "Sales decks"]
      audience_fit: ["Technical buyers", "Procurement"]

    - id: formula.reasons_why
      name: "3 Reasons Why"
      purpose: "Address skepticism through proof, differentiation, and urgency."
      writing_style: "Confident, logical"
      key_traits: ["Proof-driven", "Credibility", "Urgency"]
      structure: ["Why us", "Why it works", "Why now"]
      use_cases: ["Pitches", "B2B pages"]
      audience_fit: ["Rational leads", "Late-stage buyers"]

    - id: formula.pppp
      name: "PPPP"
      purpose: "Combine emotional and rational persuasion for action."
      writing_style: "Narrative, layered"
      key_traits: ["Picture", "Promise", "Proof", "Push"]
      structure: ["Scene", "Offer", "Evidence", "Call to action"]
      use_cases: ["Sales letters", "Video ads"]
      audience_fit: ["Mixed logic–emotion audiences"]

    - id: formula.four_us
      name: "4 U’s"
      purpose: "Write hooks that are urgent, useful, unique, and ultra-specific."
      writing_style: "Punchy, compact"
      key_traits: ["Urgency", "Value", "Differentiation", "Specificity"]
      structure: ["Checklist"]
      use_cases: ["Subject lines", "Headlines", "Carousels"]
      audience_fit: ["Impression-stage viewers"]

    - id: formula.four_cs
      name: "4 C’s"
      purpose: "Ensure messaging is aligned, trustworthy, and sharp."
      writing_style: "Clear, professional"
      key_traits: ["Clear", "Credible", "Consistent", "Competitive"]
      structure: ["Principles"]
      use_cases: ["Messaging docs", "Homepages"]
      audience_fit: ["Professional services", "B2B"]

    - id: formula.five_objections
      name: "5 Objections"
      purpose: "Preemptively answer common sales objections."
      writing_style: "Reassuring, strategic"
      key_traits: ["Handles trust, time, money, need, effectiveness"]
      structure: ["Objection-response"]
      use_cases: ["Sales collateral", "Objection handling"]
      audience_fit: ["Late-funnel buyers"]

    - id: formula.use_case_article
      name: "Use Case Article"
      purpose: "Document real-world problem-solution-result stories."
      writing_style: "Practical, outcome-focused"
      key_traits: ["Challenge", "Resolution", "Impact"]
      structure: ["Intro", "Problem", "Solution", "Impact", "Close"]
      use_cases: ["Case studies", "Product marketing"]
      audience_fit: ["Decision-makers", "Internal buyers"]

    - id: formula.sss
      name: "SSS (Star–Story–Solution)"
      purpose: "Deliver fast, memorable transformations."
      writing_style: "Short, narrative"
      key_traits: ["Character", "Arc", "Outcome"]
      structure: ["Who", "What happened", "Result"]
      use_cases: ["Carousels", "Intros"]
      audience_fit: ["Broad audience", "Social formats"]

    - id: formula.sonia_five
      name: "Sonia Simone’s 5 Pieces"
      purpose: "Use story archetypes to create emotional connection."
      writing_style: "Personal, inspirational"
      key_traits: ["Hero", "Goal", "Conflict", "Mentor", "Moral"]
      structure: ["Hero", "Goal", "Conflict", "Mentor", "Moral"]
      use_cases: ["Origin stories", "Founder content"]
      audience_fit: ["Story-driven learners", "Coaching clients"]

    - id: formula.open_loops
      name: "Open Loops"
      purpose: "Trigger curiosity through delayed resolution."
      writing_style: "Hook-driven, suspenseful"
      key_traits: ["Open narrative", "Cliffhanger", "Closure later"]
      structure: ["Open", "Tease", "Reveal"]
      use_cases: ["Post intros", "Webinar hooks"]
      audience_fit: ["Distracted audiences", "Scrollers"]

    - id: formula.pov_article
      name: "POV Article"
      purpose: "Share perspective or conviction via personal reflection."
      writing_style: "Reflective, opinionated"
      key_traits: ["Tension", "Insight", "Conclusion"]
      structure: ["Intro", "Context", "Tension", "Reflection", "Close"]
      use_cases: ["Thought pieces", "LinkedIn essays"]
      audience_fit: ["Professional peers", "Cultural communities"]

    - id: formula.microlearning
      name: "Microlearning Script"
      purpose: "Deliver short-form, clear, focused instructional content."
      writing_style: "Simple, instructional"
      key_traits: ["Compact", "Modular", "Clear takeaway"]
      structure: ["Intro", "Steps", "Tips", "CTA"]
      use_cases: ["L&D", "Internal enablement"]
      audience_fit: ["Fast learners", "HR and Ops"]

    - id: formula.playbook
      name: "Playbook Template"
      purpose: "Operationalize repeatable methods and capabilities."
      writing_style: "Procedural, strategic"
      key_traits: ["SOP", "Metrics", "Roles"]
      structure: ["Overview", "Steps", "KPIs", "Risks"]
      use_cases: ["Internal docs", "Scaling teams"]
      audience_fit: ["Managers", "Enablement leads"]

    - id: formula.trend_deep_dive
      name: "Trend Deep-Dive"
      purpose: "Explore patterns and shifts using data and commentary."
      writing_style: "Analytical, expert"
      key_traits: ["Pattern spotting", "Insight", "Implications"]
      structure: ["Intro", "Data", "Analysis", "Impact"]
      use_cases: ["Whitepapers", "Foresight posts"]
      audience_fit: ["VCs", "Execs", "Strategy teams"]

    - id: formula.readers_digest
      name: "Reader’s Digest Blueprint"
      purpose: "Convey high-value insight in short format."
      writing_style: "Concise, fact-rich"
      key_traits: ["Fact", "Takeaway", "Brevity"]
      structure: ["Fact", "Tip", "Mini-story"]
      use_cases: ["Carousels", "Cheat sheets"]
      audience_fit: ["Busy professionals", "Analysts"]

    - id: formula.scholarly_review
      name: "Scholarly Review"
      purpose: "Compare and synthesize literature to form original insight."
      writing_style: "Academic, structured"
      key_traits: ["Synthesis", "Contrast", "Gap spotting"]
      structure: ["Intro", "Themes", "Gaps", "Takeaways"]
      use_cases: ["Research blogs", "Sector reviews"]
      audience_fit: ["Researchers", "OD leaders", "L&D"]

    - id: formula.book_review
      name: "Book Review"
      purpose: "Share lessons from a book in a useful professional frame."
      writing_style: "Narrative, practical"
      key_traits: ["Why it matters", "What I learned", "Why you should care"]
      structure: ["Intro", "Takeaways", "Application", "Recommendation"]
      use_cases: ["Learning blogs", "Knowledge sharing"]
      audience_fit: ["Curious learners", "Leadership"]

    - id: formula.formula_design
      name: "Formula Design Framework"
      purpose: "Create and operationalize custom content formulas."
      writing_style: "Systematic, meta"
      key_traits: ["Repeatable", "Customizable", "Design logic"]
      structure: ["Name", "Purpose", "Structure", "Use Cases", "Example"]
      use_cases: ["Internal systems", "Tool design"]
      audience_fit: ["Strategists", "Instructional designers"]
