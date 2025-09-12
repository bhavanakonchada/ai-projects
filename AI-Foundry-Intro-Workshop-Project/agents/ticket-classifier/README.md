# Ticket Classifier Agent

## Overview
The Ticket Classifier Agent is a customer service automation component that analyzes incoming customer messages and classifies them for proper routing within an organization's support system. This agent is designed to work within Azure AI Foundry as part of an orchestrated multi-agent system.

## Purpose
- **Analyze** customer service tickets and messages
- **Classify** tickets into appropriate categories for routing
- **Route** tickets to the correct department or support level
- **Improve** response times by automating initial triage

## Classification Categories
The agent classifies tickets into the following categories:

### Priority Levels
- **Critical**: System outages, security issues, data loss
- **High**: Service degradation, billing issues, urgent feature requests
- **Medium**: General inquiries, feature requests, minor bugs
- **Low**: Documentation requests, general questions

### Department Routing
- **Technical Support**: Bug reports, technical issues, system problems
- **Billing**: Payment issues, subscription changes, invoicing
- **Sales**: Product inquiries, upgrade requests, new features
- **General**: Account questions, policy inquiries, feedback

### Issue Types
- **Bug Report**: Software defects, errors, unexpected behavior
- **Feature Request**: New functionality, improvements, enhancements
- **Account Issue**: Login problems, permissions, account management
- **Billing Inquiry**: Charges, payments, subscription management
- **General Question**: Information requests, how-to questions

## Integration with Orchestrator
This agent is designed to work as part of a larger orchestrated system where:
1. The Orchestrator Agent receives initial customer messages
2. Routes messages to the Ticket Classifier Agent for analysis
3. Receives classification results and routing recommendations
4. Directs tickets to appropriate specialized agents or human operators

## Agent Configuration
The agent uses Azure AI Foundry's agent framework with custom prompts and classification logic. See the `config/` directory for deployment configurations and the `prompts/` directory for the prompt engineering templates.

## Usage
1. Deploy the agent configuration to Azure AI Foundry
2. Connect to the Orchestrator Agent
3. Configure routing rules based on classification outputs
4. Monitor and adjust classification accuracy over time

## Files Structure
```
ticket-classifier/
├── README.md              # This documentation
├── config/
│   ├── agent-config.json  # Azure AI Foundry agent configuration
│   └── deployment.yaml    # Deployment settings
├── prompts/
│   ├── system-prompt.txt  # Main classification prompt
│   └── examples.json      # Few-shot learning examples
├── examples/
│   ├── sample-tickets.json # Sample customer messages
│   └── classification-results.json # Expected classifications
└── docs/
    ├── deployment-guide.md # Step-by-step deployment
    └── troubleshooting.md  # Common issues and solutions
```

## Getting Started
1. Review the sample tickets in `examples/sample-tickets.json`
2. Examine the classification prompts in `prompts/`
3. Follow the deployment guide in `docs/deployment-guide.md`
4. Test with the provided examples before production deployment