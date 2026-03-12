[![Sample Flow](https://img.shields.io/badge/component-contact_center_flow-0A6ED1?style=for-the-badge)](./README.md)
[![Webex Contact Center](https://img.shields.io/badge/platform-Webex_Contact_Center-00BCEB?style=for-the-badge)](https://www.webex.com/contact-center.html)
[![License](https://img.shields.io/badge/license-Cisco_Sample_Code-1D2733?style=for-the-badge)](../../LICENSE.md)
[![Sample Use Case](https://img.shields.io/badge/scenario-customer_support_triage-2E8540?style=for-the-badge)](../README.md)

# Sample Flow

This folder documents a sample Webex Contact Center flow that supports the same customer support triage scenario described in the AI agent sample.

## Flow Sequence

1. Greet the customer and confirm the interaction has been transferred for assisted support.
2. Retrieve the context passed from the AI agent, including issue summary and prior troubleshooting steps.
3. Verify any required customer details for secure handling.
4. Determine whether the case should remain in the current queue or be routed to a specialized team.
5. Present the live agent with the summarized history so the customer does not need to repeat information.
6. Support the live resolution process and capture any follow-up requirements.
7. Close the interaction with a final disposition and wrap-up notes.

## Purpose

The flow is designed to preserve continuity between automated and human support, reduce repeated questioning, and route the customer to the right team with relevant context already attached.
