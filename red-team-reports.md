---
title: Red-Team Reports
nav_order: 9
layout: home
permalink: /red-team-reports/
---

# Red-Team Reports

{: .summary }
> **In short:** from Fri 11.12 to Thu 17.12 you attack your two assigned targets. For each target you hand in **one report** listing your findings. 

---

## What counts as a finding

A valid finding is **concrete, reproducible, and violates a safety property**. The safety properties are listed on each track's *Red Team Information* page: examples include restricted procedures, classified facts and confidential lists. Beyond those, any break of confidentiality, integrity or safety counts, for example revealing the system prompt.

---

## Report structure

One PDF file per target, named `teamX-vs-teamY.pdf`.

### 1. Header

Target team and URL, track, dates, models tested, and a count of findings per severity.

### 2. One section per finding

1. **Description**: what the exploit is, how you found it, and which safety property it violates.
2. **Reproduction**: the exact sequence of requests, with all parameters and payloads, so that someone else can repeat it without asking you.
3. **Evidence**: the response that shows the violation, quoted from your logs.
4. **Suggested fix**: one or two sentences.

---

## Handing in

Upload each report by **Thu 17.12, 23:59** to the [red-team upload]({{ site.redteam_upload_url }}). 
