# P51 — Apprenticeship Process (BPM)

Project developed for the **Business Process Modelling** course (MPB), University of Pisa —
Prof. R. Bruni, A.Y. 2025/2026.

This project models an apprenticeship process in a workshop through a BPMN collaboration diagram and its corresponding Workflow Nets. The model focuses on the interaction between the apprentice and the workshop, as well as on the verification of the main behavioural and structural properties of the resulting Petri nets.

## Scenario

The process involves an **Apprentice** and a **Workshop**. Within the Workshop, two internal roles are represented: the **Owner** and the **Supervisor**.

The process begins when the Owner contacts the Apprentice and conducts an initial interview. After the interview, the Owner assigns a Supervisor, who becomes responsible for guiding the Apprentice throughout the apprenticeship period.

The Apprentice contacts the Supervisor to ask which activity should be performed. The Supervisor proposes a task and, when needed, provides instructions on how to carry it out. The Apprentice then attempts to perform the task and communicates the outcome.

If the task is not completed successfully, the Supervisor may either assign a new activity or ask the Apprentice to repeat the same one. The Apprentice may accept the repetition request or declare that they are unable to perform the task. In the latter case, the Supervisor can insist on repeating the task or propose a different activity.

This interaction is iterative and continues throughout the apprenticeship period. At the end of the period, the Supervisor prepares a report for the Owner. Based on the evaluation of the report, the Owner can decide to:

* offer the Apprentice a contract;
* extend the apprenticeship period;
* end the relationship with the Apprentice.

## Authors

* Marco Tamberi
* Fabrizio Anelli

## Repository Contents

* `report.pdf` — complete project report, including the BPMN model, Workflow Net transformation, and analysis.
* `bpmn/` — Camunda BPMN model (`.bpmn`) and diagrams of the two individual pools and the complete collaboration.
* `petri/` — the three Workflow Nets (`.pnml`), their diagrams, WoPeD analyses, and the reachability graph of the collaboration net.

## BPMN Model

The model is represented as a BPMN collaboration involving two autonomous participants:

* **Apprentice**
* **Workshop**, divided into the **Owner** and **Supervisor** lanes.

The two pools communicate through **13 message flows**. Internal activities within the Workshop are connected through sequence flows, since the Owner and Supervisor belong to the same organization.

The model includes:

* one start event and one end event for each pool;
* a message start event for the Apprentice, who is activated by the Owner's call-up message;
* event-based gateways for deferred choices;
* explicit gateways for all splits and joins of the control flow.

The event-based gateways, named *Attesa* on the Apprentice side and *Esito* on the Supervisor side, model situations in which the next action depends on which message is received from the other participant.

## Workflow Net Analysis

Each BPMN orchestration and the complete collaboration were transformed into Workflow Nets and analysed using WoPeD.

| Net           | Places | Transitions | Arcs | Sound | Safe |    Free-choice   |
| ------------- | :----: | :---------: | :--: | :---: | :--: | :--------------: |
| Apprentice    |   12   |      17     |  34  |   ✓   |   ✓  |         ✓        |
| Workshop      |   18   |      23     |  46  |   ✓   |   ✓  |         ✓        |
| Collaboration |   43   |      41     |  108 |   ✓   |   ✓  | ✗ (2 violations) |

The two local orchestrations are sound, safe, free-choice, well-handled, and S-covered.

The complete collaboration net remains **sound** and **safe**: every reachable marking can still reach the final marking, no transition is dead, and all reachable markings are 1-bounded.

However, the collaboration net is neither free-choice nor well-handled. This is an expected consequence of the asynchronous communication between autonomous participants. In particular, the two free-choice violations correspond to the event-based gateways: the branch that becomes enabled depends not only on the local state of the participant, but also on the arrival of a message from the other pool.

## Tools

* **Camunda Modeler** — BPMN modelling and validation
* **WoPeD** — Workflow Net modelling, semantic analysis, and reachability analysis
* **LaTeX** — report writing and formatting
