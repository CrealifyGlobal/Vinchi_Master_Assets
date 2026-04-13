meta::
#class_behavior
#status_completed

[[Class_Activity]]

Class Name:
Activity

Behavioral Purpose:
An Activity is the fundamental building block of operational execution—representing a discrete unit of work that contributes to a larger process. It is where strategy becomes action at the most granular level. Activities may be manual, automated, conditional, or decision-based, and are coordinated through processes such as Work Processes, Business Processes, or Management Processes. Activities are the direct moments of execution—the smallest accountable steps that define how work gets done.

Core Behaviors:
- **Execute Operational Tasks:** Activities are where employees or systems perform defined actions to move work forward.
- **Chain Together for Process Flow:** Activities link to one another in sequences, often with dependencies or parallel execution logic.
- **Express Process Design:** They embody the detail of process design, capturing where human effort, decision-making, or automation is required.
- **Enable Measurement and Optimization:** Because they are discrete and repeatable, Activities are ideal points for performance tracking and improvement.

Interaction Targets:
Activities primarily interact with:
- **Work Processes**, providing the execution-level detail that composes the process.
- **Resources**, which may be people, tools, or systems required to complete the task.
- **Policies**, enforcing constraints or rules guiding how activities are performed.
- **Metrics**, enabling visibility into time, quality, throughput, and efficiency.
- **Other Activities**, forming chains, conditions, or branching paths depending on outcomes or events.

Sensing Triggers:
- Completion of a dependent activity (sequential trigger).
- A change in input data, approval status, or external event (conditional trigger).
- Schedule or recurring execution cycles (e.g., daily report generation).
- Workflow status update or exception (e.g., rework required due to failed validation).

Decision Rules:
- Each Activity must belong to at least one parent Process (Work, Business, or Management).
- Activities can depend on other Activities but cannot loop back onto themselves.
- If the Activity is marked as Automated, it should not require manual resources.
- All Activities should be time-bound and measurable in terms of performance or completion.

Adaptation Behavior:
- Can be reassigned (human vs. machine) depending on technology enablement or resource availability.
- Adjusted or skipped based on process branching, conditions, or exception handling.
- Optimized through continuous improvement, lean mapping, or automation strategies.
- Sequenced differently depending on context (e.g., case management or adaptive workflows).

Lifecycle Phases:
- **Defined:** The activity is named, scoped, and mapped within a parent process.
- **Enabled:** Required resources (tools, roles, systems) are made available.
- **Triggered:** Execution is initiated based on process logic or external events.
- **Performed:** The activity is completed, possibly producing an output or triggering the next.
- **Measured:** Data is collected to assess time, quality, or performance.
- **Refined:** Insights drive changes in sequencing, resource allocation, or execution method.

Special Traits:
- **Atomic:** The smallest unit of executable work within the organization.
- **Flexible:** Can be reused across processes, rearranged, or conditionally skipped.
- **Bridge Between Design and Execution:** The clearest point where workflow diagrams meet actual task performance.

Fine-Tuning Parameters:
- **Persistence Score:** 7/10 — Activities tend to stay consistent, though automation and process change can replace or reshape them.
- **Flexibility Score:** 8/10 — Highly adaptable in sequencing, responsibility, and execution method.
- **Influence Radius:** 6/10 — While small individually, many Activities in sequence have large cumulative effects.
- **Sensitivity to Change Triggers:** 9/10 — Easily impacted by changes in tools, people, inputs, or rules.
