# Lesson Template — AI Tutor Method

Use this template when creating or revising lesson READMEs for this course.

The goal is not to force every lesson into identical prose. The goal is to preserve a consistent learning experience: **problem first, concept second, implementation third, practice throughout**.

---

# [Lesson XX] — [Lesson Title]

## Big Picture

Write one short paragraph answering:

> What will the learner be able to understand or do by the end of this lesson?

Keep this outcome concrete and observable.

**Good:** “By the end of this lesson, you will be able to give an agent a tool and explain when the model should call it.”

**Avoid:** “In this lesson, we will explore advanced tool-use paradigms.”

---

## Before This Lesson

Briefly state what the AI Learning Agent can already do.

Example:

> Our AI Learning Agent can answer questions, but it can only use information already available to the model or included in the prompt.

This establishes the **status quo**.

---

## The Problem

Show the limitation using one concrete learner request.

Example:

> A learner asks: “Which lessons in this repository explain tool use?”
>
> A plain conversational model may guess. It cannot inspect the repository unless we give it a way to do so.

The learner should understand the problem before seeing the solution name or API.

---

## The New Capability

Introduce the concept in plain language.

Use this pattern:

1. **What** — What is the concept?
2. **Why** — Why does it matter here?
3. **How** — How does it work at a high level?

### What

Explain the idea without relying on framework-specific vocabulary.

### Why

Connect the concept directly to the problem above.

### How

Describe the minimum mechanism the learner needs before implementation.

---

## Concrete Example: AI Learning Agent

Use the continuous project as the main example.

Include:

- the learner request;
- what the agent sees;
- what decision it needs to make;
- what external capability or information it uses;
- what changes in the final answer.

Whenever possible, show a small before/after comparison.

### Before

```text
[What the agent could do before this lesson]
```

### After

```text
[What the agent can do after this lesson]
```

---

## Key Terms

Only introduce terms after the plain-language explanation.

| Term | Plain-language meaning |
|---|---|
| [term] | [direct explanation] |

Do not define a term using two other undefined terms.

---

## How It Works

Now add technical depth progressively.

Recommended order:

1. conceptual flow;
2. components involved;
3. data passed between components;
4. framework abstraction;
5. API/class/function names.

If a diagram helps, include it here.

Example flow:

```text
User request
    ↓
Model decides it needs external information
    ↓
Tool call
    ↓
Tool result
    ↓
Model uses result in final response
```

---

## Guided Code Walkthrough

Link to the relevant notebook or code sample.

Before each important code block, explain:

- what problem this block solves;
- what inputs it receives;
- what output or behavior to expect.

Do not present framework syntax without explaining its role in the system.

### Code focus

Call out only the lines that matter for the lesson objective.

Avoid turning the lesson into a line-by-line transcription of the notebook.

---

# Hands-on

The learner must change something, not only run existing code.

## Exercise

State one concrete modification.

Example:

> Add a `search_lessons(topic)` tool that returns at most three relevant lesson paths.

## Expected behavior

Describe observable success.

Example:

> Asking about “memory” should return Lesson 13 among the results.

## Constraints

Add useful boundaries if needed, such as:

- maximum result count;
- allowed data source;
- actions requiring confirmation;
- required citations;
- latency/cost expectation.

---

# Make It Fail

Every lesson should contain at least one intentional failure or weak design.

Examples:

- give a tool an ambiguous description;
- retrieve irrelevant documents;
- overflow context with unnecessary data;
- save information that should not become memory;
- let an agent perform a sensitive action without approval;
- split a simple task into too many agents.

Ask:

1. What failed?
2. Why did it fail?
3. How would you detect this in production?
4. What is the smallest useful fix?

The point is to learn system behavior, not merely obtain a successful demo.

---

# Trade-offs

Every architecture introduces costs.

Include at least one meaningful trade-off for the lesson.

Example:

> Adding retrieval can improve grounding, but it also introduces retrieval latency and the possibility of supplying irrelevant context.

Avoid presenting the technique as universally beneficial.

---

# Trust Check

Ask what new risk appears because of the capability introduced in this lesson.

Examples:

- What data can the tool access?
- Can the action be reversed?
- Should the user approve it first?
- What should be logged?
- What happens when the tool fails?

Security and trust should be recurring design questions throughout the course.

---

# Checkpoint

The learner should answer these without copying the lesson text.

1. Explain the concept in one or two sentences.
2. What problem does it solve in the AI Learning Agent?
3. What new failure mode does it introduce?
4. When would you *not* use it?
5. How would you test whether it is working?

Add 1–3 lesson-specific questions where useful.

---

# Agent Upgrade

Finish the lesson with an explicit capability statement.

> **Before:** [old capability]
>
> **Now:** [new capability]

Then answer the course-wide question:

> **What can our agent do now that it could not do before?**

---

# One-line Takeaway

End with one sentence worth remembering.

Example:

> **A tool turns an agent from a system that can only generate text into a system that can obtain information or take narrowly defined actions.**

---

# Completion Criteria

The lesson is complete when the learner can demonstrate all of the following:

- [ ] explain the concept in plain language;
- [ ] identify the problem it solves;
- [ ] run the relevant sample;
- [ ] modify the sample independently;
- [ ] diagnose at least one failure mode;
- [ ] describe one trade-off;
- [ ] explain the new capability added to the AI Learning Agent.

---

# Author Quality Checklist

Before publishing or approving a lesson, check:

## Explanation

- [ ] Does the lesson start with the big picture?
- [ ] Is the problem understandable before framework terminology appears?
- [ ] Are technical terms defined in plain language?
- [ ] Is there at least one concrete example?
- [ ] Does complexity increase progressively?

## Practice

- [ ] Does the learner modify code or architecture rather than only copy/run it?
- [ ] Is success observable?
- [ ] Is there an intentional failure/debugging task?
- [ ] Is there a clear completion criterion?

## Architecture

- [ ] Is the concept tied to the continuous AI Learning Agent?
- [ ] Are trade-offs discussed?
- [ ] Is there a clear answer to “when should I not use this?”
- [ ] Does the lesson avoid adding complexity before its need is visible?

## Trust and Reliability

- [ ] Does the lesson identify the new risk introduced by the capability?
- [ ] Does it mention approval, permissions, logging, evaluation, or failure handling when relevant?

## Editorial

- [ ] First the problem, then the concept, then the API.
- [ ] No jargon-heavy opening paragraphs.
- [ ] No unexplained framework abstractions.
- [ ] No examples that are more complicated than the concept they teach.
- [ ] No passive “copy this code” exercise as the only practice.
- [ ] No claim that self-checking, RAG, multi-agent, memory, or guardrails guarantee correctness.
