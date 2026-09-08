[![Intro to AI Agents](./images/lesson-1-thumbnail.png)](https://youtu.be/3zgm60bXmQk?si=QA4CW2-cmul5kk3D)

> _(Click the image above to watch the video for this lesson)_

# Introduction to AI Agents and Agent Use Cases

Welcome to the **AI Agents for Beginners** course. This lesson establishes the mental model we will use throughout the rest of the course.

Come say hi in the <a href="https://discord.gg/kzRShWzttr" target="_blank">Azure AI Discord Community</a> — it's full of learners and AI builders who are happy to answer questions.

---

## Big Picture

By the end of this lesson, you should be able to explain what makes an AI agent different from a regular LLM application, recognize when an agent is useful, and sketch the basic parts of an agentic system.

More importantly, you should be able to look at a proposed AI solution and answer:

> **Does this system really need to be an agent?**

---

## Before This Lesson: What Can a Regular LLM App Do?

Imagine we are building the continuous project for this course: an **AI Learning Agent**.

A learner asks:

> “Quero aprender Tool Use. O que devo estudar primeiro?”

A normal LLM application can generate a useful answer if the right information is already in its prompt or in the model's existing knowledge.

That is valuable — but notice the limitation.

The model has not actually inspected this repository. It has not searched the lesson folders, opened a README, checked which examples exist, or decided to use an external capability.

It is still primarily **generating a response from the information it can currently see**.

That is our starting point.

---

## The Problem

Many useful tasks require more than generating text.

Consider a slightly stronger request:

> “Find the lessons in this repository that teach Tool Use, tell me which one I should read first, and show me a practical exercise from the course.”

To answer reliably, a system may need to:

1. inspect available lesson files;
2. search for relevant content;
3. choose which information matters;
4. possibly call one or more tools;
5. decide what to do next based on the results;
6. return a grounded answer to the learner.

A plain text-generation flow does not automatically provide those capabilities.

This is where **agentic systems** become useful.

---

## The New Idea: What Is an AI Agent?

### What

An AI agent is a system that uses a model to help decide **what action to take next** in pursuit of a goal.

The model is important, but the agent is the larger system around it.

A useful agent often combines:

- a **model** to interpret the request and make decisions;
- **context** describing the current task and state;
- **knowledge** or retrieved information;
- **tools** that let the system read data or take actions;
- **control logic** that decides what is allowed and what happens next;
- sometimes **memory**, planning, evaluation, or multiple agents.

### Why

Without these surrounding capabilities, an LLM can only respond from the information available to it in that moment.

An agentic system can go further: it can inspect external information, use tools, react to results, and continue working toward a goal.

### How

At a high level, an agentic loop looks like this:

```text
User goal
   ↓
Model interprets the request
   ↓
System decides whether an action is needed
   ↓
Tool / knowledge / environment interaction
   ↓
New result enters the context
   ↓
Model decides what to do next
   ↓
Final response or another action
```

The important idea is not “the model thinks forever.”

The important idea is that the system can **observe, decide, act, and react to results**.

---

## Concrete Example: AI Learning Agent v0

Our first version of the project is intentionally simple.

### Learner request

> “Quero aprender Tool Use.”

### What the system currently has

- one model;
- the user's current request;
- basic course instructions;
- no repository-search tool yet;
- no RAG yet;
- no persistent memory yet.

### Before

```text
User → LLM → Answer
```

The system can explain Tool Use, but it cannot verify the lesson structure in the repository by itself.

### Where we are going

Later in the course, the same request will become:

```text
User
  ↓
Learning Agent
  ↓
Search course files
  ↓
Read relevant lesson content
  ↓
Create a study path
  ↓
Return a grounded answer with sources and practice
```

That progression is the central story of this course.

---

## A Useful Mental Model: Environment, Sensors, and Actions

Agents existed before LLMs. One classic way to reason about them is to think in terms of three parts:

- **Environment** — the space the agent operates in;
- **Sensors** — how the agent reads the current state;
- **Actuators / actions** — how the agent changes something in that environment.

For a travel-booking agent:

- the environment might be booking systems;
- a sensor might read flight availability;
- an action might reserve a seat.

For our AI Learning Agent:

- the environment is this course repository;
- a sensor-like capability could read lesson files;
- an action could search lessons or produce a personalized practice task.

![What Are AI Agents?](./images/what-are-ai-agents.png)

This mental model helps separate the **reasoning component** from the **world the system can observe and affect**.

---

## Core Building Blocks

| Part | Plain-language meaning | In our course project |
|---|---|---|
| Model | Interprets the request and helps decide what to do | Understands that the learner wants Tool Use material |
| Context | Information visible to the model right now | The learner's current question and recent tool results |
| Knowledge | External information used to ground the answer | Lesson READMEs and code samples |
| Tools | Narrow capabilities the agent can call | Search lessons, read a file, fetch a resource |
| Control | Rules around what can happen next | Decide which tools are allowed and when approval is needed |
| Memory | Information kept for later interactions | “This learner prefers Python examples” |
| Evaluation | Checks whether behavior is useful and correct enough | Test whether recommended lessons are actually relevant |
| Observability | Lets us inspect what happened | Tool calls, latency, failures, retrieved sources |

You do not need all of these in every agent.

A major skill in agent engineering is knowing **which capabilities are necessary and which only add complexity**.

---

## Different Types of Agents

Agent categories are useful as historical and conceptual models, but real systems often combine ideas from several categories.

| Agent Type | What It Does | Travel Agent Example |
|---|---|---|
| **Simple Reflex Agent** | Follows predefined rules with little or no internal state. | Complaint email → forward to customer service. |
| **Model-Based Reflex Agent** | Maintains an internal representation of changing conditions. | Tracks changing flight-price state before deciding what to flag. |
| **Goal-Based Agent** | Chooses actions based on whether they move toward a goal. | Builds a complete itinerary to reach a destination. |
| **Utility-Based Agent** | Compares alternatives using preferences or scoring. | Balances price, duration, and convenience. |
| **Learning Agent** | Improves behavior using feedback or observed outcomes. | Adjusts recommendations based on post-trip feedback. |
| **Hierarchical Agent** | Breaks work into levels or delegated subtasks. | Splits “cancel my trip” into flight, hotel, and car tasks. |
| **Multi-Agent System** | Uses multiple independent agents that cooperate or compete. | Separate hotel, flight, and entertainment agents coordinate. |

Do not treat this table as a checklist of architectures you must implement.

Later lessons will focus more on practical patterns and trade-offs than on labels.

---

## When Should You Use an AI Agent?

Just because a task can be made agentic does not mean it should be.

![When to use AI Agents?](./images/when-to-use-ai-agents.png)

Agents are especially useful when the system must deal with one or more of these conditions:

### 1. The path is not fully known in advance

If every step is deterministic and known ahead of time, a normal workflow or function may be simpler and safer.

Agents become more useful when the system must choose among possible next steps dynamically.

### 2. The task requires multiple interactions with tools or data

A single database lookup does not automatically justify an agent.

But a task that may require searching, comparing, retrieving, deciding, and then taking another action can benefit from agentic control.

### 3. The system must adapt to intermediate results

For example:

```text
Search flights
   ↓
No acceptable result
   ↓
Try a nearby airport
   ↓
Re-evaluate options
```

The result of one step changes the next decision.

### 4. Natural-language goals need to become concrete actions

A user might say:

> “Help me plan the cheapest reasonable trip next month.”

The agent must turn an open-ended goal into smaller decisions and tool calls.

---

## When Should You *Not* Use an Agent?

Use a simpler solution when:

- the workflow is deterministic and easy to encode directly;
- one model call is enough;
- a normal search/query API can answer the question reliably;
- the cost of unpredictable behavior is higher than the benefit of flexibility;
- the system cannot safely grant the model the necessary actions.

A useful engineering rule is:

> **Start with the simplest architecture that can solve the problem. Add agentic behavior only when the problem requires it.**

---

## Agent Development

The first design question is not “Which framework should I use?”

It is:

> **What should this system be allowed to observe, decide, and do?**

Only after answering that do tools, frameworks, and APIs become useful implementation choices.

In this course, the main implementation path uses **Microsoft Foundry Agent Service** and the **Microsoft Agent Framework (MAF)**.

The platform supports agent applications that can connect models, tools, external data, and production observability.

We will introduce those APIs progressively, after the architectural idea they implement is already clear.

---

## Agentic Patterns

As systems become more complex, developers need reusable ways to structure behavior.

Examples include:

- tool-use patterns;
- retrieval/RAG patterns;
- planning patterns;
- multi-agent coordination;
- context and memory patterns;
- approval and safety patterns.

These patterns are not magic prompts. They are recurring ways to organize model decisions, data, actions, and control.

The rest of the course is organized around these capabilities.

---

## Agentic Frameworks

Frameworks help developers avoid rebuilding common infrastructure repeatedly.

A framework may help with:

- registering models and tools;
- managing state;
- orchestrating steps;
- tracing and debugging;
- connecting multiple agents;
- hosting or deploying agent workflows.

But a framework does not remove architectural decisions.

You still need to decide:

- which tools the agent should have;
- what data it can access;
- how much context it needs;
- when to require human approval;
- how to evaluate whether the agent behaved correctly.

Lesson 02 explores this in more depth.

---

# Guided Code Walkthrough

Ready to see a basic agent implementation?

- 🐍 Python: [Agent Framework](./code_samples/01-python-agent-framework.ipynb)
- 🔷 .NET: [Agent Framework](./code_samples/01-dotnet-agent-framework.md)

While reading the sample, focus on these questions rather than memorizing API names:

1. Where is the model configured?
2. What instructions or context does the agent receive?
3. What capabilities are available to it?
4. What causes the system to produce the final response?
5. Which parts belong to the model, and which belong to the surrounding application?

---

# Hands-on

## Exercise: Is This Really an Agent?

Choose one of these systems:

- customer-support assistant;
- travel planner;
- repository learning assistant;
- invoice-processing workflow.

Write a short architecture with these four headings:

```text
Goal:
Context:
Tools / actions:
Control rules:
```

Then answer:

> Could this be implemented more simply without agentic behavior?

If yes, describe the simpler version.

## Expected behavior

A strong answer should make the boundary between **model capability** and **system capability** explicit.

For example, “search the repository” is not something the model automatically owns — the surrounding system must provide that capability.

---

# Make It Fail

Take your proposed agent and remove one important component.

Examples:

- remove the search tool;
- remove approval for a sensitive action;
- remove relevant context;
- give the system a vague goal;
- give it too many unnecessary tools.

Ask:

1. What becomes unreliable?
2. Does the model fail, or does the surrounding system design fail?
3. How would you detect the problem?
4. What is the smallest useful fix?

This distinction — model problem vs. system-design problem — will matter throughout the course.

---

# Trade-off

Agents gain flexibility by allowing runtime decisions, but that flexibility creates new costs:

- more latency;
- more model/tool calls;
- more possible failure paths;
- harder testing;
- more security and permission concerns.

Agentic design is therefore an engineering trade-off, not an automatic upgrade over conventional software.

---

# Trust Check

Before giving any model-powered system an action, ask:

- What data can this action access?
- What can it change?
- Can the action be reversed?
- Should the user approve it first?
- What should be logged?
- What happens when the action fails?

You will revisit these questions in later lessons as the AI Learning Agent gains more autonomy.

---

# Checkpoint

Try answering these without copying text from the lesson:

1. What is the difference between an LLM and an AI agent system?
2. Name four parts of an agentic system besides the model itself.
3. Give one example of a task where a deterministic workflow is better than an agent.
4. What does a tool add to an agent?
5. Why can adding more autonomy make testing and security harder?

### Architecture checkpoint

For the AI Learning Agent v0, identify:

- **Model:** what interprets the learner's request?
- **Context:** what information is visible right now?
- **Tools:** what external actions are available at this stage?
- **Control:** what decides what the system is allowed to do?

---

# Agent Upgrade

> **Before:** We had a generic LLM application that could answer a learner's question.
>
> **Now:** We have a clear architecture for **AI Learning Agent v0** and can distinguish the model from the larger agentic system around it.

### What can our agent do now that it could not do before?

At this point, its runtime capabilities have not expanded much yet — and that is intentional.

The upgrade is architectural: **we now know exactly what capabilities are missing and where they belong in the system.**

In Lesson 04, we will give the agent its first real external capability through Tool Use.

---

# One-line Takeaway

> **An AI agent is not just an LLM with a clever prompt; it is a system that combines a model with context, tools, knowledge, and control so it can pursue a goal through actions and feedback.**

---

## Smoke-Testing This Agent (Optional)

Once you learn to deploy agents in [Lesson 16](../16-deploying-scalable-agents/README.md), you can add a fast post-deploy health check for this lesson's `TravelAgent` with the ready-made catalog [`tests/lesson-01-smoke-tests.json`](../tests/lesson-01-smoke-tests.json). See [`tests/README.md`](../tests/README.md) for how to run it.

---

## Got Questions?

Join the [Microsoft Foundry Discord](https://discord.com/invite/ATgtXmAS5D) to connect with other learners, attend office hours, and get your AI Agent questions answered by the community.

---

## Previous Lesson

[Course Setup](../00-course-setup/README.md)

## Next Lesson

[Exploring Agentic Frameworks](../02-explore-agentic-frameworks/README.md)
