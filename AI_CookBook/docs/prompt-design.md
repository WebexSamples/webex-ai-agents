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
Language models are strong at understanding intent and generating natural responses, but they are less reliable when asked to follow multi-step procedural logic purely from free-form text.

A prompt may describe rules such as:

- if the user has provided an Employee ID, ask what issue they are experiencing
- if the issue is printer-related, ask printer-specific questions
- if the resource is shared, ask whether other users are affected
- if the issue is software-related, ask which application is involved

You might find that writing the above rules with a programming-language style might work better than using human language. 
But flows like the one above implicitly require procedural state: for example, whether `employee_id_check` is true or false, whether a previous step has been completed, and which branch should be executed next. The problem is that an LLM does not natively operate as a deterministic workflow engine with guaranteed state tracking, conditional execution, and control flow. It generates the next response token by token.

However, across the millions of documents seen during training, the model statistically captures different patterns associated with natural language and programming language data.

Human language is not strictly tied to exact wording. Word order may vary, synonyms often preserve the core meaning, and the same concept can be expressed in many different ways. Typos or minor errors are often tolerated without changing the intended meaning. Human language relies heavily on semantics, while allowing broader syntactic variation.

Programming languages, on the other hand, depend heavily on exact syntax. Specific keywords are required, punctuation matters, and a missing comma or bracket may break the entire program. Programming languages require strict syntax and tightly constrained semantics.

For this reason, when procedures are encoded as structured JSON workflow variables, the LLM tends to follow them more precisely than equivalent free-form natural language instructions.

Externalizing workflow logic into a JSON structure helps address both limitations: structured formats strengthen syntactic focus, while state, branching, and execution rules are shifted out of the LLM into an explicit machine-readable layer.


### Recommended Action

Use prompts for conversation, tone, intent recognition, summarization, and general reasoning.

Do not rely on prompts alone to guarantee consistent execution of multi-step procedures, troubleshooting paths, or conditional workflows.

When interactions require mandatory steps, branching logic, state tracking, or repeatable outcomes, place that control logic in an external structured layer, such as a JSON workflow or database-driven flow definition.

In this model, the AI Agent focuses on language understanding and user interaction, while the external workflow layer controls sequence, decisions, and next-step execution.

This separation typically improves reliability, consistency, maintainability, and operational control.

### Best Practice

A good rule of thumb is:

- use the model for interpretation
- use structured data for control

Two implementation models can be considered:

1. Hybrid Control Model  
   Workflow logic is split between prompt instructions and an external JSON/database layer.

2. Fully Externalized Control Model  
   Workflow logic is moved almost entirely into an external JSON/database layer, while the LLM focuses on language understanding, reasoning, and interaction.

#### 1. Hybrid Control Model
For example, imagine an AI Agent used to triage IT issues. After identifying which resource is affected, the agent must ask additional questions depending on the issue type.

Possible follow-up questions are:
- Site location
- Whether other users are experiencing the same issue
- Which application is involved

These questions do not apply equally to every category. Knowing the location of a printer may be important, while it may be irrelevant for access issues on a web application.

If we describe this behavior only in human language, the AI Agent may behave inconsistently. However, if we convert the logic into variables stored in a database and retrieved as JSON, the structured format increases the syntactic focus of the interaction, making the LLM more attentive to exact fields, conditions, and transitions than it would typically be with plain natural language instructions.
This is an example of the JSON variable that can be stored on Webex Connect or an external database:

```
{
  "id": "obj1",
  "category": "Web Application",
  "ask_site": false,
  "check_application": true,
  "check_other_users": false
}

{
  "id": "obj2",
  "category": "Printer",
  "ask_site": true,
  "check_application": false,
  "check_other_users": true
}
```
After identifying the category, the agent retrieves the corresponding configuration.

AI Agent Instruction Example:

1. Identify the user issue and use the [category_list] action to map it to a single category.
2. Use the mapped category to call the [selected_category] action and retrieve its configuration.
3. If `check_application` is `true` and no application has been specified, ask which application is involved.
4. If `ask_site` is `true` and no site is confirmed, ask for the site and validate it.
5. If `check_other_users` is `true` and unknown, determine whether other users are affected.

In this model, logic is partly encoded in the prompt and partly externalized in the JSON/database layer.
Natural language instructions leave broad room for interpretation.
Structured formats such as JSON reduce that ambiguity by constraining the decision space.
Externalizing workflow logic into machine-readable structures does not make the LLM a true executor, but it significantly improves reliability, consistency, and controllability.
It is also possible to externalize the logic almost entirely, while the LLM still provides language understanding, reasoning, and interaction skills.



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

