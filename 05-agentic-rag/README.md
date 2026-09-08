[![Agentic RAG](./images/lesson-5-thumbnail.png)](https://youtu.be/WcjAARvdL7I?si=BCgwjwFb2yCkEhR9)

> _(Click the image above to view the video for this lesson)_

# Agentic RAG

## Big Picture

In Lesson 04, our AI Learning Agent gained its first tool. It can now search outside the model.

But a new problem appears immediately:

> **Retrieving something is not the same as answering from evidence.**

By the end of this lesson, you should be able to explain the difference between ordinary RAG and Agentic RAG, design a retrieval loop that can refine its search, and recognize when the agent should stop, retry, or ask for help.

---

## Before This Lesson: We Can Search, but We Can Still Be Wrong

Suppose the learner asks:

> “Qual é a melhor sequência para estudar RAG neste curso?”

Our `search_lessons()` tool returns three files.

That sounds good, but several things can still go wrong:

- the search query may be too broad;
- the top result may mention RAG only briefly;
- relevant information may be spread across multiple lessons;
- the agent may summarize something that the source does not actually say;
- the first retrieval attempt may simply be poor.

The problem is no longer access.

The problem is **grounding the answer in useful evidence**.

---

## The New Capability: Retrieval-Augmented Generation

### What is RAG?

**Retrieval-Augmented Generation (RAG)** means retrieving external information and giving that information to the model so the answer can be grounded in source material.

A basic RAG flow looks like this:

```text
User question
    ↓
Retrieve relevant documents
    ↓
Add retrieved content to model context
    ↓
Generate grounded answer
```

### Why does it matter?

A model may know general facts about AI agents, but our course assistant should answer questions about **this repository** from the repository itself.

RAG lets us separate:

- what the model already knows;
- what the application retrieves;
- what evidence supports the final answer.

### How is Agentic RAG different?

Traditional RAG often uses a fixed path:

```text
retrieve once → generate once
```

**Agentic RAG** lets the system decide whether the retrieved evidence is sufficient and take another retrieval action when needed.

```text
Question
   ↓
Retrieve
   ↓
Evaluate evidence
   ↓
Enough? ── yes ──→ Answer
   │
   no
   ↓
Rewrite query / choose another source
   ↓
Retrieve again
```

The important idea is not “more loops.”

The important idea is **adaptive retrieval based on what the system finds**.

---

## Concrete Example: AI Learning Agent v2

The learner asks:

> “Quero aprender como agentes usam ferramentas. O que devo ler primeiro e por quê?”

A weak implementation might search once for `tools` and summarize the first result.

A stronger flow can look like this:

1. search for `tool use`;
2. inspect Lesson 04;
3. notice that frameworks are referenced as prerequisite context;
4. inspect Lesson 02;
5. build a short sequence: Lesson 02 → Lesson 04;
6. cite the files used;
7. explain why that order makes sense.

Now the agent is not merely retrieving a document. It is using retrieval as part of a multi-step information-gathering process.

> **AI Learning Agent v2 can ground its recommendations in retrieved course material and refine its search when the first result is insufficient.**

---

## The Agentic Retrieval Loop

A useful mental model is:

### 1. Understand the information need

What evidence is actually required to answer the user's question?

### 2. Choose a retrieval action

Examples:

- vector or hybrid search;
- file search;
- SQL query;
- web search;
- custom repository search.

### 3. Inspect the result

The system should ask whether the result is:

- relevant;
- specific enough;
- current enough for the task;
- supported by the expected source.

### 4. Refine if needed

The agent may:

- rewrite the search query;
- narrow the scope;
- query another source;
- retrieve supporting evidence.

### 5. Stop deliberately

A loop needs a stopping rule.

Possible conditions:

- enough evidence was found;
- a maximum number of retrieval attempts was reached;
- remaining uncertainty requires human clarification;
- a source is unavailable.

Without stopping conditions, “agentic” can become an expensive infinite retry loop.

---

## What Agentic RAG Is Not

Agentic RAG does not mean the system has unlimited autonomy.

Its agency is bounded by:

- available retrieval tools;
- accessible data;
- application policies;
- tool permissions;
- time/cost limits;
- human approval rules.

The agent can choose among the retrieval strategies you expose. It cannot safely invent arbitrary access to systems you did not provide.

---

## Retrieval Sources

Agentic RAG may combine different information sources.

### Unstructured documents

Examples:

- Markdown files;
- PDFs;
- documentation;
- support articles.

Often accessed through file search, embeddings, vector search, or hybrid search.

### Structured data

Examples:

- SQL databases;
- analytics stores;
- CRM records.

Structured retrieval often needs careful validation because generated queries can fail or request inappropriate data.

### External/current sources

Examples:

- APIs;
- web search;
- live business systems.

These can improve freshness but introduce reliability, security, and provenance concerns.

---

## Failure Mode: Irrelevant Retrieval

Imagine the search returns a document that contains the phrase “tool use” but is actually about deployment.

If the agent trusts rank alone, it may produce a polished but poorly grounded answer.

A better system asks:

> Does this retrieved content actually answer the information need?

Possible responses include:

- retrieve more candidates;
- use a more specific query;
- inspect metadata;
- require multiple supporting sources for high-stakes claims.

---

## Failure Mode: Query Failure

Structured queries can fail syntactically or semantically.

An agentic system may retry with a corrected query, but retries need boundaries.

A useful pattern is:

```text
Attempt query
    ↓
Success? → inspect result
    ↓ no
Classify failure
    ↓
Safe to retry? → rewrite and retry
    ↓ no
Stop / escalate
```

Blind retrying is not self-correction. It is repeated failure.

---

## Failure Mode: Retrieval Loops

Suppose the agent repeatedly decides:

> “I need one more search.”

Every extra retrieval increases:

- latency;
- token usage;
- API cost;
- opportunity for irrelevant context.

Useful safeguards include:

- maximum iteration count;
- per-request budget;
- minimum evidence threshold;
- duplicate-query detection;
- human fallback.

---

## Memory and State Inside a Retrieval Session

During one agent run, the system should remember what it already tried.

For example:

```text
Attempt 1: "RAG"
Result: too broad

Attempt 2: "agentic RAG retrieval loop"
Result: Lesson 05

Attempt 3: "context engineering RAG"
Result: Lesson 12
```

This is **working state for the current task**.

Do not automatically confuse it with long-term memory about the user. We will separate those ideas in Lessons 12 and 13.

---

## Evidence and Citations

A grounded system should make it possible to answer:

> “Where did this claim come from?”

For the AI Learning Agent, the easiest evidence is the course file path or lesson link used during retrieval.

Example:

```text
Recommended first:
- Lesson 04 — Tool Use

Why:
It introduces function/tool calling before Lesson 05 builds retrieval workflows on top of tool use.

Sources used:
- 04-tool-use/README.md
- 05-agentic-rag/README.md
```

Citations do not guarantee correctness, but they make verification and debugging much easier.

---

## Hands-On: Design an Agentic Retrieval Loop

For the request:

> “Quero aprender memória de agentes, mas primeiro preciso entender os pré-requisitos.”

Design a retrieval trace with:

1. initial search query;
2. expected result;
3. evidence-quality check;
4. one possible query refinement;
5. stopping condition;
6. final sources shown to the learner.

### Deliverable

A retrieval plan with **at least two possible search steps** and an explicit reason the loop stops.

---

## Guided Code Samples

Inspect the existing lesson implementations after you understand the retrieval loop:

- Python: [`code_samples/05-python-agent-framework.ipynb`](./code_samples/05-python-agent-framework.ipynb)
- .NET notebook: [`code_samples/05-dotnet-agent-framework.ipynb`](./code_samples/05-dotnet-agent-framework.ipynb)
- .NET walkthrough: [`code_samples/05-dotnet-agent-framework.md`](./code_samples/05-dotnet-agent-framework.md)
- Sample source document: [`code_samples/document.md`](./code_samples/document.md)

As you read the code, identify:

```text
User information need
Retrieval tool
Retrieved evidence
Decision to continue or stop
Final grounded response
```

---

## Trust Check

Agentic retrieval creates its own risks.

Ask:

- Which sources is the agent allowed to query?
- Can retrieved content contain malicious instructions?
- How do we distinguish source data from trusted system instructions?
- What happens if the knowledge source is poisoned or outdated?
- Do sensitive queries require access controls?
- What evidence is logged for later review?

These questions connect directly to Lesson 06.

---

## Optional: Smoke Test After Deployment

After reaching [Lesson 16](../16-deploying-scalable-agents/README.md), you can smoke-test the deployed Lesson 05 `TravelRAGAgent` with [`tests/lesson-05-smoke-tests.json`](../tests/lesson-05-smoke-tests.json). See [`tests/README.md`](../tests/README.md).

---

## Checkpoint

Explain these in your own words:

1. What does RAG add that a normal model response does not?
2. What makes Agentic RAG different from a fixed retrieve-then-generate pipeline?
3. Why does an agentic retrieval loop need an explicit stopping rule?
4. What is the difference between retrieval state in one run and long-term user memory?
5. What should the system do when the first retrieval result is weak?
6. Why are source traces useful even when the final answer looks correct?

---

## AI Learning Agent Progress

**Before:** AI Learning Agent v1 could call a search tool.

**Now:** **AI Learning Agent v2 can use retrieved course material as evidence, refine retrieval when needed, and show the learner which sources supported the answer.**

That increased capability also increases risk.

The next question is:

> What prevents a capable agent from using the right tool in the wrong way?

That is the focus of Lesson 06.

---

## One-Line Takeaway

> **Agentic RAG is not just search plus an LLM; it is an evidence-gathering loop that can adapt its retrieval strategy while remaining bounded by explicit tools and stopping rules.**

---

## Additional Resources

- <a href="https://learn.microsoft.com/training/modules/use-own-data-azure-openai" target="_blank">Implement Retrieval Augmented Generation (RAG) with Azure OpenAI Service</a>
- <a href="https://learn.microsoft.com/azure/ai-studio/concepts/evaluation-approach-gen-ai" target="_blank">Evaluation of generative AI applications with Microsoft Foundry</a>
- <a href="https://weaviate.io/blog/what-is-agentic-rag" target="_blank">What is Agentic RAG | Weaviate</a>
- <a href="https://huggingface.co/learn/cookbook/agent_rag" target="_blank">Hugging Face Agentic RAG cookbook</a>
- <a href="https://arxiv.org/abs/2501.09136" target="_blank">Agentic Retrieval-Augmented Generation: A Survey on Agentic RAG</a>

## Previous Lesson

[Tool Use Design Pattern](../04-tool-use/README.md)

## Next Lesson

[Building Trustworthy AI Agents](../06-building-trustworthy-agents/README.md)
