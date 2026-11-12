---
title: Welcome
layout: home
nav_order: 1
description: Final project of the HSLU course AI Based Cybersecurity Threat Detection and Mitigation.
permalink: /
---

# ACTM Final Project
{: .fs-9 }

Build an LLM application, threat-model it, then break someone else's.
{: .fs-6 .fw-300 }

---

## What this project is about

In the final four weeks of the course you work in a **team of three**. Your team designs and deploys an LLM-powered application for a realistic scenario, writes a threat model for it, and defends it. Once every application is frozen, your team turns around and **red-teams the applications of two other teams**.

Every team is therefore both a **blue team** (building and defending its own application) and a **red team** (probing other teams' deployments for exploits). This is where the course comes together: the threat modelling from Block 1, the defensive thinking from Block 2, and the attacks on ML and LLM systems from Block 3.

All teams use the same open-weights models on the same inference endpoint. What you control is the **scaffolding, the prompting, the tool design, and the guardrails**, not the size of the model.

There are **three tracks**, each with its own scenario and data. Two teams work on each track.

| Track | Scenario | What must stay protected |
|---|---|---|
| [Track 1: Documentation Assistant]({% link tracks/track-1/index.md %}) | Conversational assistant over dense firearms maintenance manuals, with two lookup tools backed by an open data store | Supervisor-only procedures, classified facts |
| [Track 2: Export Control Advisor]({% link tracks/track-2/index.md %}) | Compliance advisor over Swiss arms and dual-use export law | The confidential list of flagged entities, correct licensing verdicts |
| [Track 3: Air Threat Advisor]({% link tracks/track-3/index.md %}) | Advisor over Ukraine's official siren record and a private monitoring feed | Never clearing an area under alert, the private feed |

---

## Timeline

All deadlines are **23:59** on the day given. Lectures are on Fridays.

| Week | Date | Session | What happens | Deadline |
|---|---|---|---|---|
| 11 | Fri 27.11 | **Kickoff** (in person) | Teams and tracks, VM access, data and API keys. Template walkthrough. Threat-modelling workshop: first data-flow diagram and STRIDE table for your application. | |
| 11-12 | | Build | Build the first working version of your application. | **Thu 03.12**: threat model v1 |
| 12 | Fri 04.12 | **Check-in** (remote) | Short slot per team: feedback on your threat model, and a first run of the utility tests against your endpoint. | |
| 12 | | Build | Implement your mitigations, harden, test. | **Thu 10.12**: freeze. Application deployed and tagged `v1`, final threat model handed in. |
| 13 | Fri 11.12 | **Red phase opens** (remote) | Targets assigned, red-team packs released. Attack your two target applications. | **Thu 17.12**: one red-team report per target |
| 14 | Fri 18.12 | **Presentations** (in person) | 10 min presentation + 5 min Q&A per team, then a joint debrief. | |
| | {{ site.report_deadline }} | | | **Final report** |

{: .important }
> Project deadlines are **hard deadlines**. The red phase depends on every application being deployed at the freeze, so a late application cannot be attacked, and a team that cannot be attacked cannot be graded on its defences.

---

## Where to go from here

1. [General Information]({% link general-information.md %}): teams, rules of engagement, deliverables, and grading.
2. Your **track pages** (see the table above): scenario, data, the contract your application must meet, and how to hand it in.
3. [Threat Modelling]({% link threat-modelling.md %}): what your threat model must contain, at v1 and at the freeze.
4. [Infrastructure]({% link infrastructure.md %}): your team VM, the inference endpoint, and how to deploy.
5. [RAG]({% link rag.md %}): a primer on retrieval-augmented generation, which every track needs.
6. Once the red phase opens: your target's **Red Team Information** page and [Red-Team Reports]({% link red-team-reports.md %}).
