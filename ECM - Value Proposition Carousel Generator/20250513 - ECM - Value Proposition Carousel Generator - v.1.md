CarouselContentGenerator:
  name: "Value Proposition Carousel Generator"
  purpose: >
    To create LinkedIn carousel posts that translate a specific value proposition into an engaging,
    persona-targeted, 10+1-slide story. Each post connects customer jobs, pains, and gains to
    the product or service offering through a structured and repeatable storytelling format.

  process:
    - Step 1: Select a value proposition document (e.g., Organizational Development)
    - Step 2: Choose one anchor: job_to_be_done, pain, or gain
    - Step 3: Identify the most relevant target persona (e.g., COO, Strategy Lead)
    - Step 4: Extract supporting excerpts from the value proposition document
    - Step 5: Create a REFERENCE SECTION:
        - anchor_type: job | pain | gain
        - anchor_description: string
        - source_excerpt: string
        - target_persona: string
        - summary_message: string
        - recommended_filename: string
    - Step 6: Draft a 10-slide carousel post:
        - Slide 1 includes a punchy title and explicitly names the persona
        - Slides 2–10 follow a customer-to-solution storytelling arc
        - Slide 11 is optional and interactive (checkbox, reflection, or question)
    - Step 7: Provide an icon/visual suggestion for each slide
    - Step 8: Write the LinkedIn post caption to introduce the carousel

  output_format:
    - type: "CarouselPostPackage"
    - language: "English"
    - tone: "Professional, conversational, clear"
    - includes:
        - ReferenceSection
        - SlideDeck (10+1 slides)
        - PersonaTag (Slide 1)
        - VisualSuggestions
        - Slide1Title
        - LinkedInPostCaption
        - SuggestedFileName

  slide_structure:
    - Slide 1:
        title: Unique headline (hook based on the job, pain, or gain)
        include_persona: true
        visual_suggestion: Compass, strategy map, or misaligned arrows
    - Slide 2:
        title: Customer situation
        visual_suggestion: Gears out of sync or overloaded systems
    - Slide 3:
        title: Desired gains
        visual_suggestion: Rowing crew, checklist, or heartbeat rhythm
    - Slide 4:
        title: Pain points
        visual_suggestion: Tangled wires or jammed traffic
    - Slide 5:
        title: Strategic tension
        visual_suggestion: Bridge gaps, puzzle pieces, or diverging paths
    - Slide 6:
        title: How we reduce friction
        visual_suggestion: Clearing path, declutter icon
    - Slide 7:
        title: How we enable success
        visual_suggestion: Spark, sunrise, or thriving workspace
    - Slide 8:
        title: Our phased method
        visual_suggestion: Blueprint, gear process
    - Slide 9:
        title: Before/after transformation
        visual_suggestion: Split view, rocket launch
    - Slide 10:
        title: Invitation / CTA
        visual_suggestion: Handshake, open door, map signpost
    - Slide 11 (optional):
        title: Reflection / Self-check
        visual_suggestion: Checkbox list, slider
        example_content: |
          Which of these apply to your team?
          [ ] Strategy is clear — but execution drifts  
          [ ] Handoffs break under pressure  
          [ ] Roles are unclear  
          [ ] You're scaling, but stuck in the weeds

  caption_format:
    - 2-line hook that appears above “see more”
    - Short narrative of problem/pain
    - What this carousel explains or solves
    - Swipe CTA
    - 3–5 targeted hashtags

  reuse_guidelines:
    - Use the same format across value propositions (Managed, Hosted, etc.)
    - Always begin with a reference section to clarify focus
    - Each post is persona-specific
    - Visual design can be handled via an external branded template system
