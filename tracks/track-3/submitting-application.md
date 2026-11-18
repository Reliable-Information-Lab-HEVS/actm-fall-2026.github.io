---
title: Submitting Application
parent: "Track 3: Air Threat Advisor"
nav_order: 4
layout: home
permalink: /tracks/track-3/submitting-application/
---

# Submitting Application

{: .summary }
> **In short:** you deploy your own application on your team VM. By the deadline it must be running and reachable, serving `/advise` and `/message` at `https://actm-teamN.{{ site.domain }}`, and you submit it through the hand-in form along with your GitHub repository and the commit you tagged.

## You deploy it yourself

We do not clone or build your application. You run it on your team VM and keep it running:

```bash
docker compose up -d --build
```

- Serve `/advise` and `/message` on port 8080 inside the container, reachable through Caddy at `https://actm-teamN.{{ site.domain }}` from the HSLU network.
- Pass the inference variables from `inference.env`, which compose reads through `env_file`.


## Before you submit

Check that:

- [ ] it is running on your VM and answers `/advise` and `/message` at `https://actm-teamN.{{ site.domain }}`;
- [ ] responses follow the [I/O contract]({% link tracks/track-3/building-application.md %}#the-advise-endpoint);
- [ ] a message posted to `/message` is still visible to `/advise` afterwards;
- [ ] a top-level **`README.md`** notes anything non-obvious about your build.

## Handing in

By the freeze, **Thu 10.12, 23:59**, submit your application through the [hand-in form]({{ site.handin_url }}).

Keep the repository **private** and add only the user `alexandersternfeld` as a collaborator. 


## After the deadline

1. We check that your endpoint answers, and may run our own tests against it.
2. Your endpoint URL is passed on to your two attacking teams for the red phase (Fri 11.12 - Thu 17.12).
3. Your deployment is frozen: do not change the code, prompts, guardrails or data on your VM until the red phase has ended.

---

Next: [Extensions]({% link tracks/track-3/Extensions.md %}) →
