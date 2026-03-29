# pydantic-ai-lmstudio-provider

A Pydantic AI provider for [LM Studio](https://lmstudio.ai/) local inference.

## Installation

```bash
uv add pydantic-ai-lmstudio-provider
```

## Requirements

- Python 3.12+
- LM Studio installed and running locally

## Quick Start

1. **Start LM Studio** and load a model (e.g., via `http://localhost:1234`)

2. **Use with Pydantic AI**:

```python
from pydantic_ai import Agent
from pydantic_ai_lmstudio_provider import LMStudioProvider

provider = LMStudioProvider()

agent = Agent(
    provider=provider,
    model='lmstudio-ai/qwen2.5-7b-instruct',  # or any model loaded in LM Studio
)

result = agent.run_sync('What is the capital of France?')
print(result.data)
```

## Configuration

The `LMStudioProvider` supports several initialization patterns:

### Default (uses cached HTTP client)

```python
provider = LMStudioProvider()
```

### Custom API key

```python
provider = LMStudioProvider(api_key='your-api-key')
```

### Custom HTTP client

```python
import httpx
from pydantic_ai_lmstudio_provider import LMStudioProvider

http_client = httpx.AsyncClient(timeout=60.0)
provider = LMStudioProvider(http_client=http_client)
```

### Pass your own OpenAI client

```python
from openai import AsyncOpenAI
from pydantic_ai_lmstudio_provider import LMStudioProvider

openai_client = AsyncOpenAI(
    base_url='http://localhost:1234/v1',
    api_key='lmstudio',
)
provider = LMStudioProvider(openai_client=openai_client)
```

## LM Studio Setup

1. Download and install [LM Studio](https://lmstudio.ai/)
2. Open LM Studio and download a model (recommended: Qwen2.5, Llama 3.2, or Mistral)
3. Click "Start Server" and ensure the local server is running at `http://localhost:1234`
4. Select your model and click "Apply"

## Usage with Tools

You can use Pydantic AI agents with tools:

```python
from pydantic import BaseModel
from pydantic_ai import Agent
from pydantic_ai_lmstudio_provider import LMStudioProvider

class WeatherResult(BaseModel):
    temperature: int
    conditions: str

weather_agent = Agent(
    provider=LMStudioProvider(),
    model='lmstudio-ai/qwen2.5-7b-instruct',
    result_type=WeatherResult,
)

result = weather_agent.run_sync('What is the weather in Paris?')
print(result.data.temperature)  # e.g., 22
```

## License

MIT
