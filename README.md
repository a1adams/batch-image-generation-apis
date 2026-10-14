# Batch Image Generation API Options: 8 Providers on Batch and Webhooks

This list compares 8 batch image generation API options, each checked against its vendor's docs on 11 October 2026 and ranked by how much of a batch one request covers and how safely it can be retried. Wireflow is first because one request runs a whole saved batch workflow, and a retry with the same key replays the original run. Of the 8, 2 state a 50% batch discount.

1. **Wireflow**: Best overall for running a multi-step batch from one request, a node canvas whose saved workflows run by REST API
2. **OpenAI API**: Best for large batches that can wait a day, a Batch API that takes a file of image requests
3. **Google Gemini API**: Best for image batches on Google models, a Batch API with webhooks and 6 weeks of result storage
4. **Runware**: Best for many image tasks in one request, a task API where every request is an array
5. **fal**: Best for a per-image queue across many hosted models, a queue API with status URLs and webhooks
6. **Replicate**: Best for a per-image queue over open and official models, predictions you poll or receive by webhook
7. **Black Forest Labs API**: Best for calling FLUX models directly, an asynchronous API that returns a polling URL
8. **Leonardo AI**: Best for outputs that should stay hosted, an image API with a webhook callback on the key

The Wireflow team maintains this list, so read entry 1 with that in mind. [Wireflow's node-based image generation page](https://www.wireflow.ai/node-based-image-generation) shows the canvas that entry 1 describes, and every other entry links to its vendor's docs so that each cell can be checked.

The tables below are generated from [`data/tools.json`](data/tools.json). Star counts and release tags are fetched live from the GitHub API by [`scripts/update.js`](scripts/update.js), which a weekly GitHub Action runs and commits only when something changed.

<!-- LAST-CHECKED:START -->
Live repository data last checked **2026-10-14** by [`scripts/update.js`](scripts/update.js), which runs weekly via GitHub Actions.
<!-- LAST-CHECKED:END -->

Maintained by [a1adams](https://github.com/a1adams). Corrections are welcome, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Contents

- [The data](#the-data)
- [Capability scores](#capability-scores)
- [Comparison table](#comparison-table)
- [The tools](#the-tools)
  - [Wireflow](#1-wireflow)
  - [OpenAI API](#2-openai-api)
  - [Google Gemini API](#3-google-gemini-api)
  - [Runware](#4-runware)
  - [fal](#5-fal)
  - [Replicate](#6-replicate)
  - [Black Forest Labs API](#7-black-forest-labs-api)
  - [Leonardo AI](#8-leonardo-ai)
- [What each check means](#what-each-check-means)
- [A check to run before a real batch](#a-check-to-run-before-a-real-batch)
- [How this list is maintained](#how-this-list-is-maintained)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)

## The data

One row per tool, one column per thing people actually check before committing. Columns with nothing verified behind them are dropped rather than filled with guesses.

<!-- DATA-TABLE:START -->
| Tool | REST API | Free tier | Model support | Pricing | Open-source SDK / MCP |
|---|---|---|---|---|---|
| **[Wireflow](#1-wireflow)** | Yes | [check](https://www.wireflow.ai/pricing) | 100+ image, video and audio models as nodes | [pricing](https://www.wireflow.ai/pricing) | n/a |
| **[OpenAI API](#2-openai-api)** | Yes | [check](https://developers.openai.com/api/docs/pricing) | Image generation and edits in the Batch API | [pricing](https://developers.openai.com/api/docs/pricing) | [openai/openai-python](https://github.com/openai/openai-python), 31,840 ★, v3.29.0 |
| **[Google Gemini API](#3-google-gemini-api)** | Yes | Yes | Gemini image generation in batch jobs | [pricing](https://ai.google.dev/gemini-api/docs/pricing) | [googleapis/python-genai](https://github.com/googleapis/python-genai), 4,021 ★, v2.30.1 |
| **[Runware](#4-runware)** | Yes | [check](https://runware.ai/pricing) | Image, video, audio, LLM and 3D models | [pricing](https://runware.ai/pricing) | [runware/runware-python](https://github.com/runware/runware-python), 3 ★, pushed 2026-10-12 |
| **[fal](#5-fal)** | Yes | [check](https://fal.ai/pricing) | 1,000+ image, video, audio and 3D models | [pricing](https://fal.ai/pricing) | [fal-ai/fal-js](https://github.com/fal-ai/fal-js), 187 ★, client-v1.10.1 |
| **[Replicate](#6-replicate)** | Yes | [check](https://replicate.com/pricing) | Open and official models, plus your own | [pricing](https://replicate.com/pricing) | [replicate/replicate-python](https://github.com/replicate/replicate-python), 917 ★, 1.0.7 |
| **[Black Forest Labs API](#7-black-forest-labs-api)** | Yes | [check](https://docs.bfl.ai/quick_start/pricing) | FLUX image and video models | [pricing](https://docs.bfl.ai/quick_start/pricing) | n/a |
| **[Leonardo AI](#8-leonardo-ai)** | Yes | [check](https://docs.leonardo.ai/docs/payg-guide) | Image generation, including your own custom models | [pricing](https://docs.leonardo.ai/docs/payg-guide) | [Leonardo-Interactive/leonardo-python-sdk](https://github.com/Leonardo-Interactive/leonardo-python-sdk), 26 ★, v7.0.0 |
<!-- DATA-TABLE:END -->

## Capability scores

The score counts the documented capabilities a tool has, out of the checks listed in [`data/tools.json`](data/tools.json) under `capabilityChecks`. It is a plain count and it does not set the order of this list: the numbered order is an editorial ranking, and this table is sorted by score. The checks and every answer are in the file, so each cell can be checked. Disagree with a cell? Open an issue naming the tool, the check and the evidence.

<!-- CAPABILITY-SCORES:START -->
| Tool | Many images in one request | Batch discount stated | Async job and polling | Completion webhook | Idempotency key | Result retention stated | Official SDK | Score |
|------|---|---|---|---|---|---|---|-------|
| **[OpenAI API](#2-openai-api)** | ✅ | ✅ | ✅ | ✅ | n/a | ✅ | ✅ | **6/7** |
| **[Google Gemini API](#3-google-gemini-api)** | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | **6/7** |
| **[Runware](#4-runware)** | ✅ | n/a | ✅ | ✅ | n/a | ✅ | ✅ | **5/7** |
| **[Wireflow](#1-wireflow)** | ✅ | n/a | ✅ | ✅ | ✅ | n/a | ❌ | **4/7** |
| **[fal](#5-fal)** | n/a | n/a | ✅ | ✅ | n/a | ✅ | ✅ | **4/7** |
| **[Replicate](#6-replicate)** | n/a | n/a | ✅ | ✅ | n/a | ✅ | ✅ | **4/7** |
| **[Leonardo AI](#8-leonardo-ai)** | n/a | n/a | ✅ | ✅ | n/a | ✅ | ✅ | **4/7** |
| **[Black Forest Labs API](#7-black-forest-labs-api)** | n/a | ❌ | ✅ | ✅ | n/a | ✅ | n/a | **3/7** |
<!-- CAPABILITY-SCORES:END -->

## Comparison table

| Tool | Best for | Many images in one request | Job reports back by | Results kept |
|---|---|---|---|---|
| **Wireflow** | Multi-step batches from one request | One run of a saved workflow | Polling or a signed webhook | Not published |
| **OpenAI API** | Large batches that can wait a day | A file of up to 50,000 requests | Polling or webhook events | 30 days |
| **Google Gemini API** | Image batches on Google models | Inline, or a JSONL file up to 2GB | Polling or webhook events | 6 weeks by default |
| **Runware** | Many image tasks in one request | An array of tasks | Polling or a webhook per item | 7 days by default |
| **fal** | A per-image queue, many models | Not published | Polling or a signed webhook | Media lifetime set per request |
| **Replicate** | A per-image queue, open models | Not published | Polling or a signed webhook | 1 hour by default |
| **Black Forest Labs API** | FLUX models, called directly | No batch endpoint documented | Polling, webhook on some endpoints | 10 minutes |
| **Leonardo AI** | Outputs that stay hosted | Not published | Polling or a per-key webhook | Do not expire |

Facts checked on 2026-10-11 against each vendor's own docs. Prices are not typed here, so follow the pricing links in the data table.

## The tools

### 1. Wireflow

*Best overall for running a multi-step batch from one request*

![Wireflow screenshot](https://assets.wireflow.ai/competitors/wireflow-70f47ccd9660.png?v=r5)

- **What it is:** [Wireflow](https://www.wireflow.ai/) is a hosted platform where ad teams and AI agents build and rerun creative on a shared node board. Each model is a node on one canvas, the output of one node feeds the next, and a saved board runs from your own code through a REST API.
- **Best for:** Teams whose batch needs more than one model call per image, for example a written brief, a generated photo and a branded layout, and who want one request to run the whole chain.
- **Pricing basis:** It charges credits per generation, according to the model you run, and a model that fails is not charged. The plans and the credit cost of each model are on the [Wireflow pricing page](https://www.wireflow.ai/pricing), checked October 2026. A batch discount is not published.
- **Standout:** One request runs a whole saved workflow. The batch guide's pattern chains an LLM, a Loop into an image generation node and a Compositor in batch mode, so one run produces a set of finished, on-brand images. The Compositor renders up to 50 rows per batch, and that render costs no credits. Source: [the batch image generation guide](https://www.wireflow.ai/docs/batch-image-generation).
- **Limit:** The API starts one run per request, so there is no bulk submit endpoint that takes a file of items, and the Compositor stops at 50 rows per batch render. There is no official SDK. The free plan lets you build up to 5 workflows, and running generations needs a paid plan. The `Idempotency-Key` replay works only on `POST /api/v1/workflows/{id}/run`, and a retry on `/execute` starts and bills a second run, as [the executions reference](https://www.wireflow.ai/docs/api/executions) explains. A retention period for run outputs is not published.
- **API or automation:** You send a saved workflow's input values to `POST /api/v1/workflows/{id}/run`, which returns 202 with an execution ID to poll. A webhook block on the same request makes Wireflow POST the result when the run finishes. Deliveries can be signed with HMAC-SHA256 and are retried for up to 12 attempts, and a metadata object is echoed back so you can match the result to your own job. See [the run endpoint reference](https://www.wireflow.ai/docs/api/run).
- **Sources:** the Wireflow homepage, pricing page, batch guide, executions reference and run reference linked in this entry, checked 2026-10-11.
- **Links:**
  - [Wireflow node based image generation page](https://www.wireflow.ai/node-based-image-generation)
  - [Docs](https://www.wireflow.ai/docs/api/overview)

Pick Wireflow when each finished image takes more than one model call and you want the chain, the callback and the retry rule in one place. If you only need thousands of single-prompt images at a stated discount, entries 2 and 3 have a batch job built for that.

Run a saved workflow from code: send its input values to this endpoint with an API key, then poll the execution or add a webhook block. The request format is in the API reference linked under Docs.
```text
POST /api/v1/workflows/{id}/run
```

### 2. OpenAI API

*Best for large batches that can wait a day*

![OpenAI API screenshot](https://assets.wireflow.ai/competitors/openai-0dd25d6d038b.png?v=r5)

- **What it is:** The [OpenAI API](https://openai.com/api/) gives developers access to OpenAI models, including image generation. Its Batch API takes a file of requests, runs them asynchronously and returns the results as a file.
- **Best for:** Developers with a large image job on OpenAI models that can wait up to a day for its results.
- **Pricing basis:** It charges per request, and image prices are per token, so the cost of one image depends on the model and its settings. Standard and Batch rates are on the [OpenAI pricing page](https://developers.openai.com/api/docs/pricing), checked October 2026.
- **Standout:** The Batch API takes a `.jsonl` file whose supported endpoints include `/v1/images/generations` and `/v1/images/edits`. The guide states a 50% cost discount compared with the synchronous APIs. A single batch may include up to 50,000 requests in a file of up to 200 MB. Source: [the OpenAI Batch guide](https://developers.openai.com/api/docs/guides/batch).
- **Limit:** Each batch completes within 24 hours, and the completion window can only be set to `24h`. An input file can only include requests to a single model. The output file is deleted 30 days after the batch completes. An idempotency key for creating a batch is not published, and the `custom_id` on each request is for matching results to requests.
- **API or automation:** You upload the file, create the batch and retrieve it to check its status. Webhooks send `batch.completed`, `batch.failed`, `batch.expired` and `batch.cancelled` events, and a failed delivery is retried for up to 72 hours, as [the OpenAI webhooks guide](https://developers.openai.com/api/docs/guides/webhooks) describes.
- **Note:** The linked repository is the official Python SDK. The Batch API is a hosted service.
- **Sources:** the Batch guide and webhooks guide linked above, and [the OpenAI libraries page](https://developers.openai.com/api/docs/libraries), which lists the official SDKs, checked 2026-10-11.
- **Links:**
  - [openai/openai-python](https://github.com/openai/openai-python)

Pick the OpenAI API over Wireflow when every image is one prompt on an OpenAI model and a day's wait is acceptable. Download the output file well inside the 30 days.

Create a batch from an uploaded file, from the OpenAI Batch guide. The guide's example targets /v1/chat/completions. For images, set the endpoint to /v1/images/generations or /v1/images/edits, which the guide lists as supported.
```bash
curl https://api.openai.com/v1/batches \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "input_file_id": "file-abc123",
    "endpoint": "/v1/chat/completions",
    "completion_window": "24h"
  }'
```

### 3. Google Gemini API

*Best for image batches on Google models*

![Google Gemini API screenshot](https://assets.wireflow.ai/competitors/google-gemini-b087c7f5788e.png?v=r5)

- **What it is:** The [Google Gemini API](https://ai.google.dev/gemini-api/docs) gives developers access to Google's Gemini models, including image generation models. Its Batch API runs a set of requests asynchronously and keeps the results for you to download.
- **Best for:** Developers who generate images on Google models and want the results kept for weeks, not days.
- **Pricing basis:** It charges per request, and the Batch API is priced at 50% of the standard cost. Rates for each model are on the [Gemini API pricing page](https://ai.google.dev/gemini-api/docs/pricing), checked October 2026.
- **Standout:** The batch guide has its own section on generating images in batch, and it recommends a JSONL input file for image generation. An input file can be up to 2GB, and results are stored for 6 weeks by default. Source: [the Gemini Batch API guide](https://ai.google.dev/gemini-api/docs/batch-api).
- **Limit:** The guide says creating a batch job is not idempotent, so sending the same creation request twice creates two separate jobs. A job that is running or pending for more than 48 hours expires and has no results to retrieve. The target turnaround time is 24 hours.
- **API or automation:** You send inline requests for a total under 20MB, or upload a JSONL file, and then poll the job. Webhooks send `batch.succeeded`, `batch.failed`, `batch.cancelled` and `batch.expired` events, for a whole project or for one job, and failed deliveries are retried for 24 hours, as [the Gemini webhooks guide](https://ai.google.dev/gemini-api/docs/webhooks) describes.
- **Note:** The linked repository is the Google GenAI SDK for Python. The pricing page describes a free tier with limited access to certain models. Check that page for whether batch jobs are covered.
- **Sources:** the Batch API guide and webhooks guide linked above, and [the Gemini libraries page](https://ai.google.dev/gemini-api/docs/libraries), which lists the Google GenAI SDKs, checked 2026-10-11.
- **Links:**
  - [googleapis/python-genai](https://github.com/googleapis/python-genai)

Pick the Google Gemini API over the OpenAI API when your images come from Google models, or when results must stay downloadable for longer than 30 days. Record the job name before you retry a create call.

Create a batch job with inline requests, from the Gemini Batch API guide. The guide's image example adds a responseModalities setting to each request and recommends a JSONL file for image generation.
```bash
curl https://generativelanguage.googleapis.com/v1beta/models/gemini-3.8-flash:batchGenerateContent \
-H "x-goog-api-key: $GEMINI_API_KEY" \
-X POST \
-H "Content-Type:application/json" \
-d '{
    "batch": {
        "display_name": "my-batch-requests",
        "input_config": {
            "requests": {
                "requests": [
                    {
                        "request": {"contents": [{"parts": [{"text": "Describe the process of photosynthesis."}]}]},
                        "metadata": {
                            "key": "request-1"
                        }
                    },
                    {
                        "request": {"contents": [{"parts": [{"text": "Describe the process of photosynthesis."}]}]},
                        "metadata": {
                            "key": "request-2"
                        }
                    }
                ]
            }
        }
    }
}'
```

### 4. Runware

*Best for many image tasks in one request*

![Runware screenshot](https://assets.wireflow.ai/competitors/runware-1dad906299e3.png?v=r5)

- **What it is:** [Runware](https://runware.ai/) is an AI infrastructure platform with pay-as-you-go APIs for image, video, audio, LLM and 3D models. Every request to its Models API is an array of tasks, and each task is processed independently.
- **Best for:** Developers generating images in bulk through an API who want each result as soon as it is ready.
- **Pricing basis:** It charges per request, and you are charged only for successful requests. Rates for each model are on the [Runware pricing page](https://runware.ai/pricing), checked October 2026. A batch discount is not published.
- **Standout:** One call can carry many tasks, because every request is an array. In a batch, each completed item triggers its own webhook call, so results arrive one by one. Source: [the Runware Models API introduction](https://runware.ai/docs/models-api/introduction).
- **Limit:** Output URLs are retained for 7 days by default, and a `ttl` parameter on the request changes that. An idempotency key is not published. The `taskUUID` you generate matches responses to requests, and the webhook guide says to track it so that you can drop a duplicate delivery. The exact cost depends on the request parameters.
- **API or automation:** You set `deliveryMethod` to `async` and poll with the `getResponse` task, or you pass a `webhookURL` and receive each result by POST. Failed deliveries are retried with exponential backoff, and the webhook authenticates through parameters in its URL, as [the Runware webhooks guide](https://runware.ai/docs/models-api/webhooks) describes.
- **Note:** The linked repository is the official Python SDK. A TypeScript SDK is documented as well.
- **Sources:** the introduction and webhooks guide linked above, and [the Runware task polling guide](https://runware.ai/docs/models-api/task-polling), checked 2026-10-11.
- **Links:**
  - [runware/runware-python](https://github.com/runware/runware-python)

Pick Runware over the two batch jobs above when you cannot wait for a 24-hour window and you still want to send many tasks in one request.

Send one image task, from the Runware Models API introduction. The request body is an array, so more task objects can go in the same call.
```bash
curl https://api.runware.ai/v1 \
  -H "Authorization: Bearer $RUNWARE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '[
    {
      "taskType": "imageInference",
      "taskUUID": "a770f077-f413-47de-9dac-be0b26a35da6",
      "model": "xai:grok-imagine@image-quality",
      "positivePrompt": "A golden retriever on a park bench, cinematic lighting",
      "width": 1024,
      "height": 1024
    }
  ]'
```

### 5. fal

*Best for a per-image queue across many hosted models*

![fal screenshot](https://assets.wireflow.ai/competitors/fal-ai-0ec799965278.png?v=r5)

- **What it is:** [fal](https://fal.ai/) is a generative media platform for developers with 1,000+ image, video, audio and 3D models behind one API and SDKs. Each request enters a queue, and you poll it or receive the result by webhook.
- **Best for:** Developers who want pay-per-use access to many generative models and will send one request per image from their own worker.
- **Pricing basis:** It charges per request, pay per use, and the final cost shifts with resolution, duration and quality. Rates for each model are on the [fal pricing page](https://fal.ai/pricing), checked October 2026. A batch discount is not published.
- **Standout:** Requests in the queue are never dropped, and there is no queue size limit. If a runner fails during processing, the request is put back in the queue and retried up to 10 times. Source: [the fal queue docs](https://fal.ai/docs/documentation/model-apis/inference/queue).
- **Limit:** A batch endpoint that takes many generations in one request is not published, so a batch is many queued requests. An idempotency key for submitting a request is not published. Request inputs and outputs are stored for 30 days by default, and the lifetime of generated media is set per request, as [fal's data retention page](https://fal.ai/docs/documentation/model-apis/media-expiration) describes.
- **API or automation:** A submitted request returns a `request_id` with a `status_url` and a `response_url`. Pass a webhook URL and fal POSTs the result when processing completes. The request is signed, and a failed delivery is retried up to 31 times. See [the fal webhooks docs](https://fal.ai/docs/documentation/model-apis/inference/webhooks).
- **Note:** The linked repository is the official JavaScript client. The Python client lives in fal-ai/fal.
- **Sources:** the queue, data retention and webhooks pages linked above, and [fal's libraries page](https://fal.ai/docs/documentation/setting-up/resources), which lists the official clients, checked 2026-10-11.
- **Links:**
  - [fal-ai/fal-js](https://github.com/fal-ai/fal-js)

Pick fal over Runware when the model you need is in fal's catalogue and you are happy to send one request per image. The queue docs say to use the `request_id` to drop a duplicate webhook delivery.

Submit a queued request with a webhook URL, from the fal webhooks docs.
```bash
curl --request POST \
  --url 'https://queue.fal.run/fal-ai/flux/dev?fal_webhook=https://url.to.your.app/api/fal/webhook' \
  --header "Authorization: Key $FAL_KEY" \
  --header 'Content-Type: application/json' \
  --data '{
  "prompt": "Photo of a cute dog"
}'
```

### 6. Replicate

*Best for a per-image queue over open and official models*

![Replicate screenshot](https://assets.wireflow.ai/competitors/replicate-dc84c666a83f.png?v=r5)

- **What it is:** [Replicate](https://replicate.com/) is a platform to run and fine-tune models and deploy custom models through a cloud API. Each run is a prediction, and you poll it or receive it by webhook.
- **Best for:** Developers who want to call many open and official models by API, or deploy their own model with Cog.
- **Pricing basis:** It charges per request. Some models are billed per output and others per second of hardware time. Rates are on the [Replicate pricing page](https://replicate.com/pricing), checked October 2026. A batch discount is not published.
- **Standout:** A webhook delivers a completed prediction's metadata and outputs to your URL, and every webhook is signed with a key unique to each user or organization. You can narrow the deliveries to start, output, logs or completed events. Source: [Replicate's webhook docs](https://replicate.com/docs/topics/webhooks).
- **Limit:** A batch endpoint and an idempotency key are not published, so one prediction is one model run. For predictions created through the API, inputs, outputs, files and logs are removed after an hour by default, so you must save your own copies. Predictions time out after running for 30 minutes. Source: [Replicate's data retention page](https://replicate.com/docs/topics/predictions/data-retention).
- **API or automation:** You create a prediction, then poll the API or use a webhook to check its status. Prediction creation is limited to 600 requests per minute, with short bursts allowed above that, so pace a large batch from your own worker. See [the prediction lifecycle page](https://replicate.com/docs/topics/predictions/lifecycle).
- **Note:** The linked repository is the official Python client. JavaScript, Swift and Go clients are listed as well.
- **Sources:** the webhook, data retention and lifecycle pages linked above, plus [Replicate's rate limits page](https://replicate.com/docs/topics/predictions/rate-limits) and [its client libraries page](https://replicate.com/docs/reference/client-libraries), checked 2026-10-11.
- **Links:**
  - [replicate/replicate-python](https://github.com/replicate/replicate-python)

Pick Replicate over fal when the model you need is hosted there, or when you want to push your own model. Save every output inside the hour.

Create a prediction with a completion webhook, from Replicate's create a prediction page.
```bash
curl -s -X POST \
  -H "Authorization: Bearer $REPLICATE_API_TOKEN" \
  -H 'Content-Type: application/json' \
  -d '{"version": "5c7d5dc6dd8bf75c1acaa8565735e7986bc5b66206b55cca93cb72c9bf15ccaa", "input": {"text": "Alice"}, "webhook": "https://my.server.com/webhooks/replicate", "webhook_events_filter": ["completed"]}' \
  https://api.replicate.com/v1/predictions
```

### 7. Black Forest Labs API

*Best for calling FLUX models directly*

![Black Forest Labs API screenshot](https://assets.wireflow.ai/competitors/black-forest-labs-api-2e3fbb330599.png?v=r5)

- **What it is:** The [Black Forest Labs API](https://docs.bfl.ai/quick_start/introduction) generates and edits images and video with the FLUX model family, including FLUX 3 and FLUX.2. Generation is asynchronous, so each request returns a URL that you poll for the result.
- **Best for:** Developers who want FLUX models called directly from code and do not need another model family.
- **Pricing basis:** It charges credits. The pricing page says batch requests multiply the base cost by the number of images, so no batch discount is stated. See the [Black Forest Labs pricing page](https://docs.bfl.ai/quick_start/pricing), checked October 2026.
- **Standout:** Some of the same models are also released as open weights for self-hosting, including `FLUX.2 [klein]`, `FLUX.2 [dev]` and `FLUX.1 [dev]`. The docs describe a FLUX MCP server as well.
- **Limit:** Generated images expire after 10 minutes and become inaccessible, and the delivery URLs have no CORS support, so you download each image and serve it from your own storage. A batch job endpoint, an idempotency key and an official SDK repository are not published. You must add credits before the first call. Source: [the Black Forest Labs integration guide](https://docs.bfl.ai/api_integration/integration_guidelines).
- **API or automation:** You POST a request, get back a `polling_url` and poll it for the finished result. The `FLUX.2 [pro]` endpoint accepts a `webhook_url` and an optional `webhook_secret` for signature verification. The FLUX 3 image endpoint reference lists a `polling_url` and no webhook field.
- **Note:** No official SDK repository is named in the docs index, so none is listed in the data table.
- **Sources:** the introduction, pricing page and integration guide linked above, and the `FLUX.2 [pro]` and FLUX 3 image endpoint references in the same docs, checked 2026-10-11.

Pick the Black Forest Labs API over the two queues above when FLUX is the only model family you need. Move each file to your own storage inside the 10 minutes.

Submit a request and read the polling URL, from the Black Forest Labs integration guide. The guide then polls that URL until the status is Ready.
```bash
response=$(curl -X 'POST' \
  'https://api.bfl.ai/v1/flux-2-pro-preview' \
  -H 'accept: application/json' \
  -H "x-key: ${BFL_API_KEY}" \
  -H 'Content-Type: application/json' \
  -d '{
    "prompt": "A serene landscape with mountains",
    "width": 1440,
    "height": 810
  }')

request_id=$(echo $response | jq -r .id)
polling_url=$(echo $response | jq -r .polling_url)
```

### 8. Leonardo AI

*Best for outputs that should stay hosted*

![Leonardo AI screenshot](https://assets.wireflow.ai/competitors/leonardo-ai-a0087167d41f.png?v=r5)

- **What it is:** [Leonardo AI](https://leonardo.ai/) has a production API that is separate from its web app plans. You buy API credits, create an API key in the web app and generate images with that key.
- **Best for:** Creators and developers who want generated images to stay available without moving them to their own storage.
- **Pricing basis:** It charges credits bought in dollars, on Pay-As-You-Go pricing where you top up a balance and pay for the generations you create. See [Leonardo's Pay-As-You-Go guide](https://docs.leonardo.ai/docs/payg-guide), checked October 2026. A batch discount is not published.
- **Standout:** The API FAQ says images generated through the API do not expire and can also be opened from the web app at any time. It also says you can use your own custom model with the API. Source: [the Leonardo API FAQ](https://docs.leonardo.ai/docs/api-faq).
- **Limit:** A batch endpoint and an idempotency key are not published. The webhook callback is set on the API key and not on each request, so changing it means creating a new production API key. Concurrency, a pending queue limit and an API rate limit all apply, as [Leonardo's limits guide](https://docs.leonardo.ai/docs/guide-to-concurrency-queue-and-rate-limit) explains.
- **API or automation:** You create a generation, then poll the Get a Single Generation endpoint until the status is `COMPLETE` or `FAILED`. The `num_images` parameter sets how many images one generation returns. A production API key can carry an HTTPS webhook callback URL that receives the links to the finished images, as [the webhook callback guide](https://docs.leonardo.ai/docs/guide-to-the-webhook-callback-feature) describes.
- **Note:** The linked repository is the official Python SDK.
- **Sources:** the guides linked above, plus [Leonardo's quick start guide](https://docs.leonardo.ai/docs/getting-started) and [its official SDKs page](https://docs.leonardo.ai/docs/leonardoai-official-sdks), checked 2026-10-11.
- **Links:**
  - [Leonardo-Interactive/leonardo-python-sdk](https://github.com/Leonardo-Interactive/leonardo-python-sdk)

Pick Leonardo AI over the Black Forest Labs API when you would rather not build storage for the outputs. Requests beyond your concurrency limit wait in a queue, so read the default limits before a large run.

Create a generation, from Leonardo's quick start guide. The response returns a generation ID to poll.
```bash
curl --request POST \
     --url https://cloud.leonardo.ai/api/rest/v1/generations \
     --header 'accept: application/json' \
     --header 'authorization: Bearer <YOUR_API_KEY>' \
     --header 'content-type: application/json' \
     --data '
{
  "alchemy": false,
  "height": 1080,
  "modelId": "7b592283-e8a7-4c5a-9ba6-d18c31f258b9",
  "contrast": 3.5,
  "num_images": 4,
  "styleUUID": "111dc692-d470-4eec-b791-3475abac4c46",
  "prompt": "A serene watercolor painting of a mountain lake at sunrise",
  "width": 1920,
  "ultra": false
}
'
```

## What each check means

A check means the vendor's own docs describe it. A cross means the docs say it works another way. A dash means not published in the pages we read, which is not proof that it is absent.

- **Many images in one request:** One request or one batch job starts many image generations. That can be a batch input file, an array of tasks, or one run of a saved workflow that loops over a list. Several images from one prompt do not count.
- **Batch discount stated:** The vendor says batch work costs less than the same requests sent one by one.
- **Async job and polling:** A slow job returns an ID or a URL that you poll for status and results.
- **Completion webhook:** The vendor calls your URL when a job, a task or a batch finishes.
- **Idempotency key:** The docs describe a key that stops a retried request from starting and billing a second job.
- **Result retention stated:** The vendor says how long outputs or results stay available.
- **Official SDK:** The vendor's docs name a first-party client library.

## A check to run before a real batch

No vendor page can tell you how a provider behaves with your own prompts, so run a small batch first. These steps are a recipe, and we have not run them for this list.

1. Pick 50 real prompts or product records, and write down the output you expect for each one.
2. Submit them the provider's own way: a batch file, an array of tasks, one run of a saved workflow, or one queued request per image.
3. Stop the client halfway and resubmit. Use the same idempotency key where one is documented, then count how many jobs were billed.
4. Receive the results by webhook, then turn the webhook off and confirm that polling returns the same results.
5. Compare how long the output URLs stay live with the stated retention, and copy every output to your own storage.

## How this list is maintained

Every cell comes from the vendor's own docs, pricing page or API reference, last checked on 11 October 2026. The numbered order is editorial and uses 4 checks: how much of a batch one request covers, how a job reports back, whether the docs describe a safe retry, and how long results are kept. The capability score is a separate plain count of documented checks, and it does not set the order. Nothing here was benchmarked, so no batch was timed and no price was tested.

The list is capped at 8 entries. Stability AI is left out because its developer pages are rendered by script, and we could not read its cells from the page source on the check date.

To correct a cell, open an issue or a pull request with the tool, the check and a link to the vendor page that proves it. PRs are welcome, including corrections to entry 1.

- [`data/tools.json`](data/tools.json) is the source of truth. The tables in this README are generated from it and are overwritten on every run, so edit the JSON, not the tables.
- [`scripts/update.js`](scripts/update.js) fetches star counts and latest release tags from the GitHub API for the tools that publish an official repo, stamps the check date, and regenerates the tables. `--offline` regenerates without the network; `--check` exits non-zero if the README has drifted from the data.
- [`.github/workflows/refresh.yml`](.github/workflows/refresh.yml) runs it weekly and on manual dispatch, and commits only when the data actually changed.
- Prices are deliberately not stored as numbers. A stale price in a comparison table is worse than no price, so the table links to each vendor's own pricing page.

## FAQ

<details>
<summary><strong>What is a batch image generation API?</strong></summary>

A batch image generation API accepts many image requests as one job or one request and returns the results later. On this list, the OpenAI API and the Google Gemini API take a file of requests, Runware takes an array of tasks, and Wireflow runs a saved workflow that loops over a list.

</details>

<details>
<summary><strong>What image generation API is best for batch creative production?</strong></summary>

It depends on what one finished image needs. If each image is a single prompt and can wait a day, the OpenAI API and the Google Gemini API state a 50% batch discount. If each image needs several steps, Wireflow runs the whole chain from one request, as [this text to image endpoint example](https://www.wireflow.ai/features/text-to-image-api) shows.

</details>

<details>
<summary><strong>Which image generation APIs support asynchronous processing for high volume calls?</strong></summary>

All 8 of them support it: each returns something you can poll, and each documents a webhook for finished work. What differs is the unit of work: a batch file on the OpenAI API and the Google Gemini API, an array of tasks on Runware, one run of a saved workflow on Wireflow, and one request per image on the other 4.

</details>

<details>
<summary><strong>How do I check the status of a batch?</strong></summary>

You poll the job or wait for a webhook. The OpenAI API and the Google Gemini API report one status for the whole batch, while Runware, fal and Replicate report each task or request separately. A Wireflow run reports one execution, and [the Wireflow webhooks reference](https://www.wireflow.ai/docs/api/webhooks) lists what the completion delivery contains.

</details>

<details>
<summary><strong>Can I prevent duplicate batch executions?</strong></summary>

You can only rely on it where the vendor documents it. Wireflow replays the original run when a retried request to its run endpoint carries the same `Idempotency-Key`, and the Google Gemini API docs say creating a batch job is not idempotent. For the other 6, the pages we read describe no idempotency key for submitting work, so keep your own record of what you sent.

</details>

<details>
<summary><strong>What happens if I run out of credits mid-batch?</strong></summary>

Each vendor handles it differently, so check before a large run. Wireflow's docs say an API run without enough credits returns a 402 error with the credit cost of each node, and a model that fails is not charged. Runware's pricing page says you are charged only for successful requests.

</details>

<details>
<summary><strong>Can an agent trigger a batch run?</strong></summary>

Yes, on the 5 entries that document an MCP server. Wireflow is a Claude connector, so a connected agent can run a saved workflow and read back the results, and Runware, fal, Replicate and the Black Forest Labs API each document an MCP server as well. Check each server's own docs for what it can start.

</details>

## The short version

Pick Wireflow when each finished image needs more than one step and you want one request, one callback and a documented retry rule. Pick the OpenAI API, the next entry, when the job is thousands of single prompts that can wait a day at a stated discount. If you need results sooner and still want many tasks per request, go to Runware. Before you commit, send 50 real prompts through your two finalists, stop the client halfway and count what was billed. For the first option, the next page to read is [a batch built on the canvas](https://www.wireflow.ai/features/batch-ai-generation).

## Contributing

Corrections and additions are welcome, including corrections to the entry for the tool that maintains this list. Open an issue with the tool name, a working link, one line on what it does that the tools already listed do not, and one line on where it stops. Entries are judged on whether they are usable today, not on popularity. Full rules in [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[CC0 1.0 Universal](LICENSE), public domain. Take the data, fork the list, no attribution required.

---

Maintained by [a1adams](https://github.com/a1adams).
