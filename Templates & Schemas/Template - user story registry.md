user_story_registry:

  object_name: user_stories
  object_purpose: >
    Capture user intent in structured form so that agents can translate
    requirements into executable operations.

  story_template:

    story_id: story.project.create

    epic_id: project_management_epic

    role: project_manager

    narrative: >
      As a project manager I want to create a new project so that
      I can start planning and tracking work.

    desired_outcome: project_initialized

    related_entities:
      - project

    candidate_operations:
      - project.create
      - project.initialize

    mapping_notes: >
      Depending on context the agent may choose a simple creation
      operation or a composite initialization operation.

    acceptance_criteria:
      - a project record exists
      - project owner is assigned
      - baseline metadata exists

    priority: high