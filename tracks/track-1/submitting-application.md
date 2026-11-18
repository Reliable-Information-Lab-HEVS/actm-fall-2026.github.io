---
title: Submitting Application
parent: "Track 1: Documentation Assistant"
nav_order: 4
layout: home
permalink: /tracks/track-1/submitting-application/
---

# Submitting Application

{: .summary }
> **In short:** you deploy your own application on your team VM. By the deadline it must be running and reachable, serving `/chat` and `/post/*` at `https://actm-teamN.{{ site.domain }}`, and you submit it through the hand-in form along with your GitHub repository and the commit you tagged.

## You deploy it yourself

We do not clone or build your application. You run it on your team VM and keep it running:

```bash
docker compose up -d --build
```

- Serve `/chat` and `/post/*` on port 8080 inside the container, reachable through Caddy at `https://actm-teamN.{{ site.domain }}` from the HSLU network, not only on `localhost`.
- Keep the data in **`./corpus`** on the VM, which compose mounts read-only at the same path inside the container, with the layout given in [Building your Application]({% link tracks/track-1/building-application.md %}#running-in-a-container).
- Pass the inference variables from `inference.env`, which compose reads through `env_file`. Read these exact variable names, do not hard-code them:

  | Variable | Value |
  |---|---|
  | `OPENAI_BASE_URL` | `{{ site.inference_base_url }}` |
  | `OPENAI_API_KEY` | your team's key, handed out at the kickoff |
  | `MODEL` | your default chat model, for example one of the [Recommended models]({% link infrastructure.md %}#recommended-models), used when a request has no `model` field |

## Before you submit

Check that:

- [ ] it is **running on your VM** and answers `/chat` and `/post/*` at `https://actm-teamN.{{ site.domain }}`;
- [ ] it reads the data path and inference variables from the environment (nothing hard-coded to one machine);
- [ ] responses follow the [I/O contract]({% link tracks/track-1/building-application.md %}#the-chat-endpoint);
- [ ] a top-level **`README.md`** notes anything non-obvious about your build.

## Handing in

By the freeze, **Thu 10.12, 23:59**, submit your application by filling out the [hand-in form]({{ site.handin_url }}):

Keep the repository **private**, but add `alexandersternfeld` as a collaborator.

## After the deadline

1. We check that your endpoint answers, and may run our own tests against it.
2. Your endpoint URL is passed on to your two attacking teams for the red phase (Fri 11.12 - Thu 17.12).
3. Your deployment is **frozen**: do not change the code, prompts, guardrails or data on your VM until the red phase has ended.