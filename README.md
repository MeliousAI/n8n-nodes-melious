# n8n-nodes-melious

An [n8n](https://n8n.io) community node for [Melious](https://melious.ai) — OpenAI-compatible chat models served on European infrastructure (GDPR/TTDSG).

Melious already works in n8n through the OpenAI Chat Model node's Base URL field. This package adds a native **Melious Chat Model** sub-node so the base URL is pre-filled and, more usefully, so the model dropdown only offers models that can actually serve a chat request — Melious serves chat, embedding, image, audio and guardrail models from one endpoint, and the generic OpenAI node lists all of them.

[Installation](#installation) · [Credentials](#credentials) · [Usage](#usage) · [Models](#models) · [Compatibility](#compatibility) · [Resources](#resources)

## Installation

Follow the [community nodes installation guide](https://docs.n8n.io/integrations/community-nodes/installation/).

In n8n, go to **Settings → Community nodes → Install**, enter `n8n-nodes-melious`, and confirm.

## Credentials

You need a Melious API key. Create one at [melious.ai](https://melious.ai); keys start with `sk-mel-`.

In n8n, go to **Credentials → New → Melious**, paste the key, and save. n8n verifies it by calling `GET /v1/models` — a green check means you're connected. The base URL is set for you.

## Usage

Add a **Melious Chat Model** node and connect it to any node that accepts a language model — **AI Agent**, **Basic LLM Chain**, **Question and Answer Chain**, and so on.

Pick a model from the dropdown (searchable) or switch the field to **By ID** and type a model ID directly.

### Options

| Option | Default | Notes |
| --- | --- | --- |
| Sampling Temperature | `0.7` | Lower is more deterministic |
| Maximum Number of Tokens | `-1` | `-1` lets the model decide |
| Top P | `1` | Prefer adjusting this *or* temperature, not both |
| Frequency Penalty | `0` | Discourages repeating the same line |
| Presence Penalty | `0` | Encourages new topics |
| Response Format | `text` | `json_object` enables JSON mode — include the word "json" in your prompt |
| Timeout | `360000` | Milliseconds |
| Max Retries | `2` | |

## Models

The dropdown lists only chat-capable models, filtered on the `_meta.type` field returned by `GET /v1/models?include_meta=true`.

For tool calling — which the AI Agent node needs — prefer a model reporting `_meta.capabilities.function_calling`. `glm-5.1` is a solid default and is what the node preselects. Browse the full catalogue at [melious.ai/hub/models](https://melious.ai/hub/models).

For embeddings, use n8n's **Embeddings OpenAI** node with an OpenAI credential pointed at `https://api.melious.ai/v1` and a model such as `bge-m3`. This package does not ship an embeddings node.

## Compatibility

Tested against n8n 2.35. Requires an n8n version that supports the AI Node SDK.

Works with **AI Agent** node versions 2 and 3. It will not appear in the model connector of the legacy **Agent V1** node, which filters against a fixed list of built-in model nodes.

## Resources

- [Melious n8n integration guide](https://melious.ai/docs/integrations/n8n)
- [Melious API reference](https://melious.ai/docs/reference)
- [n8n community nodes documentation](https://docs.n8n.io/integrations/community-nodes/)

## License

[MIT](LICENSE)
