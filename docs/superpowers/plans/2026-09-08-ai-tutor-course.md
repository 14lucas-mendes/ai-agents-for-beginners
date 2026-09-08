# AI Tutor Course Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Transform the existing 18-lesson AI Agents for Beginners repository into a coherent, project-driven course that applies the AI Tutor teaching method and progressively builds one AI Learning Agent.

**Architecture:** Keep the existing lesson folders and code samples intact, but add a pedagogical layer that connects them through a single evolving capstone. Each lesson should introduce the problem first, explain the concept in plain language, then move into implementation, practice, failure analysis, and a measurable checkpoint.

**Tech Stack:** Markdown, Jupyter notebooks, Python 3.12+, Microsoft Agent Framework, Microsoft Foundry / Azure OpenAI.

**Spec:** `COURSE_PLAN.md`

## Global Constraints

- Preserve the existing numbered lesson structure (`00` through `18`).
- Do not rewrite notebooks until the corresponding lesson design is approved.
- Use plain English/Portuguese first; introduce framework/API terminology only after the learner understands the problem it solves.
- Every lesson must contain at least one concrete example and one hands-on task.
- Every lesson must answer: “What can the agent do now that it could not do before?”
- Use one continuous capstone, the **AI Learning Agent**, across the curriculum.
- Prefer approximately 40% concept and 60% practice for instructor-led sessions.
- Keep security, evaluation, observability, and human approval as recurring concerns rather than end-of-course-only topics.

---

### Task 1: Establish the curriculum specification

**Files:**
- Create: `COURSE_PLAN.md`

**Interfaces:**
- Consumes: Existing lesson sequence and `STUDY_GUIDE.md`.
- Produces: The canonical curriculum map used by all later lesson rewrites.

- [ ] **Step 1: Write the course promise and learner profile**

Define the target learner, prerequisites, final outcome, and the mental-model shift expected by the end of the course.

- [ ] **Step 2: Map the 18 lessons into progressive phases**

Use these phases:

1. Understand Agents
2. Give Agents Capabilities
3. Orchestrate Intelligence
4. Engineer Agent Systems
5. Build in the Real World
6. Ship Safely
7. Capstone

- [ ] **Step 3: Define the evolving AI Learning Agent**

Document versions `v0` through `v8`, with one new capability added at each stage.

- [ ] **Step 4: Define delivery and assessment**

Include lesson rhythm, hands-on weighting, checkpoints, project increments, capstone rubric, and architectural justification.

- [ ] **Step 5: Review for consistency**

Check that lessons `00`–`18` all appear, that each phase has a clear learner outcome, and that the project progression has no capability appearing before it is taught.

- [ ] **Step 6: Commit**

Commit message:

```text
docs: add AI Tutor curriculum plan
```

---

### Task 2: Create the lesson authoring standard

**Files:**
- Create: `LESSON_TEMPLATE.md`

**Interfaces:**
- Consumes: `COURSE_PLAN.md` and AI Tutor principles.
- Produces: A repeatable authoring template for every lesson README and supporting notebook narrative.

- [ ] **Step 1: Define the lesson opening**

Require Big Picture, Status Quo, Problem, Solution, and a concrete scenario before technical implementation.

- [ ] **Step 2: Define the teaching core**

Require What → Why → How, plain-language explanation, terminology, and a concrete example tied to the AI Learning Agent.

- [ ] **Step 3: Define practice**

Require guided walkthrough, learner modification, failure mode/debugging exercise, and an explicit deliverable.

- [ ] **Step 4: Define evidence of learning**

Require checkpoint questions, one-line takeaway, completion criteria, and the “agent can now…” capability statement.

- [ ] **Step 5: Add authoring quality checklist**

Check for jargon-before-intuition, unexplained abstractions, weak examples, passive copy/paste exercises, missing failure modes, and missing trade-offs.

- [ ] **Step 6: Commit**

Commit message:

```text
docs: add AI Tutor lesson template
```

---

### Task 3: Pilot the method on Lesson 01

**Files:**
- Modify: `01-intro-to-ai-agents/README.md`
- Inspect: `01-intro-to-ai-agents/code_samples/*-python-agent-framework.ipynb`

**Interfaces:**
- Consumes: `LESSON_TEMPLATE.md`.
- Produces: The reference implementation for later lesson rewrites.

- [ ] **Step 1: Preserve factual and technical content**

Inventory the current learning objectives, diagrams, code links, terminology, and resources so none are lost during restructuring.

- [ ] **Step 2: Rewrite the lesson opening**

Start from the difference between a chatbot that answers and an agent that can pursue a goal using context and actions.

- [ ] **Step 3: Connect Lesson 01 to AI Learning Agent v0**

Use the learner request “Quero aprender Tool Use” as the concrete scenario and explain why v0 is still only conversational.

- [ ] **Step 4: Add a checkpoint and mini-exercise**

