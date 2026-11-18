---
title: General Information
layout: home
nav_order: 2
permalink: /general-information/
---

# General Information

Here is general information that holds for all teams. 

---

## Teams and tracks

- We have five teams of three, and one team of four. We have two teams per track, and the tracks are assigned at the kickoff on Friday 27.11.
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

---

## Rules of engagement

The VMs sit on the HSLU network, and your team has root on its own VM. 

{: .warning }
> **In scope:** only the documented HTTP endpoints of **your two assigned targets**, on `https://actm-teamN.{{ site.domain }}`, plus harmless `GET` requests such as `/health`.
>
> **Out of scope:** SSH or any other port on another team's VM, port or vulnerability scanning, the inference endpoint itself, this website, Ilias, and any HSLU system. No denial of service: keep to at most 5 concurrent requests per target, automated tools and LLM agents included.

- Do not touch another team's VM, data or inference key, even if you find a way to.
- Do not share your inference API key, and do not use it from anywhere but your own VM and your own laptops.
- If you think you have found a problem with the infrastructure rather than with an application, please inform Alexander. 

---

## Deliverables

| Deliverable | Due | Where |
|---|---|---|
| Threat model and Application Plan| Sunday 06.12, 23.59 | [Hand-in](https://forms.gle/zxaATJR22rB2fMGDA) |
| Deployed application | Thursday 10.12, 23.59 | [Hand-in](https://forms.gle/Uyb8weJvRuceuSHQ8); see your track's *Submitting Application* page |
| One red-team report per target (two in total) | Thursday 17.12, 23.59 | [Red-team upload](https://forms.gle/ioVjW6juDme5xBCw8); see [Red-Team Reports]({% link red-team-reports.md %}) |
| Presentation | Friday 18.12 | In class |

### Presentation

15 minutes plus 5 minutes of questions, all team members present. Cover your design, your best defence, your best attack, and what you would do differently. We may ask about your code and design decisions, so make sure every member understands the codebase.

---

## Grading

The project counts for **50%** of the course grade, and the grading is based on the threat model, the application, the red-team report, and the presentation. 




