epic_registry:

  object_name: epics
  object_purpose: >
    Organize related user stories into thematic collections representing
    larger requirement groupings.

  epic_template:

    epic_id: project_management_epic
    name: Project Lifecycle Management
    description: >
      Provide capabilities for creating, managing, and monitoring projects
      throughout their lifecycle.

    related_domains:
      - project_management

    related_capabilities:
      - delivery management
      - portfolio governance

    contained_user_stories:
      - story.project.create
      - story.project.assign_owner