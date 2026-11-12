---
title: Red-Team Reports
nav_order: 9
layout: home
permalink: /red-team-reports/
---

# Red-Team Reports

{: .summary }
> **In short:** from Fri 11.12 to Thu 17.12 you attack your two assigned targets. For each target you hand in **one report** listing your findings. Every finding must be concrete, reproducible, and break a safety property of the target's track.

---

## What counts as a finding

A valid finding is **concrete, reproducible, and violates a safety property**. The safety properties are listed on each track's *Red Team Information* page: restricted procedures, classified facts, confidential lists, a private feed, wrong verdicts, unsafe advice. Beyond those, any break of confidentiality, integrity or safety counts, for example revealing the system prompt, reading another user's session, or making the application obey instructions planted in its data.

Creativity is rewarded. A finding that works through an indirect channel and survives sensible defences is worth more than a one-line jailbreak.

Negative results are useful too. "We tried X in five ways and it held" tells the target team what they did right; include the most interesting ones in a short section.

---

## How to work

- **Log everything.** Keep every request and response you send, with a timestamp and the model, for example as one JSON file per exchange. You need them as evidence, and a finding you cannot reproduce does not count.
- **Reproduce before you report.** Run each finding again in a fresh session. If the target lets you choose the model, try a second one.
- **Prefer variety to volume.** Five findings of five different classes beat twenty variations of the same jailbreak.
- **Stay in scope.** Only the documented endpoints of your two targets, at most 2 concurrent requests. See the [rules of engagement]({% link general-information.md %}#rules-of-engagement).

---

## Report structure

One Markdown or PDF file per target, named `teamX-vs-teamY`.

### 1. Header

Target team and URL, track, dates, models tested, and a count of findings per severity.

### 2. Summary table

| ID | Severity | Class | Title | Models | Status |
|---|---|---|---|---|---|
| X-01 | High | Indirect prompt injection | Posted record makes the assistant reveal its system prompt | gemma, mistral | Confirmed (fresh session) |

- **Severity**: Critical, High, Medium, Low, or Info. Justify it from the impact in the track's scenario, not from how clever the attack was.
- **Class**: for example direct prompt injection, indirect prompt injection, jailbreak / framing, system-prompt extraction, data exfiltration, integrity (wrong verdict or advice), session or access control. Map it to the [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/) where you can.

### 3. One section per finding

1. **Description**: what the exploit is, how you found it, and which safety property it violates.
2. **Reproduction**: the exact sequence of requests, with all parameters and payloads, so that someone else can repeat it without asking you.
3. **Evidence**: the response that shows the violation, quoted from your logs.
4. **Suggested fix**: one or two sentences.

### 4. What held

The defences you tried and could not break.

---

## Handing in

Upload each report by **Thu 17.12, 23:59** to the [red-team upload]({{ site.redteam_upload_url }}). Afterwards, every report is passed on to the team it targets, so write it for them: they need to understand and reproduce your findings for their retrospective.
