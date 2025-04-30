
# Adding Multiple Models to the Astra Workspace

## Introduction

This guide documents the process of adding support for multiple language models in the Astra workspace. This allows users to select different models when interacting with agents, providing flexibility in terms of performance, cost, and capabilities.

## Why Support Multiple Models?

Supporting multiple models offers several benefits:

1. **Performance vs. Cost Trade-offs**: Smaller models (e.g., GPT-4.1-mini) are faster and less expensive than larger models (e.g., GPT-4.1), allowing users to choose based on their specific needs.

2. **Capability Differences**: Different models excel at different tasks. Some models might be better at reasoning, while others might be better at creative tasks.

3. **Testing and Comparison**: Having multiple models allows developers to test and compare responses across models without switching between systems.

4. **Fallback Options**: If one model is unavailable or over capacity, applications can fallback to alternative models.

## Architecture Overview

The Astra system supports multiple models through a layered architecture:

1. **Model Definition**: Models are defined in an Enum in `api/routes/agents.py`
2. **Agent Implementation**: Agent files (e.g., `agents/master_astra.py`) accept a model parameter
3. **Agent Instances**: Multiple instances of the same agent with different models are created in `api/routes/playground.py`
4. **Playground Integration**: The playground displays all agent instances for user selection

## Step-by-Step Implementation Guide

### 1. Define Available Models

The first step is to define all available models in the `Model` enum located in `api/routes/agents.py`:

```python
class Model(str, Enum):
    gpt_4 = "gpt-4.1"
    gpt_4_mini = "gpt-4.1-mini"
    gpt_4_nano = "gpt-4.1-nano"
    gpt_4o_mini = "gpt-4o-mini"
    o4_mini = "o4-mini"
    o3_mini = "o3-mini"
    bedrock_claude = "anthropic.claude-instant-v1"
```

Each model has:
- A Python-friendly enum name (e.g., `gpt_4_mini`)
- The actual model identifier used by the API (e.g., `gpt-4.1-mini`)

To discover available models, you can use the OpenAI API:
```bash
curl https://api.openai.com/v1/models -H "Authorization: Bearer YOUR_API_KEY"
```

### 2. Update Agent Implementation

Modify your agent implementation to support dynamic model selection. In `agents/master_astra.py`:

```python
def get_master_astra(
    model_id: str = "gpt-4.1",
    user_id: Optional[str] = None,
    session_id: Optional[str] = None,
    debug_mode: bool = True,
) -> Agent:
    # Create unique agent_id and display name for each model
    model_suffix = model_id.replace('-', '_').replace('.', '_')
    agent_id = f"master-astra-{model_suffix}"
    display_name = f"Master Astra ({model_id})"
    
    return Agent(
        name=display_name,
        agent_id=agent_id,
        model=OpenAIChat(id=model_id),
        # ... other parameters
    )
```

Key changes:
- Accept `model_id` as a parameter
- Create a unique `agent_id` for each model variation
- Set a display name that includes the model identifier
- Pass the model identifier to the model constructor

### 3. Create Multiple Agent Instances

In `api/routes/playground.py`, create one instance of your agent for each model:

```python
# Master Astra agents with different models
master_astra_gpt4 = get_master_astra(model_id="gpt-4.1", debug_mode=True)
master_astra_gpt4_mini = get_master_astra(model_id="gpt-4.1-mini", debug_mode=True)
master_astra_gpt4_nano = get_master_astra(model_id="gpt-4.1-nano", debug_mode=True)
master_astra_gpt4o_mini = get_master_astra(model_id="gpt-4o-mini", debug_mode=True)
master_astra_o4_mini = get_master_astra(model_id="o4-mini", debug_mode=True)
```

### 4. Add All Instances to the Playground

Still in `api/routes/playground.py`, include all agent instances in the playground:

```python
# Create a playground instance
playground = Playground(
    agents=[
        sage_agent, 
        scholar_agent, 
        master_astra_gpt4,
        master_astra_gpt4_mini,
        master_astra_gpt4_nano,
        master_astra_gpt4o_mini,
        master_astra_o4_mini
    ],
    teams=[finance_researcher_team, multi_language_team],
    workflows=[blog_post_generator, investment_report_generator],
)
```

### 5. Update Default Model in API Requests

In `api/routes/agents.py`, update the default model for RunRequest:

```python
class RunRequest(BaseModel):
    """Request model for running an agent"""
    message: str
    stream: bool = True
    model: Model = Model.gpt_4  # Default model
    user_id: Optional[str] = None
    session_id: Optional[str] = None
```

## Database Considerations

Each agent with a unique agent_id will have its own distinct storage:

- When using the PostgresAgentStorage class, consider the `table_name` parameter
- For agents with a knowledge base, the PgVector table should correspond to the agent type

## Testing Multiple Models

After implementing multiple models:

1. Start the Astra server
2. Access the playground at app.agno.com
3. You should see multiple instances of your agent in the agent selection dropdown
4. Each instance will be labeled with its model name (e.g., "Master Astra (gpt-4.1-mini)")
5. Select an agent to interact with that specific model

## Troubleshooting

Common issues when adding multiple models:

1. **API Key Permissions**: Ensure your API key has access to all the models you're trying to use
2. **Naming Conflicts**: Make sure each agent has a unique agent_id
3. **Database Conflicts**: If multiple agents share table names, they might overwrite each other's data
4. **Model Availability**: Some models may not be available in all regions or accounts

## Best Practices

1. **Naming Convention**: Use a consistent naming pattern for models
2. **Default Models**: Choose reasonable default models based on the use case
3. **Model Groups**: Group similar models together in the UI
4. **Testing**: Test all models to ensure they work as expected
5. **Documentation**: Document which models are recommended for which tasks
6. **Cost Monitoring**: Be aware of cost differences between models
