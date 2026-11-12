---
title: Threat Modelling
layout: home
nav_order: 6
permalink: /threat-modelling/
---

# Threat Modelling

{: .summary }
> **In short:** before you harden anything, map how your application can be attacked. You hand in a **v1** after the first week (Thu 03.12) and a **final** version at the freeze (Thu 10.12). After the red phase you compare it with what actually happened.

This builds directly on the week 4 lecture and exercise. Use the same tools: a data-flow diagram, STRIDE, and the AI-specific catalogues.

---

## What your threat model must contain

### 1. System description

Half a page: what the application does, who its users are, and which assets need protection. Use your track's pages: each one lists the sensitive content and the safety properties the application must keep.

### 2. Data-flow diagram (DFD)

One diagram of your application as you will actually build it:

- **external entities**: the user, anyone who can post to your stores, the inference endpoint;
- **processes**: your API, retrieval, the LLM call, tool calls, guardrails;
- **data stores**: the corpus, the vector index, the tool or message store, session memory;
- **data flows** between them, and the **trust boundaries** they cross.

Draw it with any tool (draw.io, Excalidraw, Mermaid, on paper). Every trust boundary is a place to look for threats.

### 3. Threat table

One row per threat. For each threat give:

| Column | Content |
|---|---|
| ID | e.g. `T-07` |
| Component / flow | where in the DFD it applies |
| STRIDE category | Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege |
| AI-specific mapping | the matching [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/) entry and/or [MITRE ATLAS](https://atlas.mitre.org/) technique, where one applies |
| Description | the attack, concretely: who does what, through which input |
| Likelihood / impact | low, medium or high each, with one line of justification |
| Mitigation | what you will do about it, or why you accept the risk |
| Status | planned, implemented, or accepted |

Aim for **15-25 threats**. A short list of well-reasoned, concrete threats beats a long list of generic ones. "Prompt injection" is not a threat; "a posted bulletin instructs the model to append the system prompt to every answer" is.

### 4. Prioritisation

Which five threats matter most for your application, and why? These are the ones you should mitigate first.

---

## v1 and final

| | v1 (Thu 03.12) | Final (Thu 10.12, freeze) |
|---|---|---|
| Purpose | Get feedback before you build your defences | Document what you actually built |
| DFD | Your planned architecture | Your deployed architecture |
| Threat table | Mitigations may all be *planned* | Status of every mitigation is up to date |
| Length | 2-4 pages | 3-6 pages |

You get feedback on v1 at the check-in on Fri 04.12. The final version goes into your final report **unchanged**: the retrospective is graded on how honestly you compare it with the red phase, not on whether it was perfect.

---

## Retrospective (after the red phase)

For each exploit landed against you, add a row to a retrospective table:

| Column | Content |
|---|---|
| Finding | the red team's finding ID and title |
| In the threat model? | yes (which threat ID), partly, or no |
| Mitigated? | yes, partly, or no, and if yes, why did the mitigation fail? |
| Root cause | the underlying reason, not the symptom |
| Fix | what you would change, and whether that would have stopped it |

Finish with a short paragraph: which kind of threat did you consistently underestimate, and what does that tell you about threat modelling AI systems?

---

## Useful references

- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/)
- [MITRE ATLAS](https://atlas.mitre.org/): adversarial threat landscape for AI systems
- [Microsoft STRIDE threat categories](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats)
- [NIST AI 100-2: Adversarial Machine Learning taxonomy](https://csrc.nist.gov/pubs/ai/100/2/e2025/final)
- The week 4 slides and exercise on Ilias