The learner must identify model, context, tools, and control in one proposed agent system.

- [ ] **Step 5: Validate links and notebook references**

Confirm every referenced path still exists and all existing technical resources remain reachable.

- [ ] **Step 6: Commit**

Commit message:

```text
[Lesson-01] apply AI Tutor lesson structure
```

---

### Task 4: Roll out Lessons 02–06

**Files:**
- Modify: `02-explore-agentic-frameworks/README.md`
- Modify: `03-agentic-design-patterns/README.md`
- Modify: `04-tool-use/README.md`
- Modify: `05-agentic-rag/README.md`
- Modify: `06-building-trustworthy-agents/README.md`

**Interfaces:**
- Consumes: Lesson 01 as the reference implementation.
- Produces: The complete beginner foundation phase and AI Learning Agent versions v1–v2.

- [ ] **Step 1: Rewrite Lesson 02 around framework motivation**
- [ ] **Step 2: Rewrite Lesson 03 around designing behavior before code**
- [ ] **Step 3: Rewrite Lesson 04 around giving an agent its first external capability**
- [ ] **Step 4: Rewrite Lesson 05 around grounding answers in course content**
- [ ] **Step 5: Rewrite Lesson 06 around risk introduced by increased autonomy**
- [ ] **Step 6: Cross-check terminology and progressive complexity across Lessons 01–06**
- [ ] **Step 7: Commit each lesson independently**

Use descriptive `[Lesson-XX]` commit titles matching repository conventions.

---

### Task 5: Roll out planning and orchestration Lessons 07–09

**Files:**
- Modify: `07-planning-design/README.md`
- Modify: `08-multi-agent/README.md`
- Modify: `09-metacognition/README.md`

**Interfaces:**
- Consumes: AI Learning Agent v2.
- Produces: AI Learning Agent v3 with planning, optional specialization, and output review.

- [ ] **Step 1: Teach planning as goal decomposition rather than hidden magic**
- [ ] **Step 2: Teach multi-agent only after the single-agent limits are clear**
- [ ] **Step 3: Teach review/self-check as a reliability mechanism with explicit limits**
- [ ] **Step 4: Add a single-agent-vs-multi-agent architecture decision exercise**
- [ ] **Step 5: Commit each lesson independently**

---

### Task 6: Roll out system engineering Lessons 10–13

**Files:**
- Modify: `10-ai-agents-production/README.md`
- Modify: `11-agentic-protocols/README.md`
- Modify: `12-context-engineering/README.md`
- Modify: `13-agent-memory/README.md`

**Interfaces:**
- Consumes: AI Learning Agent v3.
- Produces: AI Learning Agent v4–v5 with observability, protocol integration, context management, and memory.

- [ ] **Step 1: Make production concerns concrete through failure/cost/latency scenarios**
- [ ] **Step 2: Introduce protocols as an integration problem before naming protocol APIs**
- [ ] **Step 3: Contrast context, knowledge, and memory using the same learner scenario**
- [ ] **Step 4: Add a memory-safety decision exercise**
- [ ] **Step 5: Commit each lesson independently**

---

### Task 7: Roll out real-world implementation Lessons 14–17

**Files:**
- Modify: `14-microsoft-agent-framework/README.md`
- Modify: `15-browser-use/README.md`
- Modify: `16-deploying-scalable-agents/README.md`
- Modify: `17-creating-local-ai-agents/README.md`

**Interfaces:**
- Consumes: AI Learning Agent v5.
- Produces: AI Learning Agent v6–v8 with framework abstractions, environment interaction, deployment, and local-first alternatives.

- [ ] **Step 1: Position Microsoft Agent Framework as an abstraction over already-learned concepts**
- [ ] **Step 2: Teach computer/browser use with explicit approval boundaries**
- [ ] **Step 3: Teach deployment through reliability, routing, caching, evaluation gates, and smoke tests**
- [ ] **Step 4: Teach local agents through privacy, latency, connectivity, and capability trade-offs**
- [ ] **Step 5: Commit each lesson independently**

---

### Task 8: Close the course with security and capstone

**Files:**
- Modify: `18-securing-ai-agents/README.md`
- Modify: `STUDY_GUIDE.md`
- Modify: `README.md`

**Interfaces:**
- Consumes: All previous lesson revisions.
- Produces: A discoverable, end-to-end course journey and final capstone instructions.

- [ ] **Step 1: Reframe Lesson 18 as the consolidation of recurring security decisions**
- [ ] **Step 2: Add the final AI Learning Agent capstone specification**
- [ ] **Step 3: Update `STUDY_GUIDE.md` to link course phases, project versions, and checkpoints**
- [ ] **Step 4: Update the root README with a concise link to the new course plan**
- [ ] **Step 5: Validate all lesson links and course navigation**
- [ ] **Step 6: Commit documentation integration**

Commit message:

```text
docs: integrate AI Tutor learning path
```
