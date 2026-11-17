---
title: Infrastructure
nav_order: 7
layout: home
permalink: /infrastructure/
---

# Infrastructure

{: .summary }
> **In short:** each team gets an Ubuntu VM with Docker on the HSLU network. You deploy your application on it yourself, behind Caddy on port 443, at `https://actm-teamN.{{ site.domain }}`. All teams use the same OpenAI-compatible inference endpoint, with one API key per team.

---

## Your team VM

| | |
|---|---|
| **OS** | Ubuntu 26.04 LTS |
| **Resources** | 4 vCPUs, 7.3 GB RAM, 18 GB disk (about 11 GB free) |
| **Software** | Docker and Docker Compose |
| **Hostname** | `actm-teamN.{{ site.domain }}`, with `N` your team number |
| **Login** | `ssh labadmin@actm-teamN.{{ site.domain }}`, with SSH keys only |

{: .important }
> The VMs are reachable **only from the HSLU network**: HSLU Wi-Fi on campus, or the HSLU VPN from elsewhere. If a hostname does not resolve, check that you are connected to one of the two.

### SSH access

Password login over SSH is disabled. Send the teaching team the **public** key of each team member (the `.pub` file) before the kickoff; we add it to your VM. If you do not have a key yet:

```bash
ssh-keygen -t ed25519 -C "your-name-actm"
# send us the content of ~/.ssh/id_ed25519.pub, never the file without .pub
```

To make logging in easier, add this to `~/.ssh/config` on your laptop:

```
Host actm
    HostName actm-teamN.{{ site.domain }}
    User labadmin
    IdentityFile ~/.ssh/id_ed25519
```

Then `ssh actm` is enough.

The `labadmin` user has `sudo` rights; the password is handed to your team at the kickoff. **The VM is yours to break, but if you break it, you rebuild it**: keep everything you need in your Git repository.

### Network

| Port | Open to | Use |
|---|---|---|
| 22 | HSLU network | SSH |
| 443 | HSLU network and the other VMs | **your application**, through Caddy |
| 8080 | HSLU network and the other VMs | for testing only; your submitted endpoint is on 443 |

All other inbound ports, including 80, are closed. Outbound HTTPS works (PyPI, Docker Hub, GitHub, Hugging Face, the inference endpoint).

{: .warning }
> HSLU's network protection automatically throttles excessive traffic. Keep your own load tests small, and respect the rate limits in the [rules of engagement]({% link general-information.md %}#rules-of-engagement) during the red phase.

---

## Deploying with Caddy

Your VM already has a TLS certificate for its own hostname, `actm-teamN.{{ site.domain }}`, under `/etc/tls/actm/` (`fullchain.pem` and `privkey.pem`). The track templates ship a `compose.yaml` with two containers: **Caddy**, which terminates TLS on port 443 using that certificate, and **your app**, which Caddy reaches as `app:8080` on the internal Docker network.

Before the first start, make two changes:

1. In the `Caddyfile`, replace `N` in `actm-teamN.{{ site.domain }}` with your team number.
2. Copy `inference.env.example` to `inference.env` and fill in your API key and your model. The endpoint `{{ site.inference_base_url }}` is already set.

Then:

```bash
docker compose up -d --build
docker compose ps            # both containers up, app healthy
curl https://actm-teamN.{{ site.domain }}/health
```

{: .warning }
> Do not delete or replace the certificate under `/etc/tls/actm/`. It is provisioned for you, and it cannot be re-issued from the VM.

---

## Inference endpoint

All teams use the same OpenAI-compatible endpoint, the [Swiss AI Serving](https://serving.swissai.svc.cscs.ch/) platform at CSCS, with the same recommended models, so the contest turns on your scaffolding rather than on who picked the bigger model.

| | |
|---|---|
| **Endpoint** | `{{ site.inference_base_url }}` |
| **API key** | one per team, handed out at the kickoff |
| **Rate limit** | about 15 requests per minute per key for the models below |

{: .warning }
> The rate limit applies to every call your application makes, including embeddings, reranking and any extra LLM calls in your guardrails. Design your pipeline so that one user question does not burn through the whole minute's budget.

Read `OPENAI_BASE_URL`, `OPENAI_API_KEY` and `MODEL` from the environment. Do not hard-code them, and never commit the key: `inference.env` is in the template's `.gitignore`. Because the endpoint is OpenAI-compatible, the `openai` Python SDK picks up the first two by itself.

### Recommended models

Use the **exact** ids below, including the prefix before the first `/`. Without it the endpoint returns an error.

**Chat models**, called on `/chat/completions`:

| Model id | Notes |
|---|---|
| `CSCS-Inference/google/gemma-4-31B-it` | fast, good default |
| `RCP-AIaaS/swiss-ai/Apertus-large` | the Swiss open LLM |

For grounded, extractive answers you will usually want a `temperature` well below the default. If your design relies on tool calling (`tools` in the request), check early that the model you pick handles it reliably.

**Embedding model**, called on `/embeddings`:

| Model id | Dimensions |
|---|---|
| `RCP-AIaaS/Qwen/Qwen3-Embedding-8B` | 4096 |

**Reranker** (optional): `RCP-AIaaS/Qwen/Qwen3-Reranker-8B`. See the reranking section of [RAG]({% link rag.md %}) for when a reranker helps.

You can also run a small embedding or reranking model locally on your VM, for example with `sentence-transformers`; with 4 CPUs and no GPU, keep it small.

### Other models

You may try any other model the endpoint serves. List them with:

```bash
curl -H "Authorization: Bearer $OPENAI_API_KEY" {{ site.inference_base_url }}/models
```

or browse them on [serving.swissai.svc.cscs.ch](https://serving.swissai.svc.cscs.ch/). If you deploy something other than the recommended models, say which model and why in your threat model. Some models are reasoning models whose responses carry a `reasoning_content` field next to `content`: read the answer from `content`, and do not show the reasoning to a user or feed it back as context without thinking about what is in it.

### A first request

```python
from openai import OpenAI
import os

client = OpenAI()  # reads OPENAI_BASE_URL and OPENAI_API_KEY
reply = client.chat.completions.create(
    model=os.environ["MODEL"],
    messages=[{"role": "user", "content": "Say hello in five words."}],
    temperature=0.2,
)
print(reply.choices[0].message.content)
```

---

## Using AI coding tools

You can use any of the models on the endpoint for coding assistance, or your own subscriptions. For the hosted models, an existing framework such as [OpenCode](https://opencode.ai/) works well. See [Using AI tools]({% link general-information.md %}#using-ai-tools) for the course rules.
