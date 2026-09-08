[![How to Design Good AI Agents](./images/lesson-3-thumbnail.png)](https://youtu.be/m9lM8qqoOEA?si=4KimounNKvArQQ0K)

> _(Click the image above to view the video for this lesson)_

# AI Agentic Design Principles

## Big Picture

A working agent is not automatically a good agent.

Once a system can make decisions, use tools, remember information, and act over time, we need to decide **how it should behave around people**.

By the end of this lesson, you should be able to design an agent experience around three questions:

1. What should the agent do for the user?
2. What should remain visible and controllable?
3. How should the relationship evolve over time without surprising the user?

---

## Before This Lesson: Architecture Is Not Experience

Our **AI Learning Agent** has an emerging technical structure.

We know it may eventually use tools, knowledge, context, memory, and workflows.

But imagine this interaction:

> Learner: “Help me learn Tool Use.”
>
> Agent: “I created a learning plan, saved your preferences, searched several resources, and changed your schedule.”

Even if every action was technically successful, the experience is poor if the learner never understood or approved those actions.

The architecture tells us **what the system can do**.

Design principles help us decide **how those capabilities should be experienced by a human**.

---

## The Problem: More Agency Creates More Ways to Surprise the User

A deterministic application normally follows a path the developer explicitly designed.

Agentic systems introduce uncertainty:

- the model may choose different actions for similar requests;
- context changes over time;
- tools may have side effects;
- an agent may act proactively;
- stored information can affect later behavior.

That means we cannot design only the happy-path output.

We need to design the relationship between the user and the agent.

---

## The New Capability: Human-Centered Agent Design

The design principles in this lesson are not a fixed software architecture. They are a set of lenses for deciding how an agent should behave in a way that supports people rather than simply maximizing automation.

A useful starting principle is:

> **An agent should expand human capability while keeping important actions understandable and controllable.**

The original design principles can be grouped into three lenses: **space, time, and core trust**.

![Agentic Design Principles](./images/agentic-design-principles.png)

---

## 1. Agent in Space: How the Agent Fits Into the User's Environment

### Connect, do not collapse

Agents should help connect people to information, actions, and other people. They should not unnecessarily replace human relationships or hide collaboration behind a single opaque interface.

For the AI Learning Agent, that might mean:

- recommending the relevant lesson rather than pretending to be the only source of truth;
- linking to the README or notebook it used;
- helping the learner reach community or instructor support when needed.

### Accessible, but not constantly intrusive

An agent can operate in the foreground or background, but its presence and important actions should remain discoverable.

A proactive nudge can be useful:

> “You finished Tool Use yesterday. Want a five-minute review exercise?”

Sending repeated unsolicited messages because the system *can* do so would not be useful.

---

## 2. Agent in Time: Past, Present, and Future

Agent behavior is not limited to the current prompt.

### Past

Historical context can improve relevance.

For example:

> “You struggled with function schemas in the previous exercise, so this example starts there.”

But historical data should not silently become permanent memory. Later lessons will separate **context** from **memory** more precisely.

### Present

The agent should respond to the user's current need rather than dumping everything it knows.

A learner asking for a quick reminder may need three sentences, not an entire tutorial.

### Future

Agent experiences may adapt as the user learns, changes preferences, or moves between devices and interfaces.

Adaptation should remain predictable enough that the learner can understand why the experience changed.

---

## 3. Agent Core: Embrace Uncertainty, Establish Trust

LLM-based systems are probabilistic. We should not design as though uncertainty can be removed completely.

Instead, trustworthy design makes uncertainty manageable.

Important behaviors include:

- expose important limitations;
- make consequential actions visible;
- let the user correct the system;
- provide ways to stop, change, or reverse behavior when possible;
- avoid pretending that model confidence is certainty.

The user should know when an AI system is involved and maintain meaningful control over important decisions.

---

## Three Practical Guidelines

### Transparency

The user should be able to understand what the system is doing at the level needed to make an informed decision.

For our project:

- show which course files were used;
- identify when a tool was needed;
- explain when the agent is uncertain;
- make restrictions discoverable.

Transparency does not mean exposing private chain-of-thought. It means exposing useful evidence, actions, state, and system behavior.

### Control

Users should be able to influence the system where their preferences or risk are involved.

Examples:

- choose the depth of explanations;
- approve a high-impact tool call;
- correct a remembered preference;
- delete or avoid stored information where memory is supported.

### Consistency

Similar actions should behave similarly across the product.

If one destructive action requires confirmation and another equally destructive action happens automatically, the interface teaches the user the wrong mental model.

Consistency lowers cognitive load and helps people predict the agent's behavior.

---

## Concrete Example: Designing the AI Learning Agent Before Adding Tools

A learner says:

> “Find what I should study about tool use and create a practice task.”

Before writing code, design the experience.

### Step 1: Define the learner's goal

The learner wants a small, useful path — not every resource in the repository.

### Step 2: Define visible agent behavior

The agent may:

1. identify relevant lessons;
2. tell the learner what it found;
3. recommend a short order;
4. create one exercise;
5. show the sources it relied on.

### Step 3: Define boundaries

The agent should not:

- modify repository files merely because the learner asked for study help;
- save personal information by default;
- claim a lesson says something it did not verify;
- take unrelated external actions.

### Step 4: Define user control

If later versions can save progress, the learner should know that this is happening and have a way to change that state.

This design is useful *before* we implement `search_lessons()` in the next lesson.

---

## Travel Agent Example

The same principles apply to the original travel scenario.

### Transparency

Tell the user that the agent is AI-enabled, show the important booking details, and make restrictions visible.

### Control

Let the user confirm destinations, dates, preferences, and especially purchases or cancellations before irreversible actions occur.

### Consistency

Use predictable interaction patterns for booking, modifying, and cancelling travel. The user should not have to guess which actions are automatic.

---

## Hands-On: Design Before Code

Design the next version of the AI Learning Agent without writing implementation code.

For the request:

> “Quero aprender RAG. Encontre o conteúdo certo e crie um exercício para mim.”

Write the following:

1. **Goal** — what outcome does the learner want?
2. **Agent actions** — what may the system do?
3. **Visible evidence** — what should it show about those actions?
4. **User controls** — where can the learner correct, approve, or stop behavior?
5. **Boundaries** — what should the agent explicitly not do?
6. **Uncertainty behavior** — what should happen if the agent cannot find enough evidence?

### Deliverable

A one-page design sketch containing all six elements.

---

## Failure Mode: Designing for Maximum Autonomy

A tempting assumption is:

> “The best agent is the one that asks the user for the least input.”

That is not always true.

Removing every confirmation may reduce friction, but it can also remove meaningful control.

The right amount of autonomy depends on the consequence of the action.

Compare:

- searching course files;
- recommending a lesson;
- saving a preference;
- sending an email;
- buying a flight;
- deleting data.

These should not all share the same approval policy.

---

## Trade-Off: Convenience vs. Control

More autonomous behavior can make an experience faster.

More user control can make it safer and more predictable.

Good agent design chooses that boundary deliberately rather than using a universal rule.

A useful question is:

> **If the agent makes the wrong choice here, what happens next?**

The more serious the consequence, the stronger the case for explicit validation, approval, or reversibility.

---

## Trust Check

Before adding a capability to an agent, ask:

- Does the user know the capability exists?
- Can the user understand when it is being used?
- Is the amount of autonomy proportional to the risk?
- Can the action be corrected or reversed?
- What evidence will help the user or developer understand a failure?

We will turn several of these ideas into concrete guardrails in Lesson 06.

---

## Guided Code Samples

The lesson includes implementation samples that you can inspect after you have designed the behavior:

- Python: [`code_samples/03-python-agent-framework.ipynb`](./code_samples/03-python-agent-framework.ipynb)
- .NET: [`code_samples/03-dotnet-agent-framework.md`](./code_samples/03-dotnet-agent-framework.md)

As you read the sample, do not only ask “What does this API do?”

Also ask:

> “What user-experience decision does this code implement?”

---

## Checkpoint

Explain these in your own words:

1. Why is technical correctness not enough for an agent experience?
2. What does transparency mean without exposing private model reasoning?
3. Give one example where more autonomy improves the experience.
4. Give one example where more autonomy creates unacceptable risk.
5. What should be designed before choosing tools or framework APIs?

---

## AI Learning Agent Progress

**Before:** We had a technical structure for building the agent.

**Now:** We have defined how the learner should experience that system — including visibility, control, boundaries, and uncertainty.

We still have not given the agent an external tool.

That is deliberate.

**We design the behavior before increasing the capability.**

---

## One-Line Takeaway

> **Good agent design is not maximum autonomy; it is useful autonomy with understandable boundaries and meaningful human control.**

---

## Additional Resources

- <a href="https://openai.com" target="_blank">Practices for Governing Agentic AI Systems | OpenAI</a>
- <a href="https://microsoft.com" target="_blank">The HAX Toolkit Project - Microsoft Research</a>
- <a href="https://responsibleaitoolbox.ai" target="_blank">Responsible AI Toolbox</a>

## Got More Questions about AI Agentic Design Patterns?

Join the [Microsoft Foundry Discord](https://discord.com/invite/ATgtXmAS5D) to meet with other learners, attend office hours, and get your AI Agents questions answered.

## Previous Lesson

[Exploring Agentic Frameworks](../02-explore-agentic-frameworks/README.md)

## Next Lesson

[Tool Use Design Pattern](../04-tool-use/README.md)
