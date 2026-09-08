[![Exploring AI Agent Frameworks](./images/lesson-2-thumbnail.png)](https://youtu.be/ODwF-EZo_O8?si=1xoy_B9RNQfrYdF7)

> _(Click the image above to view the video for this lesson)_

# Explore AI Agent Frameworks

## Big Picture

In Lesson 01, we learned to recognize an agent as a **system**, not just a model prompt. That immediately creates a practical engineering question:

> If an agent needs models, instructions, tools, state, retries, approvals, and observability, do we really want to wire all of that ourselves every time?

By the end of this lesson, you should be able to explain **why agent frameworks exist**, identify which responsibilities they can take over, and distinguish a development framework from the managed infrastructure where an agent may run.

---

## Before This Lesson: We Know the Parts, but We Have No Structure

Our continuous project is the **AI Learning Agent**.

A learner asks:

> “Quero aprender Tool Use. O que devo estudar primeiro?”

At the end of Lesson 01, we can sketch the system:

```text
User request
    ↓
Model
    ↓
Possible actions / tools
    ↓
Result
    ↓
Final response
```

But this diagram hides a lot of software work.

Someone still has to:

- configure the model connection;
- send instructions and conversation messages;
- describe tools in a format the model understands;
- execute the selected tool safely;
- return tool results to the model;
- maintain state across multiple steps;
- record enough information to debug failures;
- handle authentication and service integration.

You *can* build those pieces directly. In fact, understanding the underlying loop is valuable.

The problem is repetition.

---

## The Problem: Agent Applications Repeat the Same Plumbing

Imagine building three agents:

1. a course helper;
2. a travel assistant;
3. a support agent.

Their business logic is different, but many infrastructure needs are the same.

Without a reusable abstraction, every team may invent a different way to represent tools, messages, state, retries, and agent execution. That increases code, inconsistency, and debugging effort.

This is the problem agent frameworks try to solve.

---

## The New Capability: Reusable Agent Building Blocks

### What is an AI agent framework?

An **agent framework** is a software library or SDK that packages common agent-building patterns into reusable components.

Instead of implementing every model/tool interaction from scratch, you work with concepts such as:

- agents;
- instructions;
- tools;
- conversations or state;
- workflows;
- tracing and observability hooks.

The framework does not invent your product architecture for you. It reduces the amount of repeated plumbing needed to implement that architecture.

### Why does that matter?

Frameworks let the developer spend more time on the questions that are specific to the application:

- What is the user's goal?
- Which tools should the agent have?
- What data should it see?
- What actions need approval?
- How will we know whether it worked?

### How does it work at a high level?

A framework normally sits between your application logic and the lower-level model/service APIs:

```text
Your application
      ↓
Agent framework
      ↓
Model + tools + state + integrations
```

The framework coordinates these pieces through a consistent programming model.

---

## Framework vs. Managed Agent Service

These two ideas are easy to mix up.

A **framework** helps you *write and organize agent code*.

A **managed agent service/platform** provides infrastructure that helps you *run, connect, manage, or operate agents* without building every infrastructure component yourself.

In this repository, the main development path uses the **Microsoft Agent Framework (MAF)** together with **Microsoft Foundry** services and model deployments.

A useful mental model is:

| Question | Framework | Managed platform/service |
|---|---|---|
| How do I represent an agent in code? | Yes | Sometimes exposed through SDK/API |
| How do I register tools? | Yes | Often integrated |
| How do I organize workflows? | Yes | May support hosted execution |
| Who owns the application logic? | You | You |
| Who operates model/service infrastructure? | Depends on provider | Usually the platform |

Do not choose a framework just because it has many features. Choose it because its abstractions fit the system you are actually building.

---

## Concrete Example: AI Learning Agent

Suppose our first version needs only one capability later in the course:

```python
search_lessons(query: str) -> list[str]
```

Without a framework, we would need to manually:

1. define a tool schema;
2. send it to the model;
3. detect whether the model requested the tool;
4. parse the arguments;
5. execute the Python function;
6. pass the output back to the model;
7. ask for a final response.

With a framework, much of that interaction can be represented using higher-level agent and tool abstractions.

That does **not** mean the underlying loop disappears. It means the framework manages more of it for us.

---

## A Minimal Microsoft Agent Framework Example

The samples in this course use `FoundryChatClient` to create and run agents.

A simplified example looks like this:

```python
import os

from agent_framework import tool
from agent_framework.foundry import FoundryChatClient
from azure.identity import AzureCliCredential


@tool(approval_mode="never_require")
def find_lesson(topic: str) -> str:
    """Find a course lesson for a topic."""
    return f"Suggested lesson for {topic}: 04-tool-use"


provider = FoundryChatClient(
    project_endpoint=os.environ["AZURE_AI_PROJECT_ENDPOINT"],
    model=os.environ["AZURE_AI_MODEL_DEPLOYMENT_NAME"],
    credential=AzureCliCredential(),
)

agent = provider.as_agent(
    name="learning_agent",
    instructions="Help learners navigate the AI Agents course.",
    tools=[find_lesson],
)
```

Look at the example conceptually before focusing on syntax.

The important mapping is:

```text
FoundryChatClient → model/service connection
agent             → instructions + capabilities
find_lesson       → external capability
agent.run(...)    → execution loop
```

That mapping is more important than memorizing class names.

---

## What Frameworks Commonly Help With

### 1. Model connections

A consistent interface for sending requests to the model provider used by your application.

### 2. Tool registration

A way to describe functions and make them available to an agent.

### 3. Conversation and state handling

Support for carrying relevant interaction state across calls.

### 4. Workflow composition

Ways to connect multiple steps or multiple agents when the task truly needs them.

### 5. Observability

Hooks or integrations that help you inspect model calls, tool calls, errors, latency, and execution paths.

These capabilities are useful, but abstraction has a cost: if you do not understand what the framework is hiding, debugging becomes harder.

---

## When a Framework Helps — and When It Does Not

A framework is especially useful when your application has several of these needs:

- multiple tools;
- multi-step execution;
- persistent or managed state;
- reusable agent configurations;
- workflows or agent coordination;
- production observability;
- integrations with managed AI infrastructure.

A framework may be unnecessary when your application is simply:

```text
user input → one model call → text output
```

The simplest architecture that reliably solves the problem is usually the better starting point.

---

## Guided Code Walkthrough

Use the existing samples to connect the concepts above to real code:

- Python / Microsoft Foundry: [`code_samples/02-python-agent-framework.ipynb`](./code_samples/02-python-agent-framework.ipynb)
- Python / Azure OpenAI path: [`code_samples/02-python-agent-framework-azure-openai.ipynb`](./code_samples/02-python-agent-framework-azure-openai.ipynb)
- .NET walkthrough: [`code_samples/02-dotnet-agent-framework.md`](./code_samples/02-dotnet-agent-framework.md)
- Additional Foundry agent creation guide: [`azure-ai-foundry-agent-creation.md`](./azure-ai-foundry-agent-creation.md)

As you read the sample, label each section as one of these responsibilities:

**model connection → agent configuration → tool/capability → execution → result**

---

## Hands-On: Framework Responsibility Map

Choose an agent idea — preferably the AI Learning Agent — and create a table like this:

| Responsibility | Application-specific or reusable plumbing? | Framework can help? |
|---|---|---|
| Understand learner goal | Application-specific | Partly |
| Connect to model | Reusable plumbing | Yes |
| Search course files | Application-specific tool | Registration/execution |
| Track tool calls | Reusable plumbing | Often |
| Decide what may be remembered | Application policy | No framework can decide this for you |

Then answer:

> Which responsibilities should the framework manage, and which decisions must remain explicit in our application design?

### Deliverable

A responsibility map with at least **five** rows and one sentence explaining why you would or would not use a framework for this project.

---

## Failure Mode: Framework-First Design

A common mistake is starting with:

> “I want to use framework X. What can I build with it?”

That reverses the design process.

A better sequence is:

```text
User problem
    ↓
Required capabilities
    ↓
System boundaries
    ↓
Choose abstractions/frameworks
```

If the framework becomes the architecture, teams often add unnecessary agents, memory systems, or orchestration because the library makes them easy to create.

Easy to implement is not the same as necessary.

---

## Trade-Off to Remember

Frameworks trade **control and transparency** for **speed and reusable abstractions**.

More abstraction can mean:

- less code;
- faster prototyping;
- more consistent patterns;

but also:

- another dependency to understand;
- framework-specific behavior;
- harder debugging if you do not know the underlying model/tool loop.

This is why we learn the concepts before depending on the abstraction.

---

## Checkpoint

Before continuing, explain these in your own words:

1. What repeated engineering problem does an agent framework solve?
2. What is the difference between a framework and a managed agent platform/service?
3. Which parts of an agent application remain your responsibility even when a framework handles the plumbing?
4. When would a direct model API be simpler than an agent framework?
5. In the sample, what do the model client, agent instructions, tools, and execution call each represent?

If you can answer those questions without naming a specific SDK class, you understand the important part of this lesson.

---

## AI Learning Agent Progress

**Before:** We knew the components of an agent system.

**Now:** We know how a framework can organize those components without replacing our architectural decisions.

Our agent has not gained a new end-user capability yet. Instead, **we have gained a reusable engineering structure for adding capabilities safely and consistently.**

That distinction matters.

---

## One-Line Takeaway

> **An agent framework is reusable plumbing for agent systems — useful after you understand the problem and architecture, not before.**

---

## Got Questions?

Join the [Microsoft Foundry Discord](https://discord.com/invite/ATgtXmAS5D) to connect with other learners and builders.

## Previous Lesson

[Introduction to AI Agents](../01-intro-to-ai-agents/README.md)

## Next Lesson

[AI Agentic Design Principles](../03-agentic-design-patterns/README.md)
