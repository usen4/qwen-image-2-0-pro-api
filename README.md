# Qwen Image 2.0 Pro API (`qwen-image-2.0-pro`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.05 per image** on the APIMart route — one OpenAI-compatible endpoint at `https://api.apimart.ai/v1`, no monthly plan required. *(observed 2026-10-12)*

**[Get an API key](https://go.apimart.ai/k-a8e40d)** · **[Live pricing](https://go.apimart.ai/k-497d55)** · **[Model page](https://go.apimart.ai/k-a3569b)** · [⚡ 60-second quickstart](#quickstart)

**Why teams call Qwen Image 2.0 Pro (`qwen-image-2.0-pro`) through APIMart**

- **One key, entire catalog.** The same `https://api.apimart.ai/v1` base URL and `Authorization` header reach Qwen Image 2.0 Pro (`qwen-image-2.0-pro`) and 300+ other image, video and language models — switch the `model` field, not your client.
- **$1 minimum, pay as you go.** No subscription and no prepaid plan to size up front: top up from $1 and spend it on calls. There is no free quota to burn through first, so the price in this table is the price you pay.
- **The charge comes back in the response.** Every call reports the amount billed (`cost` / `credits_cost`), so a spend number is read per call instead of guessed at month end.
- **Async by design.** Submit, take the `task_id`, poll `GET /v1/tasks/{id}` — batching and retries are ordinary queue work, not a bespoke integration.

<!-- /conv-kit:v1 -->



Qwen Image 2.0 Pro delivers powerful AI image generation capabilities. Create high-quality images from text prompts or transform existing images with Qwen Image 2.0 Pro.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `qwen-image-2.0-pro` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-a3569b) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.0625 | 0.05 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-497d55) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-a3569b) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"qwen-image-2.0-pro","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "qwen-image-2.0-pro", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**What is the Qwen Image 2.0 Pro model id?**

`qwen-image-2.0-pro`. See the route table above.

**How is it billed?**

**from $0.05 per image** on the APIMart route. The completed task reports the exact amount in its `cost` field; a failed task is refunded.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-a8e40d) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [qwen-image-2-0-pro-prompts](https://github.com/usen4/qwen-image-2-0-pro-prompts)
- **Same model, other languages** — 10 more language builds: 中文 [qwen-image-2-0-pro-api-cn](https://github.com/f6l9dj/qwen-image-2-0-pro-api-cn) · Deutsch `qwen-image-2-0-pro-api-de` · Español [qwen-image-2-0-pro-api-es](https://github.com/w986aot4hmq3w/qwen-image-2-0-pro-api-es) · Français [qwen-image-2-0-pro-api-fr](https://github.com/pgew1l/qwen-image-2-0-pro-api-fr) · Relay/gateway `qwen-image-2-0-pro-api-gateway` · Bahasa [qwen-image-2-0-pro-api-id](https://github.com/is-g7vdb4/qwen-image-2-0-pro-api-id) · 日本語 [qwen-image-2-0-pro-api-ja](https://github.com/q-ul1h/qwen-image-2-0-pro-api-ja) · 한국어 `qwen-image-2-0-pro-api-ko` · Português `qwen-image-2-0-pro-api-pt` · Русский [qwen-image-2-0-pro-api-ru](https://github.com/nu0b4nr5wn3aw/qwen-image-2-0-pro-api-ru)
- **More models in this language** — [grok-image-api](https://github.com/hpcsd0apt/grok-image-api) · [grok-imagine-image-quality-api](https://github.com/xt5chw51l/grok-imagine-image-quality-api) · [seedream-4-0-api](https://github.com/qela34r8sb7/seedream-4-0-api) · [wan-2-7-image-pro-api](https://github.com/u3cdzay/wan-2-7-image-pro-api) · [z-image-api](https://github.com/t917b/z-image-api) · [minimax-h3-api](https://github.com/apimartchlp3td/minimax-h3-api)
- **Topic hubs** — every build of this model, all languages: [qwen-image-2-0-pro](https://github.com/topics/qwen-image-2-0-pro) · [qwen-image-2-0-pro-api](https://github.com/topics/qwen-image-2-0-pro-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-a3569b> | `apimart.ai/model/qwen-image-2-0-pro` |
| Live pricing | <https://go.apimart.ai/k-497d55> | `apimart.ai/pricing` |
| Get an API key | <https://go.apimart.ai/k-a8e40d> | `apimart.ai/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Qwen Image 2.0 Pro is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
