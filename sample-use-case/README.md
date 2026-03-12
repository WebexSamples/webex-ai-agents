[![Sample Use Case](https://img.shields.io/badge/sample-customer_support_triage-0A6ED1?style=for-the-badge)](./README.md)
[![Webex](https://img.shields.io/badge/platform-Webex-00BCEB?style=for-the-badge)](https://developer.webex.com/)
[![License](https://img.shields.io/badge/license-Cisco_Sample_Code-1D2733?style=for-the-badge)](../LICENSE.md)
[![Contributing](https://img.shields.io/badge/contributing-guidelines-2E8540?style=for-the-badge)](../CONTRIBUTING.md)

# Sample Use Case

This sample use case documents a customer support triage journey that starts with a Webex AI agent and escalates to Webex Contact Center when human assistance is required.

## Scenario Overview

In this example, a customer reaches out for support with a service or account issue. The AI agent captures the request, determines intent, responds to common questions, and decides whether the interaction can be resolved through self-service or must be transferred to a live support team.

## How The Sample Is Organized

- [`sample-ai-agent/`](./sample-ai-agent/) describes the AI-first interaction model
- [`sample-flow/`](./sample-flow/) describes the Webex Contact Center flow that receives escalated conversations

Together, these folders show how structured context can move from automated handling to human-assisted resolution without losing the customer narrative.
