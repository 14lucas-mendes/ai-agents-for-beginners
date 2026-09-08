[![How to Design Good AI Agents](./images/lesson-4-thumbnail.png)](https://youtu.be/vieRiPRx-gI?si=cEZ8ApnT6Sus9rhn)

> _(Click the image above to view the video for this lesson)_

# Tool Use Design Pattern

## Big Picture

Until now, our AI Learning Agent could understand a request and generate a response, but it could not actually inspect the course repository or perform external work.

This lesson changes that.

By the end, you should be able to explain how a model decides to request a tool, how your application executes that tool, and why tool use is the point where an AI application begins to gain real operational capability.

---

## Before This Lesson: The Agent Can Talk, but It Cannot Act

A learner asks:

> “Quais aulas deste repositório falam sobre Tool Use?”

Without access to the repository, the model has only two options:

1. answer from information already in context;
2. guess based on what it thinks the course contains.

Neither is equivalent to actually checking the files.

The limitation is simple:

> **The model can generate text, but it cannot independently access systems outside the model call.**

---

## The Problem: Useful Answers Often Require External Capabilities

Real applications need access to things the model does not own:

- current data;
- databases;
- files;
- business APIs;
- calculators;
- code execution;
- search systems;
- messaging or workflow services.

Giving the model direct unrestricted access to all of these would be dangerous.

Instead, the application exposes a controlled set of capabilities called **tools**.

---

## The New Capability: Tools

### What is a tool?

A **tool** is a capability your application makes available to the model in a structured way.

A tool can be as simple as:

```python
def get_current_time(location: str) -> str:
    ...
```

or as consequential as:

```python
def cancel_order(order_id: str) -> str:
    ...
```

The important point is that the model does **not** execute arbitrary code by itself.

Your application defines what tools exist and controls what happens when one is requested.

### Why does this matter?

Tools let the agent cross the boundary between **reasoning/generation** and **external action or retrieval**.

### How does the loop work?

At a high level:

```text
User request
    ↓
Model receives available tool descriptions
    ↓
Model chooses: answer directly OR request a tool
    ↓
Application validates and executes tool
    ↓
Tool result returns to model
    ↓
Model produces final answer or requests another tool
```

This loop is the core of tool use.

---

## Function Calling: The Contract Between Model and Code

The model needs a description of each available tool.

That description normally includes:

- tool name;
- purpose;
- input parameters;
- parameter types;
- required fields.

For example:

```json
{
  "name": "search_lessons",
  "description": "Search course lessons for a topic",
  "parameters": {
    "type": "object",
    "properties": {
      "query": {
        "type": "string",
        "description": "Topic the learner wants to study"
      }
    },
    "required": ["query"]
  }
}
```

The tool description is not decorative documentation. It influences whether the model selects the tool and what arguments it produces.

---

## Concrete Example: AI Learning Agent v1

We now give our continuous project its first real external capability:

```python
def search_lessons(query: str) -> list[str]:
    """Search lesson titles and content for the learner's topic."""
    ...
```

The learner asks:

> “Quero aprender Tool Use. Encontre as aulas certas.”

The system can now behave like this:

```text
Learner request
    ↓
Model identifies missing repository knowledge
    ↓
search_lessons("Tool Use")
    ↓
["04-tool-use", "11-agentic-protocols", ...]
    ↓
Model explains what to study first
```

This is the first major upgrade in the course project.

> **AI Learning Agent v1 can now use a tool to inspect information outside the model.**

---

## Tool Categories

Tools usually fall into two broad categories.

### Knowledge tools

They retrieve information.

Examples:

- search course files;
- query Azure AI Search;
- look up customer records;
- fetch weather;
- query a SQL database.

### Action tools

They change something in the world.

Examples:

- send an email;
- create a ticket;
- cancel a reservation;
- update a database;
- trigger a deployment.

This distinction matters because action tools usually require stronger validation and approval policies.

---

## Building Blocks of Tool Use

A robust tool system includes more than a Python function.

### 1. Tool schema

Describes the capability to the model.

### 2. Selection logic

The model decides whether a tool is needed and which one matches the request.

### 3. Execution layer

Your application runs the selected tool.

### 4. Validation

Inputs are checked before execution.

### 5. Result handling

Tool output is returned to the model in a form it can use.

### 6. Error handling

The system needs a plan for timeouts, unavailable APIs, invalid inputs, and partial results.

### 7. State and traceability

Developers should be able to determine which tool ran, with what inputs, and what happened next.

---

## Manual Function Calling vs. Framework Tool Use

It is useful to understand both levels.

### Manual loop

Using a lower-level model API, the application:

1. sends tool schemas;
2. receives a tool call;
3. parses the arguments;
4. executes the function;
5. sends the result back;
6. asks the model to continue.

The existing lesson material contains a full example using the Azure OpenAI Responses API and a `get_current_time` function.

### Framework abstraction

With Microsoft Agent Framework, a function can be registered as a tool and the framework handles much of the back-and-forth interaction.

Example:

```python
import os

from agent_framework import tool
from agent_framework.foundry import FoundryChatClient
from azure.identity import AzureCliCredential


@tool(approval_mode="never_require")
def get_current_time(location: str) -> str:
    """Get the current time for a location."""
    ...


provider = FoundryChatClient(
    project_endpoint=os.environ["AZURE_AI_PROJECT_ENDPOINT"],
    model=os.environ["AZURE_AI_MODEL_DEPLOYMENT_NAME"],
    credential=AzureCliCredential(),
)

agent = provider.as_agent(
    name="time_agent",
    instructions="Use available tools when needed.",
    tools=[get_current_time],
)

response = await agent.run("What time is it in San Francisco?")
```

Remember the Lesson 02 principle:

> The framework hides plumbing, not responsibility.

You still own tool design, permissions, validation, and business consequences.

---

## Good Tool Design

A good tool is:

- **narrow** — one clear job;
- **well named** — the purpose is obvious;
- **typed** — inputs have predictable structure;
- **described clearly** — the model can distinguish it from alternatives;
- **safe to fail** — errors do not silently cascade;
- **least privilege** — it can access only what it needs.

Compare:

```python
def database_tool(command: str):
    ...
```

with:

```python
def get_customer_order_status(order_id: str):
    ...
```

The second tool has a much smaller and safer capability surface.

---

## Hands-On: Add the First Capability

Design a tool for the AI Learning Agent:

```python
def search_lessons(query: str) -> list[str]:
    ...
```

Your task is to specify:

1. the tool name;
2. one-sentence description;
3. input type;
4. output type;
5. what data it may access;
6. what it must never modify;
7. one expected failure mode.

Then sketch the execution flow for:

> “Encontre as aulas sobre RAG.”

### Deliverable

A tool contract plus a six-step execution trace from user request to final response.

---

## Failure Mode: Giving the Agent One Powerful Generic Tool

A common shortcut is exposing a tool like:

```python
def execute(command: str):
    ...
```

That may be flexible, but it is difficult to validate and difficult to restrict.

If the agent only needs to search lessons, give it a search tool.

If it only needs read access, do not give it write access.

Capability should match the task.

---

## Failure Mode: Tool Errors Are Not Model Errors

Suppose `search_lessons()` times out.

A poor agent might invent a result and continue.

A better system can:

1. identify the tool failure;
2. retry if appropriate;
3. use an alternative tool if available;
4. tell the user that it could not verify the answer.

The model should not turn an infrastructure failure into a confident hallucination.

---

## Trust Check: Retrieval vs. Action

Before registering a tool, ask:

- Does it only read, or can it change state?
- What is the worst valid input it could receive?
- What is the worst invalid input?
- Does execution need user approval?
- What should be logged?
- Can the operation be reversed?

We will formalize these controls in Lesson 06.

---

## Guided Code Samples

Use the repository samples to inspect real implementations:

- Python: [`code_samples/04-python-agent-framework.ipynb`](./code_samples/04-python-agent-framework.ipynb)
- .NET: [`code_samples/04-dotnet-agent-framework.md`](./code_samples/04-dotnet-agent-framework.md)
- Supporting tests: [`code_samples/test_demo_plugins.py`](./code_samples/test_demo_plugins.py)

As you read them, identify:

```text
Tool definition
Tool registration
Model decision
Execution
Tool output
Final response
```

---

## Optional: Smoke Test After Deployment

Once you reach [Lesson 16](../16-deploying-scalable-agents/README.md), you can run the provided smoke tests for the deployed Lesson 04 agent. See [`tests/README.md`](../tests/README.md).

---

## Checkpoint

Explain these in your own words:

1. Why can a model not simply “use an API” unless the application exposes a capability?
2. What information belongs in a tool schema?
3. What is the difference between a knowledge tool and an action tool?
4. Why is a narrow tool usually safer than a generic executor?
5. Who actually executes the tool: the model or the application runtime?
6. What should happen if a tool fails?

---

## AI Learning Agent Progress

**Before:** The agent could reason about a learner request but could not inspect external information.

**Now:** **AI Learning Agent v1 can call a controlled tool to retrieve information from outside the model.**

Next we face a new problem:

> Finding information is not enough. How do we make sure the final answer is actually grounded in the right source material?

That is the focus of Lesson 05.

---

## One-Line Takeaway

> **Tools give an agent capabilities outside the model, but your application still controls what exists, what runs, and what is allowed.**

---

## Previous Lesson

[AI Agentic Design Principles](../03-agentic-design-patterns/README.md)

## Next Lesson

[Agentic RAG](../05-agentic-rag/README.md)
