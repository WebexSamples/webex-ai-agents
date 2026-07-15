# Table of Contents

- [Prompt Engineering for AI Agents](#prompt-engineering-for-ai-agents)
  - [Good Practices for Natural-Language Prompts](#good-practices-for-natural-language-prompts)
    - [General Prompt Engineering Guidelines](#general-prompt-engineering-guidelines)
    - [Be Specific](#be-specific)
	- [Precise Instructions Change Behavior](#precise-instructions-change-behavior)
  	- [Prefer Causal Logic Over Pure Sequence](#prefer-causal-logic-over-pure-sequence)
    - [Use Natural-Language Instructions, Not Code-Like Instructions](#use-natural-language-instructions-not-code-like-instructions)
    - [Use Domain-Specific Terminology](#use-domain-specific-terminology)
  - [When Additional Control Is Needed](#when-additional-control-is-needed)
    - [Workflow Design Guidelines](#workflow-design-guidelines)
     - [Use Actions for Deterministic Operations](#use-actions-for-deterministic-operations)
     - [Prefer Specialized AI Agents for Long Procedures](#prefer-specialized-ai-agents-for-long-procedures)
     - [Externalize Workflow Logic When Necessary](#externalize-workflow-logic-when-necessary)
   - [Best Practices](#best-practices)
    - [Hybrid Control Model](#hybrid-control-model) 


# Prompt Engineering for AI Agents

Prompt engineering is the practice of designing clear and effective instructions, written in natural language, that guide how an AI Agent should behave.

This represents an important shift in system design. Instead of relying solely on traditional programming languages, AI Agent behavior can now be influenced through carefully designed natural-language instructions.

Well-designed prompts can significantly improve tone, consistency, reasoning quality, and the overall user experience.

## Good Practices for Natural-Language Prompts

When prompts are written in natural language, clarity becomes essential. Ambiguous or overly generic instructions often lead to inconsistent behavior.

Effective prompts typically include:

- **Clear objective**  
  Explain what the agent is expected to achieve.

- **Expected behavior**  
  Define the desired tone, style, priorities, or boundaries.

- **Context**  
  Provide relevant background information.

- **Constraints**  
  Specify what the agent must always do, should avoid, or must never do.

- **Output expectations**  
  Describe the expected format or level of detail.

### General Prompt Engineering Guidelines

- **Keep It Simple:** Use clear and concise language. Avoid technical jargon or unnecessarily complex sentences.
- **Use Markdown:** Structure prompts using headings and ordered or unordered lists whenever appropriate.
- **Define the Agent's Role:** Clearly describe the agent's identity or role (for example, *"You are a helpful customer support agent..."*).
- **Break Tasks Down:** Describe complex tasks as a sequence of smaller objectives.
- **Plan for Errors:** Include fallback instructions such as *"I'm sorry, could you please repeat that?"* when user input is unclear.
- **Preserve Context:** Instruct the agent to consider relevant information collected earlier in the conversation.
- **Reference Actions Clearly:** Specify when external actions should be invoked. Ensure that all referenced actions are available and enabled.
- **Add Guardrails:** Clearly define the boundaries within which the AI Agent should operate.
- **Provide Examples:** Include examples whenever they help clarify the expected behavior.

---

### Be Specific

Specify what the AI Agent must do, how it should do it, and any constraints that must be respected.

For example, explicitly state whether a set of rules belongs to an authentication procedure or an identity-verification process. Providing this context helps the AI Agent better understand the purpose of the workflow and apply the rules more consistently.

---

### Precise Instructions Change Behavior

Do not assume that an AI Agent behaves like a human agent. It may interpret instructions differently and therefore produce unexpected outcomes.

#### Weak Prompt Example

`Ask the user for their first name, last name, and Employee ID.`

The AI Agent may ask for all three pieces of information in a single question because nothing explicitly states that it should wait for the user's response before asking the next question.

#### Improved Prompt

`Ask for the first name. Wait for the user's response. Then ask for the last name. Wait for the response. Finally, ask for the Employee ID.`

If necessary, make the constraint explicit:

`Wait for the user's response before asking the next question.`

---

### Prefer Causal Logic Over Pure Sequence

Prompts should express logical dependencies, not just temporal order. Instructions such as *"First do A, then do B"* may be interpreted as guidance rather than as a strict dependency. Whenever possible, explain why a step is required and what condition enables the next one.

#### Weak Example

`Collect the Employee ID, check the HR system, and inform the user of the PTO balance.`

This prompt describes a sequence of tasks but does not explicitly state the dependency between them. Although it may work correctly in many cases, the AI Agent may attempt to query the HR system before obtaining the Employee ID, or generate a response without using the actual system result.

#### Stronger Example

`Ask for the Employee ID, then use it to query the HR system to retrieve the employee's PTO balance. Provide the result returned by the HR system to the user.`

Here, causality is reinforced by explicitly stating that the Employee ID is required to retrieve the PTO balance.

---

### Use Natural-Language Instructions, Not Code-Like Instructions

Although an AI Agent is software, prompts should generally be written as natural-language instructions rather than as pseudo-code. LLMs interpret instructions semantically; they do not execute them as procedural programs.

#### Weak Prompt Example

```text
Step 1: Validate the user-provided value using an authoritative source.
Step 2: If the value is valid, continue to Step 3. Otherwise, go to Step 5.
Step 3: Ask the user to describe the issue.
...
```

This prompt assumes procedural concepts such as state variables, conditional branching, and explicit control flow. However, AI Agents do not execute prompts as traditional programs.

#### Stronger Example

```text
## INPUT VALIDATION

Before continuing, verify that the user-provided value is valid using the appropriate authoritative source.

If the value cannot be validated, ask the user for clarification.

If the value still cannot be validated after multiple attempts, politely end the conversation.

## ISSUE IDENTIFICATION

Once the value has been successfully validated, ask the user to describe the issue they are experiencing.
```

This formulation avoids relying on explicit procedural control flow. Instead, it organizes the workflow into semantic sections connected by causal relationships rather than by explicit *if-then-else* logic.

---

### Use Domain-Specific Terminology

Whenever possible, use domain-specific terminology. Terms such as **Identity Verification**, **Authentication**, **Classification**, **Validation**, and **Retrieval** carry a much more precise operational meaning than generic verbs or terms such as **Identification**, **Check**, **Find**, or **Get**. This richer semantic context often leads to more reliable and consistent AI Agent behavior.

For example, suppose your AI Agent occasionally skips what is intended to be a mandatory **Identification** procedure. From the agent's perspective, the user may already have been identified through the conversation or previous context, causing the procedure to appear unnecessary.

By renaming the procedure to **Identity Verification**, you make its purpose much more explicit. The expression **Identity Verification** conveys that the user's identity must be confirmed by following a defined procedure, even if the agent already knows who the user is. This reduces ambiguity and helps the agent follow the intended workflow more consistently.

In general, procedure names should describe their purpose rather than the individual actions they consist of.

---

## When Additional Control Is Needed

Natural-language prompts are excellent for guidance, tone, reasoning, and intent recognition.

However, when a workflow requires many mandatory steps, strict validation, conditional branching, or repeatable execution, prompts alone may not provide sufficient control.
### Workflow Design Guidelines

Prompt engineering can significantly improve the reliability of AI Agents. However, not every aspect of a workflow should remain inside the prompt. A useful design principle is to assign each task to the component best suited to perform it.

#### Use Actions for Deterministic Operations

Whenever the expected result is deterministic, an external action is generally preferable to asking the LLM to produce it.

Typical examples include:

- mathematical calculations
- date calculations
- comparisons
- database lookups
- eligibility checks
- business rule evaluation

For example, instead of asking the LLM to compare the requested PTO days with the employee's available balance, an external action can perform the calculation and simply return the result.

Similarly, if an identity-verification procedure requires checking that the first name and last name provided by the user match the values stored in the database, the comparison itself should be performed by an action. The AI Agent should simply interpret and communicate the returned result.

A useful rule of thumb is:

> **If the expected output is deterministic, consider implementing it as an action rather than as prompt logic.**
---

The goal is not to replace prompt engineering, but to complement it with deterministic components whenever stronger procedural guarantees are required.

#### Prefer Specialized AI Agents for Long Procedures

As workflows become longer, involve many mandatory steps, require multiple external actions, or contain several decision points, controlling a single AI Agent reliably becomes increasingly difficult.

Whenever possible, the recommended architecture is to use a **concierge AI Agent** responsible for:

- understanding the user's request
- performing the initial triage
- routing the conversation to the appropriate specialized AI Agent

Each specialized AI Agent is then responsible for a single business process, such as:

- PTO balance requests
- absence reporting
- access requests
- troubleshooting
- HR inquiries

This architecture keeps prompts smaller, reduces ambiguity, and generally improves adherence to procedural workflows.

---

#### Externalize Workflow Logic When Necessary

Some environments may not support specialized AI Agents, or a single AI Agent may still be preferred.

In these situations, workflow adherence can be improved by progressively externalizing procedural logic into structured JSON variables stored in Webex Connect or an external database.

Instead of describing an entire workflow in natural language, the AI Agent retrieves only the structured information required for the current task.

Typical examples include:

- workflow configuration
- decision trees
- branching conditions
- question selection
- business rules

This approach allows the LLM to continue doing what it does best—understanding language, interpreting user intent, and interacting naturally—while procedural control is delegated to a deterministic, machine-readable workflow definition.


---

### Best Practices
The following example illustrates this approach using an IT triage workflow.

A useful rule of thumb is:

- use the LLM for interpretation
- use structured data for control

Two implementation models can be considered:

1. **Hybrid Control Model**  
   Workflow logic is split between prompt instructions and an external JSON/database layer.

2. **Fully Externalized Control Model**  
   Workflow logic is moved almost entirely into an external JSON/database layer, while the LLM focuses on language understanding, reasoning, and interaction. Although this model is outside the scope of this document, it is included here for architectural completeness.

---

#### Hybrid Control Model

Imagine an AI Agent used to triage IT issues.

After identifying the affected resource, the agent must ask additional questions depending on the issue category.

Possible follow-up questions include:

- Site location
- Whether other users are experiencing the same issue
- Which application is involved

These questions are not applicable to every category.

For example, knowing the location of a printer may be essential, while it may be irrelevant for an issue involving a web application.

---

**Benefit of Structured Variables**

Structured JSON variables strengthen the syntactic focus of the interaction, making the LLM more attentive to exact fields, conditions, and transitions.

---

If this behavior is described only in natural language, the AI Agent may behave inconsistently. However, if the workflow logic is represented as structured variables stored in a database and retrieved as JSON, the structured format increases the syntactic focus of the interaction, making the LLM more attentive to exact fields, conditions, and transitions than it would typically be with equivalent natural-language instructions.

Below is an example of a JSON structure that can be stored in Webex Connect or an external database:

```json
[
  {
    "id": "obj1",
    "category": "Web Application",
    "ask_site": false,
    "check_application": true,
    "check_other_users": false
  },
  {
    "id": "obj2",
    "category": "Printer",
    "ask_site": true,
    "check_application": false,
    "check_other_users": true
  }
]
```

After identifying the issue category, the AI Agent retrieves the corresponding configuration.

The following are the instructions for the AI Agent:

```text
1. Identify the user's issue and use the [category_list] action to map it to a single category.
2. Use the selected category to call the [selected_category] action and retrieve its configuration.

Then evaluate the following conditions:

- If `check_application` is `true` and no application has been specified, ask which application is involved.
- If `ask_site` is `true` and no site has been confirmed, ask for the site and validate it.
- If `check_other_users` is `true` and you do not yet know whether other users are affected, ask the user whether anyone else is experiencing the same issue.
```

In this model, workflow logic is partly encoded in the prompt and partly externalized in the JSON/database layer.

Natural-language instructions leave broader room for interpretation. Structured formats such as JSON reduce that ambiguity by constraining the decision space.

Externalizing workflow logic into machine-readable structures does not turn the LLM into a deterministic workflow engine, but it significantly improves reliability, consistency, maintainability, and controllability.

For workflows that require even stronger procedural guarantees, the control logic can be externalized almost entirely while the LLM continues to provide language understanding, reasoning, and natural interaction.

