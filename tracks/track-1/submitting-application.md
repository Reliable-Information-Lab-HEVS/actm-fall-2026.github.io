---
title: Submitting Application
parent: "Track 1: Documentation Assistant"
nav_order: 4
layout: home
permalink: /tracks/track-1/submitting-application/
---

# Submitting Application

{: .summary }
> **In short:** you deploy your own application on your team VM. By the deadline it must be **running and reachable**, serving `/chat` and `/post/*` at `https://teamN.{{ site.domain }}`, and you submit it through the hand-in form along with your GitHub repository and the commit you tagged.

## You deploy it yourself

We do not clone or build your application. You run it on your team VM and keep it running:

```bash
docker compose up -d --build
```

`compose.yaml` already sets `restart: unless-stopped` on both containers, so your deployment comes back by itself after a reboot.

- Serve `/chat` and `/post/*` on port **8080 inside the container**, reachable through Caddy at `https://teamN.{{ site.domain }}` from the HSLU network, not only on `localhost`.
- Keep the data in **`./corpus`** on the VM, which compose mounts read-only at the same path inside the container, with the layout given in [Building your Application]({% link tracks/track-1/building-application.md %}#running-in-a-container).
- Pass the inference variables from `inference.env`, which compose reads through `env_file`. Read these exact variable names, do not hard-code them:

  | Variable | Value |
  |---|---|
  | `OPENAI_BASE_URL` | `{{ site.inference_base_url }}` |
  | `OPENAI_API_KEY` | your team's key, handed out at the kickoff |
  | `MODEL` | one of the ids in [Available models](/infrastructure/#available-models) |

## Before you submit

Check that:

- [ ] it is **running on your VM** and answers `/chat` and `/post/*` at `https://teamN.{{ site.domain }}`;
- [ ] it reads the data path and inference variables from the environment (nothing hard-coded to one machine);
- [ ] responses follow the [I/O contract]({% link tracks/track-1/building-application.md %}#the-chat-endpoint);
- [ ] your tool seed (`tools_seed_data.json`) is in your image, and the corpus `.txt` files are read from `/corpus`;
- [ ] a top-level **`README.md`** notes anything non-obvious about your build.

## Handing in

By the freeze, **Thu 10.12, 23:59**, submit through the [hand-in form]({{ site.handin_url }}):

- the **URL of your running endpoint**, `https://teamN.{{ site.domain }}`,
- your **GitHub repository URL**,
- the **commit hash** you tagged `v1`, and
- your **final threat model** (see [Threat Modelling]({% link threat-modelling.md %})).

Keep the repository **private** and add the teaching team as collaborators, so other teams cannot read your code before the red phase is over.

The repository is how we verify and judge what you built; the URL is what gets attacked.

## After the deadline

1. We check that your endpoint answers, and run the acceptance battery against it.
2. Your endpoint URL is passed on to your two attacking teams for the red phase (Fri 11.12 - Thu 17.12).
3. Your deployment is **frozen**: do not change the code, prompts, guardrails or data on your VM until the red phase has ended.

After the red phase you may patch your application for the presentation. If you do, tag the commit as `final` and describe the changes in your report; the endpoint URL stays the same.
