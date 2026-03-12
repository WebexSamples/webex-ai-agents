[![AI Agent](https://img.shields.io/badge/component-AI_agent-0A6ED1?style=for-the-badge)](./README.md)
[![Webex](https://img.shields.io/badge/platform-Webex-00BCEB?style=for-the-badge)](https://developer.webex.com/)
[![License](https://img.shields.io/badge/license-Cisco_Sample_Code-1D2733?style=for-the-badge)](../../LICENSE.md)
[![Sample Use Case](https://img.shields.io/badge/scenario-customer_support_triage-2E8540?style=for-the-badge)](../README.md)

# Sample AI Agent

This folder documents a sample Webex AI agent that acts as the first point of contact for customer support triage.

## Responsibilities

- Welcome the customer and collect the initial issue description
- Classify the request by intent, urgency, and likely support domain
- Answer common questions or guide the customer through self-service steps
- Summarize the interaction context for downstream handoff
- Escalate to Webex Contact Center when the issue requires a human agent

## Typical Inputs

- Customer question or problem statement
- Basic account or case identifiers supplied during intake
- Conversation history gathered during the current session

## Typical Outputs

- Intent classification and triage summary
- Suggested next action for self-service or escalation
- Structured context package for transfer to a live support queue

## Escalation Triggers

Escalation should occur when the issue cannot be resolved automatically, requires account-sensitive action, involves repeated failed attempts, or indicates customer frustration that would benefit from human intervention.
