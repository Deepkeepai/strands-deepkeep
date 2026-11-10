# strands-deepkeep

> **Development moved:** This integration is maintained in the
> [DeepKeep integrations monorepo](https://github.com/Deepkeepai/deepkeep-integrations/tree/main/integrations/strands).
> Please open new [issues](https://github.com/Deepkeepai/deepkeep-integrations/issues) and pull requests in the monorepo.
> Historical source and existing links remain available here.
> The PyPI package name remains `strands-deepkeep`.

Strands Agents hook provider for the [DeepKeep AI Firewall](https://deepkeep.ai/capabilities/ai-firewall).

DeepKeep runs before a Strands invocation reaches the model and after the model returns. It can block unsafe content, pass safe content through, and apply redacted or modified text returned by DeepKeep.

## Installation

```bash
pip install strands-deepkeep
```

## Quick start

```python
import os

from strands import Agent
from strands_deepkeep import DeepKeepGuardrail

agent = Agent(
    plugins=[
        DeepKeepGuardrail(
            api_key=os.environ["DEEPKEEP_API_KEY"],
            base_url=os.environ["DEEPKEEP_BASE_URL"],
            pre_model="input-firewall-id",
            post_model="output-firewall-id",
        )
    ]
)

result = agent("Summarize this customer support request")
```

`pre_model` and `post_model` are DeepKeep firewall IDs. Each value is sent to the matching moderation endpoint as the `model` field.

## Behavior

- `allow` and `alert` decisions let the invocation continue.
- `block` decisions cancel the invocation before the model or clear blocked model output after the model call.
- `redact` and `modify` decisions replace the latest user text before the invocation or replace generated assistant text after the model call.
- DeepKeep API decisions are stored in `invocation_state` as `deepkeep_pre` and `deepkeep_post` for audit and logging.

## Environment

You can pass credentials directly or set:

```bash
export DEEPKEEP_API_KEY="dk-..."
export DEEPKEEP_BASE_URL="https://your-deepkeep-instance.example"
```

## Strands catalog entry

The integration was accepted into the Strands catalog in
[PR #3996](https://github.com/strands-agents/harness-sdk/pull/3996); do not submit a
duplicate entry. [PR #5852](https://github.com/strands-agents/harness-sdk/pull/5852)
tracks updating its source/documentation links. The snippet below shows the
intended monorepo destinations, not a claim that the catalog update is deployed:

```yaml
name: strands-deepkeep
description: DeepKeep AI Firewall guardrails for Strands Agents using lifecycle hooks around model input and output.
integrationType: plugin
languages:
  python:
    package: strands-deepkeep
github: https://github.com/Deepkeepai/deepkeep-integrations/tree/main/integrations/strands
docsUrl: https://github.com/Deepkeepai/deepkeep-integrations/tree/main/integrations/strands#readme
addedDate: 2026-08-26
maintainedBy: partner
```
