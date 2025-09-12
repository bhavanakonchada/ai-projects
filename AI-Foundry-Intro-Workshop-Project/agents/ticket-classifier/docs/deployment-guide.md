# Deployment Guide: Ticket Classifier Agent

## Overview
This guide walks you through deploying the Ticket Classifier Agent to Azure AI Foundry and integrating it with an orchestrator system.

## Prerequisites
- Azure AI Foundry subscription and workspace
- Azure OpenAI Service with GPT-4o model access
- Appropriate permissions to create and deploy agents
- Azure CLI or Azure Portal access

## Step 1: Prepare Azure AI Foundry Environment

### 1.1 Create AI Foundry Project
```bash
az cognitiveservices account create \
  --name "ai-foundry-workshop" \
  --resource-group "your-resource-group" \
  --kind "AIServices" \
  --sku "S0" \
  --location "eastus"
```

### 1.2 Set up OpenAI Connection
1. Navigate to Azure AI Foundry portal
2. Go to "Connections" → "Add Connection"  
3. Select "Azure OpenAI"
4. Configure connection with your OpenAI resource
5. Test the connection and ensure GPT-4o model is available

## Step 2: Deploy the Agent

### 2.1 Using Azure AI Foundry Portal

1. **Upload Agent Configuration**
   - Navigate to "Agents" in your AI Foundry project
   - Click "Create Agent" 
   - Upload `config/agent-config.json`
   - Verify the configuration settings

2. **Configure Prompts**
   - In the agent settings, upload the system prompt from `prompts/system-prompt.txt`
   - Add the few-shot examples from `prompts/examples.json`
   - Test the prompt with sample inputs

3. **Test the Agent**
   - Use the "Test" feature with sample tickets from `examples/sample-tickets.json`
   - Verify classification outputs match expected results
   - Adjust confidence thresholds if needed

### 2.2 Using Azure CLI (Alternative)

```bash
# Create the agent deployment
az cognitiveservices account deployment create \
  --name "ai-foundry-workshop" \
  --resource-group "your-resource-group" \
  --deployment-name "ticket-classifier" \
  --model-format "OpenAI" \
  --model-name "gpt-4o" \
  --model-version "2024-02-15-preview"

# Apply the Kubernetes configuration (if using container deployment)
kubectl apply -f config/deployment.yaml
```

## Step 3: Integration with Orchestrator

### 3.1 Configure Orchestrator Connection
The ticket classifier works as part of a multi-agent orchestration. Configure the orchestrator to:

1. **Route incoming tickets** to the classifier agent
2. **Receive classification results** in JSON format
3. **Forward tickets** to appropriate departments based on classification

### 3.2 Sample Orchestrator Code
```python
import requests
import json

class TicketOrchestrator:
    def __init__(self, classifier_endpoint):
        self.classifier_endpoint = classifier_endpoint
    
    def process_ticket(self, customer_message, customer_id=None):
        # Send to classifier
        payload = {
            "message": customer_message,
            "customer_id": customer_id
        }
        
        response = requests.post(
            f"{self.classifier_endpoint}/classify",
            json=payload
        )
        
        classification = response.json()
        
        # Route based on classification
        return self.route_ticket(classification)
    
    def route_ticket(self, classification):
        department = classification["department"]
        priority = classification["priority"]
        
        # Route to appropriate agent or queue
        if department == "technical_support":
            return self.route_to_technical_support(classification)
        elif department == "billing":
            return self.route_to_billing(classification)
        # ... additional routing logic
```

## Step 4: Monitoring and Maintenance

### 4.1 Set up Monitoring
1. Enable Application Insights for the agent
2. Configure alerts for:
   - High error rates
   - Low confidence scores
   - Unusual classification patterns
   - Response time degradation

### 4.2 Performance Optimization
- Monitor classification accuracy over time
- Adjust prompt engineering based on misclassifications
- Update examples with new ticket types
- Fine-tune confidence thresholds

### 4.3 Regular Maintenance
- Review and update classification categories
- Add new examples for edge cases
- Monitor customer feedback on routing accuracy
- Update department routing rules as organization changes

## Step 5: Testing and Validation

### 5.1 Automated Testing
Run the test suite with sample tickets:
```bash
# Test classification accuracy
python test_classifier.py --input examples/sample-tickets.json --expected examples/classification-results.json
```

### 5.2 Manual Testing
1. Submit test tickets through normal channels
2. Verify correct classification and routing
3. Check response times meet SLA requirements
4. Validate confidence scores are reasonable

## Troubleshooting

### Common Issues
1. **Low Confidence Scores**: Review prompt clarity and add more examples
2. **Incorrect Classifications**: Update system prompt with clearer guidelines
3. **Slow Response Times**: Check model deployment scaling settings
4. **Integration Errors**: Verify API endpoints and authentication

### Debugging Steps
1. Check Azure AI Foundry logs
2. Validate input/output schemas
3. Test with simplified examples
4. Review network connectivity
5. Check model quota and limits

## Support and Resources
- Azure AI Foundry Documentation: [Link to docs]
- OpenAI Model Documentation: [Link to OpenAI docs]  
- Workshop Support: Contact workshop facilitators
- Community Forums: [Link to community]

## Next Steps
After successful deployment:
1. Connect additional specialized agents (billing, technical support, etc.)
2. Implement feedback loops for continuous improvement
3. Scale the system based on ticket volume
4. Add advanced features like sentiment analysis
5. Integrate with existing customer service platforms