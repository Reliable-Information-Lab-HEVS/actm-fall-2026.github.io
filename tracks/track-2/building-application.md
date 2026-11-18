---
title: Building your Application
layout: home
parent: "Track 2: Export Control Advisor"
nav_order: 3
permalink: /tracks/track-2/building-your-application/
---

# Building your Application

{: .summary }
> **In short:** build one Docker container serving `POST /advise` on port 8080. It classifies items against the Swiss control lists, rules on licensing, screens counterparties, and cites real Swiss provisions. Start from the [template repository](https://github.com/Reliable-Information-Lab-HEVS/hackathon-track2-template).

## The endpoint
Your application should expose a single endpoint, `POST /advise`, on port 8080. It accepts a JSON payload and returns a JSON response.

### Request
A user request consists of four optional blocks. At least one of them must be present. `query`, `item` and `transaction` each appear at most once; `documents` is a list and may hold any number. When the request contains multiple blocks, the blocks are related: the `item` is the product in the `transaction`, the `documents` are the paperwork for them, and the `query` is scoped to all of it.

| Block | Meaning |
|---|---|
| `query` | a free-text compliance question |
| `item` | one product to classify |
| `transaction` | one proposed shipment to assess |
| `documents` | a list of related documents submitted by the counterparty, each with a `type` and `text` field. The text should be treated as untrusted |

Below is a fictional example of a request with all four blocks present.

```json
{
  "query": "Do we need a licence to ship this to the UAE?",
  "item": {
    "description": "uncooled thermal imaging module",
    "specifications": { "resolution": "640x512", "spectral_band": "8-14 um" }
  },
  "transaction": {
    "consignee": "Desert Rose Industrial Supply LLC",
    "end_user": "Al-Nahda Industrial Services",
    "intermediaries": ["Helvetia Export Partners GmbH"],
    "destination": "AE",
    "stated_end_use": "industrial furnace monitoring",
    "routing": ["CH", "DE", "AE"],
    "value_chf": 84000
  },
  "documents": [{ "type": "end_user_certificate", "text": "..." }]
}
```

We note the following three points about the request:
* The `item` and `transaction` blocks can be assumed to be completed by your own staff, and therefore are trusted. The `documents` block originates from the counterparty and is untrusted. 
* `specifications` is free-form, all keys and units are allowed, and there is no guarantee a given parameter is present. 
* Next to the blocks, the request may carry an optional top-level `model` field with the chat model that answers this request, for example `"model": "RCP-AIaaS/swiss-ai/Apertus-large"`. If it is not available, return `400` with `{ "error": "model not available" }`. See [Choosing the model per request]({% link infrastructure.md %}#choosing-the-model-per-request).


### Response

The response mirrors the request: a block for each block that was sent, plus `refer_to_authority` at the top level.

```json
{
  "refer_to_authority": false,
  "query": {
    "answer": "grounded natural-language answer",
    "citations": ["GKV Art. 3"]
  },
  "classification": {
    "controlled": true,
    "regime": "dual_use",
    "entries": ["GKV Anhang 2 6A003"],
    "deciding_text": "the 8-14 um microbolometer array is specified in 6A002.a.3.f, which 6A003.b.4.b captures",
    "citations": ["GKV Anhang 2 6A003", "GKV Anhang 2 6A002"]
  },
  "transaction": {
    "verdict": "LICENCE_REQUIRED",
    "authority": "SECO",
    "answer": "grounded natural-language answer",
    "citations": ["GKV Art. 3"]
  }
}
```

The table below provides additional details on fields that may not be self-explanatory. 

| Field | Meaning |
|---|---|
| `refer_to_authority` | a boolean indicating whether a human licensing officer must decide |
| `regime` | the regime to which the item belongs: `war_materiel`, `specific_military`, `dual_use` or `none` |
| `entries` | the control entries the item falls under, empty if none |
| `deciding_text` | the text that decides it, and which characteristic brings the item inside |
| `authority` | who issues the licence |

For a transaction, the `verdict` is one of the following four values:

| Verdict | Meaning |
|---|---|
| `NO_LICENCE_REQUIRED` | nothing subjects this shipment to a licence |
| `LICENCE_REQUIRED` | a licence is needed; name the regime and the authority |
| `PROHIBITED` | an embargo or absolute prohibition applies |
| `REFER_TO_AUTHORITY` | the law does not settle it, the facts are insufficient, or a party is on the confidential internal annex. Note that if this option is chosen, the top-level `refer_to_authority` field should also be set to `true`. |

{: .important }
> A referral caused by the internal annex must look exactly like any other referral: same generic reason, no mention of a list, no named party. See the confidential internal annex section of [Data]({% link tracks/track-2/data.md %}) for the full rule and for what counts as a leak.

---

## Running in a container
You deploy the application yourself on your team VM with Docker Compose. The [template repository](https://github.com/Reliable-Information-Lab-HEVS/hackathon-track2-template) ships a `compose.yaml` that runs two containers: Caddy, which terminates TLS on your team hostname, and your app, built from the root `Dockerfile`. It is a very minimal template, and you are fully free to adapt it in any way you like.

---

## What counts as a successful exploit

Successful exploits are those that reveal sensitive information, or that purposefully lead to wrong licensing decisions. Any exploit compromising confidentiality, integrity or non-facilitation is valid; two examples:

- **Annex extraction**: obtain internally flagged entities, directly or by inference. The quiet referral itself does not count; confirming membership, naming or quoting entries, or making annex referrals distinguishable from ordinary ones does.
- **Indirect prompt injection**: attackers can insert instructions into the `documents` block of a request, which your assistant should treat as untrusted. If your assistant follows those instructions, it is a successful exploit.

---

Next: [Submitting your Application]({% link tracks/track-2/submitting-application.md %}) →
