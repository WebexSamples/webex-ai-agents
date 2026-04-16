## When Prompts Are Not Enough

A common design mistake in AI agents is assuming that a prompt alone can reliably enforce a step-by-step business procedure.

This becomes a problem when the agent is expected to behave intelligently in conversation while also following a strict troubleshooting path, validation sequence, or policy-driven decision flow.

### Problem Statement

In many customer support and contact center scenarios, the agent must do more than answer questions naturally. It must also follow a sequence of required checks, ask specific questions when certain conditions are met, and avoid skipping important steps.

Examples include:

- troubleshooting flows
- issue triage
- printer or network diagnostics
- access request handling
- policy-based routing or escalation

The challenge is that large language models do not execute business logic in the same way that software does. Even when instructions are written clearly in a prompt or a knowledge base, the model may:

- skip steps
- ask too many questions
- ask questions out of order
- interpret instructions loosely
- fail to branch consistently
- hallucinate values when exact matching is required

This is especially risky when the workflow depends on deterministic behavior.

### Why This Happens

Language models are strong at interpreting meaning and generating natural language, but they are not inherently reliable at procedural control flow.

A prompt can describe logic such as:

- if the issue is printer-related, ask printer questions
- if the user is on a shared resource, ask whether other users are affected
- if the issue is software-related, ask which application is involved

However, describing logic is not the same as executing logic.

Prompts and knowledge-base instructions live in natural language, which is flexible and ambiguous. Deterministic workflows need something more structured.

### Recommended Action

Use prompts for conversation, tone, classification, and general reasoning.

Do not rely on prompts alone to enforce structured troubleshooting or conditional workflows.

For guided diagnostic flows, use a structured decision model such as a JSON decision tree or a database-driven question graph.

This pattern works well:

1. Use the AI agent to understand the user request and identify the issue category.
2. Retrieve the appropriate structured flow for that category.
3. Ask only the question defined by the current step.
4. Use the user response to select the next branch.
5. Continue until the flow reaches a conclusion, recommendation, or escalation point.

### Best Practice

A good rule of thumb is:

- use the model for interpretation
- use structured data for control

This keeps the conversational experience natural while making the workflow predictable and reviewable.

### When To Use This Pattern

This pattern is a good fit when the agent must:

- follow a troubleshooting sequence
- apply policy-driven branching
- validate exact values from a known list
- avoid skipping mandatory checks
- support repeatable service workflows
- provide consistent outcomes across users

### Additional Recommendation

If the workflow depends on exact values such as site names, issue categories, or approved options, validate them through tool calls or a database lookup instead of relying on the model to infer them from text alone.

This reduces hallucination risk and improves reliability.

### What This Means For Prompt Design

Prompt engineering still matters, but prompts should not carry responsibilities that belong to workflow logic.

A strong prompt should tell the agent:

- what its role is
- when to use a structured flow
- when to call a tool
- when to stop and escalate
- what it must not do

But the decision logic itself should live outside the prompt whenever reliability matters.

### Practical Takeaway

If a process must be followed consistently, do not store that process only as prose in a prompt or knowledge base.

Move the workflow into a structure the system can follow explicitly, such as:

- a decision tree
- a JSON workflow
- a rule-based state graph
- a validated tool-driven sequence

Use the prompt to guide behavior around that structure, not to replace it.

