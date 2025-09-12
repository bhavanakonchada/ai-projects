# Ticket Classifier Agent

## Overview
The Ticket Classifier Agent is a production-ready customer service automation component that analyzes incoming customer messages and classifies them for proper routing within an organization's support system. This agent is designed to integrate seamlessly with Azure AI Foundry's multi-agent orchestration system.

## Purpose
- **Multi-dimensional Classification**: Analyzes tickets across priority (Critical/High/Medium/Low), department (Technical/Billing/Sales/General), and issue type (Bug/Feature/Account/Billing/General)
- **Confidence Scoring**: Provides 0.0-1.0 confidence scores for classification reliability  
- **SLA-aware Routing**: Suggests response timeframes based on priority and issue type
- **Context-aware Analysis**: Includes routing notes for human agents with relevant context

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

## Technical Specifications
- **Model**: GPT-4o with optimized parameters (temperature: 0.1, max_tokens: 1000)
- **Input Schema**: Flexible JSON accepting message, customer_id, timestamp, and channel
- **Output Schema**: Structured JSON with priority, department, issue_type, confidence, response_time, and routing_notes
- **Integration**: Designed for orchestrator-agent communication patterns

## Sample Classifications
The agent handles diverse scenarios including:
- **Critical Issues**: System outages with immediate escalation (confidence: 0.98)
- **Billing Inquiries**: Duplicate charges with same-day resolution (confidence: 0.95)
- **Feature Requests**: Enhancement suggestions with product team routing (confidence: 0.85)
- **Security Concerns**: Unauthorized access with immediate investigation (confidence: 0.95)

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

## Getting Started
1. Review the 10 realistic sample tickets in `examples/sample-tickets.json`
2. Examine the 8 few-shot learning examples in `prompts/examples.json`
3. Study the classification prompts in `prompts/system-prompt.txt`
4. Follow the comprehensive deployment guide in `docs/deployment-guide.md`
5. Test with the provided examples and expected outputs in `examples/classification-results.json`

## Complete Implementation Structure
```
agents/ticket-classifier/
├── config/
│   ├── agent-config.json    # Azure AI Foundry agent configuration
│   └── deployment.yaml      # Kubernetes deployment specification
├── prompts/
│   ├── system-prompt.txt    # Comprehensive classification prompt
│   └── examples.json        # 8 diverse few-shot learning examples
├── examples/
│   ├── sample-tickets.json  # 10 realistic test scenarios
│   └── classification-results.json  # Expected outputs with analysis
├── docs/
│   ├── deployment-guide.md  # Step-by-step Azure deployment
│   └── troubleshooting.md   # Comprehensive issue resolution guide
└── README.md               # Complete documentation and usage guide
```