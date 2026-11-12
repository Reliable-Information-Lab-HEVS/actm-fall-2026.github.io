---
title: General Information
layout: home
nav_order: 2
permalink: /general-information/
---

# General Information

The rules that apply to every track. Read this page before you start building.

---

## Teams and tracks

- **Six teams of three**, two teams per track. Teams and tracks are announced at the kickoff on Fri 27.11.
- Each team gets its own VM; see [Infrastructure]({% link infrastructure.md %}).
- In the red phase, each team attacks **two** applications, one from each of the other two tracks. Every application is therefore attacked by two teams.

| Team | Track | Attacks | Attacked by |
|---|---|---|---|
| Team 1 | Track 1 | Team 3, Team 5 | Team 3, Team 5 |
| Team 2 | Track 1 | Team 4, Team 6 | Team 4, Team 6 |
| Team 3 | Track 2 | Team 1, Team 5 | Team 1, Team 5 |
| Team 4 | Track 2 | Team 2, Team 6 | Team 2, Team 6 |
| Team 5 | Track 3 | Team 1, Team 3 | Team 1, Team 3 |
| Team 6 | Track 3 | Team 2, Team 4 | Team 2, Team 4 |

You never attack the other team on your own track: you know that scenario too well from the inside.

---

## Red-team visibility

You attack under realistic, partially-informed conditions:

- You **know** the target's track specification (all of its track pages), the I/O contract, and the model menu.
- You **do not see** the target's source code, system prompts, guardrail configuration, or threat model. The target is a **black box**.
- At the start of the red phase you receive the **red-team pack** for each target's track: what was seeded in the data, so you know what counts as a hit.

Repositories stay private until the end of the red phase. Threat models are shared with your attackers after the red phase, for your retrospective.

---

## Rules of engagement

The VMs sit on the HSLU network, and your team has root on its own VM. These rules are strict. Breaking them can cost you your project grade, and HSLU's network protection may block the VM that broke them.

{: .warning }
> **In scope:** only the documented HTTP endpoints of **your two assigned targets**, on `https://teamN.{{ site.domain }}`, plus harmless `GET` requests such as `/health`.
>
> **Out of scope, always:** SSH or any other port on another team's VM, port or vulnerability scanning, the inference endpoint itself, this website, Ilias, and any HSLU system. No denial of service: keep to at most **2 concurrent requests** per target, automated tools and LLM agents included.

- Do not touch another team's VM, data or inference key, even if you find a way to.
- Do not share your inference API key, and do not use it from anywhere but your own VM and your own laptops.
- Everything in the data is synthetic or public. Do not try to harm real people or real systems.
- If you think you have found a problem with the infrastructure rather than with an application, stop and tell the teaching team.

---

## Deliverables

| Deliverable | Due | Where |
|---|---|---|
| Threat model v1 | Thu 03.12 | [Hand-in]({{ site.handin_url }}) |
| Deployed application, tagged `v1`, plus final threat model | Thu 10.12 (freeze) | [Hand-in]({{ site.handin_url }}); see your track's *Submitting Application* page |
| One red-team report per target (two in total) | Thu 17.12 | [Red-team upload]({{ site.redteam_upload_url }}); see [Red-Team Reports]({% link red-team-reports.md %}) |
| Presentation | Fri 18.12 | In class |
| Final report | {{ site.report_deadline }} | [Hand-in]({{ site.handin_url }}) |

After the red phase, each team receives the red-team reports written against its application. You need them for the retrospective in your final report.

### Final report

One report per team, about **10-15 pages** plus appendices. It contains:

1. **Threat model**: the final version from the freeze, unchanged.
2. **Design and mitigations**: how your application works, and how each mitigation traces back to a threat in your threat model.
3. **Red-team work**: a summary of what you found against your two targets, and how you went about it.
4. **Retrospective**: for every exploit landed against you, was it in your threat model? Was it mitigated? Why did the mitigation fail? What would you patch, and how?
5. **Reflection and AI usage**: what went well, what you would do differently, and the AI-use declaration required by the course rules (which tools, for which parts, where they helped and where they led you astray).

### Presentation

10 minutes plus 5 minutes of questions, all team members present. Cover your design, your best defence, your best attack, and what you would do differently. We may ask about your code and design decisions, so make sure every member understands the whole codebase.

---

## Grading

The project counts for **50%** of the course grade: 25% for the final report and 25% for the presentation.

### Acceptance bar

At the freeze, your application must be **running, reachable, and follow the I/O contract of your track**, and pass a minimum of the utility tests run against it. This stops a team from scoring "secure" by refusing everything. An application below the acceptance bar caps the report grade.

### Report

| Component | Weight |
|---|---|
| Threat model: completeness, realism, prioritisation | 25% |
| Design and mitigations, and how they trace to the threat model | 25% |
| Red-team work: diversity, reproducibility and severity reasoning of your findings (not their number) | 25% |
| Retrospective, reflection, and AI-use declaration | 25% |

{: .note }
> **Being exploited does not cost you marks.** Every application will be broken somewhere. What we grade is whether you anticipated it, how you reasoned about it, and what you learned from it.

### Presentation

Graded on clarity, technical depth, and how well you explain what you would do differently next time.

### Leaderboard

For fun, we keep a leaderboard in the style of a hackathon: points for exploits you land (weighted by severity and difficulty), and points for the utility of your application. It is presented on Fri 18.12 and does not count towards your grade.

---

## Using AI tools

AI tools are encouraged in the project: for writing code, tool wrappers, test scripts, and for red-teaming. The course rules apply:

- **Own your codebase.** We may ask any team member about any part of the code.
- **Do not let AI write your threat model or report.** The point of writing them is to clarify your own thinking.
- **Declare all AI usage** in your final report.
- Automated or agent-driven red-teaming is allowed, within the rate limit above.

For effective use, first write down a plan of your application yourselves, then use the models to implement it. Models are good at implementing a plan, and much weaker at devising one.
