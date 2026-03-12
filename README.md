[![Docs Only](https://img.shields.io/badge/repo-docs--only-0A6ED1?style=for-the-badge)](./README.md)
[![Webex](https://img.shields.io/badge/platform-Webex-00BCEB?style=for-the-badge)](https://developer.webex.com/)
[![License](https://img.shields.io/badge/license-Cisco_Sample_Code-1D2733?style=for-the-badge)](./LICENSE.md)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-2E8540?style=for-the-badge)](./CONTRIBUTING.md)

# webex-ai-agents

`webex-ai-agents` is a documentation-first sample repository that captures the intent, structure, and collaboration model for Webex AI agent use cases. It is designed to show how an AI-first customer interaction can connect to a Webex Contact Center flow when self-service is no longer sufficient.

## Repository Intent

This repository provides a clean starting point for documenting:

- Sample Webex AI agent use cases
- Sample Webex Contact Center flow design
- The relationship between AI-led self-service and human-assisted escalation

The current sample scenario focuses on customer support triage. In this example, a Webex AI agent handles initial intake, identifies the customer issue, attempts resolution for common requests, and passes structured context into Webex Contact Center when escalation is required.

## Sample Structure

```text
.
├── README.md
├── CONTRIBUTING.md
├── LICENSE.md
└── sample-use-case/
    ├── README.md
    ├── sample-ai-agent/
    │   └── README.md
    └── sample-flow/
        └── README.md
```

## Included Example

The sample content is organized around a single customer support triage journey:

- [`sample-use-case/`](./sample-use-case/) explains the end-to-end example
- [`sample-use-case/sample-ai-agent/`](./sample-use-case/sample-ai-agent/) describes the AI agent responsibilities and escalation logic
- [`sample-use-case/sample-flow/`](./sample-use-case/sample-flow/) outlines the related Webex Contact Center flow

## Contributing

Contributions should keep the repository documentation-focused, consistent, and easy to navigate. Follow the guidance in [`CONTRIBUTING.md`](./CONTRIBUTING.md) before opening a pull request.

## License

This repository is provided under the Cisco Sample Code License. See [`LICENSE.md`](./LICENSE.md).
