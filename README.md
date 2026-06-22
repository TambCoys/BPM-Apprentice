# P51 — Apprendista (BPM)

Project for the Business Process Modelling course (MPB), University of Pisa —
Prof. R. Bruni, A.Y. 2025/2026.

Modeling of an apprenticeship scenario in a workshop: a BPMN collaboration
model and its corresponding workflow nets, with analysis of their properties.

## Authors
- Marco Tamberi — 621490
- Fabrizio Anelli — 545819

## Contents
- `report.pdf` — full report
- `bpmn/` — Camunda model (`.bpmn`) and diagrams of the two pools and the collaboration
- `petri/` — the three workflow nets (`.pnml`), their diagrams, the WoPeD analyses, and the reachability graph

## Model
- Two pools: **Apprentice** and **Workshop** (Owner/Supervisor lanes), 13 message flows
- Event-based gateways for deferred choices (hubs *Attesa* and *Esito*)
- A single start and a single end per pool

## Analysis (summary)
| Net | Places | Trans. | Arcs | Sound | Safe | Free-choice |
|-----|:------:|:------:|:----:|:-----:|:----:|:-----------:|
| Apprentice | 12 | 17 | 34 | ✓ | ✓ | ✓ |
| Workshop | 18 | 23 | 46 | ✓ | ✓ | ✓ |
| Collaboration | 43 | 41 | 108 | ✓ | ✓ | ✗ (2 violations) |

The collaboration net remains **sound** and **safe** but is neither free-choice
nor well-handled: an expected consequence of the asynchronous communication /
deferred choice between participants.

## Tools
Camunda Modeler (BPMN) · WoPeD (workflow nets and analysis) · LaTeX (report)
