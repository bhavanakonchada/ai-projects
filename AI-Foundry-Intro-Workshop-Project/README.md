# Customer Support AI Agent Orchestration

A multi-agent AI system built for Azure AI Foundry that handles customer support tickets through intelligent agent orchestration. The system processes customer inquiries through specialized agents that classify, research, and generate professional responses.

<img width="771" height="648" alt="agent-orchestration-1" src="https://github.com/user-attachments/assets/151ad398-b659-4dbe-8b0e-6ba75daeac22" />


## 🏗️ Architecture Overview

```
Customer Input → Orchestrator Agent → [3 Specialized Agents] → Final Response
                      ↓
              ┌─────────────────┐
              │ Ticket          │ ← Classifies category, urgency, routing
              │ Classifier      │
              └─────────────────┘
                      ↓
              ┌─────────────────┐
              │ Knowledge Base  │ ← Searches solutions, assesses confidence
              │ Agent           │
              └─────────────────┘
                      ↓
              ┌─────────────────┐
              │ Response        │ ← Generates professional customer response
              │ Generator       │
              └─────────────────┘
```

## 🚀 Quick Start

1. **Deploy Agents**: Use system prompts from [`/agents/`](./agents/) folder
2. **Upload Knowledge Base**: Import [`/data/knowledge-base.json`](./data/knowledge-base.json)
3. **Run Tests**: Try scenarios from [`/tests/`](./tests/) folder
4. **Follow Setup Guide**: Complete instructions in [`/setup/deployment-guide.md`](./setup/deployment-guide.md)

## 📁 Repository Structure

- **[`/agents/`](./agents/)** - System prompts for all 4 agents
- **[`/tests/`](./tests/)** - Individual agent tests and end-to-end scenarios  
- **[`/setup/`](./setup/)** - Deployment and configuration guides
- **[`/data/`](./data/)** - Knowledge base and sample data
- **[`/examples/`](./examples/)** - Code examples and usage patterns

## 🤖 Agent Components

| Agent | Purpose | Input | Output |
|-------|---------|-------|--------|
| **[Orchestrator](./agents/orchestrator-agent.md)** | Coordinates workflow | Customer message | Final response |
| **[Ticket Classifier](./agents/ticket-classifier-agent.md)** | Categorizes issues | Raw customer input | Classification JSON |
| **[Knowledge Base](./agents/knowledge-base-agent.md)** | Finds solutions | Classification + message | Solution steps |
| **[Response Generator](./agents/response-generator-agent.md)** | Creates responses | All previous data | Customer email |

## 🧪 Testing

### Individual Agent Tests
- **[Ticket Classifier Tests](./tests/individual-agent-tests.md#ticket-classifier-tests)** - 4 classification scenarios
- **[Knowledge Base Tests](./tests/individual-agent-tests.md#knowledge-base-agent-tests)** - 4 solution finding tests  
- **[Response Generator Tests](./tests/individual-agent-tests.md#response-generator-tests)** - 4 response creation tests

### End-to-End Scenarios
- **[Orchestrator Scenarios](./tests/orchestrator-scenarios.md)** - 4 complete customer support workflows

## 📊 Success Metrics

- **Classification Accuracy**: >90% correct category assignment
- **Solution Relevance**: >85% appropriate KB article selection  
- **Response Quality**: Professional tone matching customer sentiment
- **Processing Time**: <2 minutes average end-to-end
- **Escalation Accuracy**: Proper routing of complex issues

## 🛠️ Technology Stack

- **Platform**: Azure AI Foundry
- **Models**: GPT-4o, GPT-4o mini
- **Vector Search**: Azure AI Search
- **Knowledge Base**: JSON-based articles with semantic search
- **Orchestration**: Connected agents with function calling

## 💡 Use Cases

- **Customer Support Automation**: 24/7 intelligent ticket handling
- **Multi-Language Support**: Extensible to global customer bases
- **Enterprise Integration**: API-first architecture for existing systems
- **Quality Assurance**: Consistent response quality and compliance

## 🤝 Contributing

1. Fork the repository
2. Test new scenarios using provided test frameworks
3. Add new knowledge base articles in [`/data/`](./data/)
4. Submit pull requests with improvements
5. Update documentation and test cases

## 📋 Prerequisites

- Azure subscription with AI Foundry access
- Basic understanding of multi-agent systems
- Customer support domain knowledge (helpful)

## 📄 License

MIT License - See [LICENSE](./LICENSE) for details

---
