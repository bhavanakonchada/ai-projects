# Individual Agent Tests

This file contains test scenarios for each agent to validate their individual functionality before orchestration testing.

## Ticket Classifier Agent Tests

### Test 1: High Urgency Account Access
**Scenario:** Premium customer with urgent business deadline

**Input:**
```
"I can't log into my premium account and have a client presentation in 1 hour! Need immediate help!"
```

**Expected Output:**
```json
{
  "category": "Account Access",
  "urgency": "High",
  "department": "Account Management",
  "reasoning": "Login issue with urgent business deadline for premium customer"
}
```

**Validation Criteria:**
- ✅ Correctly identifies Account Access category
- ✅ Recognizes High urgency from timeline
- ✅ Routes to Account Management
- ✅ Reasoning mentions both urgency and customer type

---

### Test 2: Critical System Issue
**Scenario:** System outage affecting business operations

**Input:**
```
"URGENT: Our entire system is down and we're losing revenue! Multiple customers complaining and our production environment is completely inaccessible!"
```

**Expected Output:**
```json
{
  "category": "Technical Issue", 
  "urgency": "Critical",
  "department": "IT Support",
  "reasoning": "System outage affecting business operations and revenue"
}
```

**Validation Criteria:**
- ✅ Identifies Technical Issue category
- ✅ Recognizes Critical urgency from system-wide impact
- ✅ Routes to IT Support for technical resolution
- ✅ Reasoning captures business impact

---

### Test 3: Low Priority Product Question
**Scenario:** General information request with no urgency

**Input:**
```
"Hi, I'm curious about the difference between Basic and Premium plans. What extra features do I get with Premium? No rush on this."
```

**Expected Output:**
```json
{
  "category": "Product Inquiry",
  "urgency": "Low", 
  "department": "Sales",
  "reasoning": "General product information request with no time constraints"
}
```

**Validation Criteria:**
- ✅ Identifies Product Inquiry category
- ✅ Recognizes Low urgency from "no rush" indicator
- ✅ Routes to Sales for upselling opportunity
- ✅ Reasoning notes lack of urgency

---

### Test 4: Billing Dispute
**Scenario:** Payment issue requiring investigation

**Input:**
```
"I was charged twice this month for my subscription - $29 on Sept 5th and another $29 on Sept 12th. Can you please refund the duplicate charge and fix this billing error?"
```

**Expected Output:**
```json
{
  "category": "Billing",
  "urgency": "Medium",
  "department": "Billing", 
  "reasoning": "Duplicate charge requiring refund processing and billing investigation"
}
```

**Validation Criteria:**
- ✅ Identifies Billing category
- ✅ Assigns Medium urgency (not critical but needs resolution)
- ✅ Routes to Billing department
- ✅ Reasoning mentions both refund and investigation needs

---

## Knowledge Base Agent Tests

### Test 1: Account Recovery Solution
**Scenario:** Standard login issue with high-confidence solution

**Input:**
```json
{
  "category": "Account Access",
  "urgency": "High",
  "customer_message": "Can't log into my account, forgot my password"
}
```

**Expected Output:**
```json
{
  "solution_found": true,
  "primary_solution": {
    "title": "Password Reset and Account Recovery",
    "steps": [
      "Go to login page at app.company.com/login",
      "Click 'Forgot Password' link",
      "Enter your registered email address", 
      "Check email for reset instructions",
      "Create new password following security requirements"
    ],
    "kb_article": "KB001"
  },
  "confidence": "High",
  "escalation_needed": false,
  "reasoning": "Standard password reset process resolves most login issues with high success rate"
}
```

**Validation Criteria:**
- ✅ Finds correct KB001 article
- ✅ Returns step-by-step solution
- ✅ High confidence for standard procedure
- ✅ No escalation needed for routine issue

---

### Test 2: Technical Troubleshooting
**Scenario:** Mobile app issue with clear solution path

**Input:**
```json
{
  "category": "Technical Issue",
  "urgency": "Medium", 
  "customer_message": "Mobile app keeps crashing when I try to upload files on iOS"
}
```

**Expected Output:**
```json
{
  "solution_found": true,
  "primary_solution": {
    "title": "Mobile App Crashing Issues",
    "steps": [
      "Force close the app completely",
      "Restart your mobile device",
      "Update app to latest version from App Store",
      "Clear app cache by offloading and reinstalling",
      "Ensure 1GB+ free storage space available"
    ],
    "kb_article": "KB002"
  },
  "confidence": "High",
  "escalation_needed": false,
  "reasoning": "Mobile app troubleshooting steps resolve most crash issues, especially upload-related problems"
}
```

**Validation Criteria:**
- ✅ Identifies KB002 mobile troubleshooting
- ✅ Provides systematic troubleshooting steps
- ✅ High confidence for common technical issue
- ✅ No escalation for standard troubleshooting

---

### Test 3: Complex Technical Issue Requiring Escalation
**Scenario:** API integration problem beyond standard KB scope

**Input:**
```json
{
  "category": "Technical Issue",
  "urgency": "High",
  "customer_message": "Our enterprise API integration is failing with 500 errors across all endpoints since system maintenance"
}
```

**Expected Output:**
```json
{
  "solution_found": true,
  "primary_solution": {
    "title": "API Integration and Developer Support", 
    "steps": [
      "Verify API key validity and format",
      "Check system status at status.company.com",
      "Review recent system maintenance notifications",
      "Contact developer support for enterprise API issues"
    ],
    "kb_article": "KB007"
  },
  "confidence": "Medium",
  "escalation_needed": true,
  "reasoning": "Enterprise API issues post-maintenance likely require system-level investigation beyond standard troubleshooting"
}
```

**Validation Criteria:**
- ✅ Finds relevant KB007 developer support article
- ✅ Provides initial troubleshooting steps
- ✅ Medium confidence due to complexity
- ✅ Correctly identifies need for escalation

---

### Test 4: Billing Policy Information
**Scenario:** Refund request with clear policy guidance

**Input:**
```json
{
  "category": "Billing",
  "urgency": "Medium",
  "customer_message": "I want to cancel my subscription and get a refund. What's your refund policy?"
}
```

**Expected Output:**
```json
{
  "solution_found": true,
  "primary_solution": {
    "title": "Billing and Payment Issues",
    "steps": [
      "Review 30-day money-back guarantee policy",
      "Contact billing@company.com with cancellation request", 
      "Provide account email and subscription details",
      "Refund processed in 3-5 business days if eligible"
    ],
    "kb_article": "KB003"
  },
  "confidence": "High", 
  "escalation_needed": false,
  "reasoning": "Standard refund policy clearly outlined in billing KB with straightforward process"
}
```

**Validation Criteria:**
- ✅ Locates KB003 billing policy
- ✅ Provides clear policy information
- ✅ High confidence for policy questions
- ✅ No escalation for standard policy inquiry

---

## Response Generator Agent Tests

### Test 1: Urgent Account Issue
**Scenario:** High urgency login problem with clear solution

**Input:**
```json
{
  "customer_name": "John Smith",
  "classification": {"category": "Account Access", "urgency": "High"},
  "kb_solution": {
    "title": "Password Reset Process",
    "steps": ["Go to login page", "Click 'Forgot Password'", "Check email for reset link"],
    "confidence": "High"
  },
  "original_message": "Can't log into account, urgent presentation tomorrow"
}
```

**Expected Output:**
```json
{
  "customer_response": "Hi John,\n\nI understand you're having trouble logging into your account and this is urgent with your presentation tomorrow. I can help you resolve this immediately.\n\nPlease follow these steps right now:\n1. Go to our login page at app.company.com/login\n2. Click 'Forgot Password' below the login form\n3. Check your email (including spam folder) for the reset link\n4. Create a new password following the security requirements\n\nThis should restore access within 5 minutes. If you don't receive the reset email within 10 minutes, please reply immediately and I'll escalate this for urgent handling.\n\nBest regards,\nAccount Management Team",
  "escalation_needed": false,
  "next_actions": ["Customer follows password reset steps", "Monitor for follow-up within 15 minutes"],
  "estimated_resolution": "5 minutes"
}
```

**Validation Criteria:**
- ✅ Uses customer name (John)
- ✅ Acknowledges urgency and deadline
- ✅ Provides clear step-by-step solution
- ✅ Sets realistic timeline expectation
- ✅ Offers escalation path if needed

---

### Test 2: Technical Problem Requiring Escalation
**Scenario:** Critical system issue needing immediate technical intervention

**Input:**
```json
{
  "customer_name": "Sarah Johnson", 
  "classification": {"category": "Technical Issue", "urgency": "Critical"},
  "kb_solution": {
    "title": "System Outage Emergency Procedures",
    "steps": ["Escalate to Level 3 support", "Create priority incident"],
    "confidence": "Medium",
    "escalation_needed": true
  },
  "original_message": "Production system completely down, losing $5K per hour"
}
```

**Expected Output:**
```json
{
  "customer_response": "Hi Sarah,\n\nI sincerely apologize for this critical system outage affecting your production environment and causing significant business impact.\n\nIMMEDIATE ACTIONS TAKEN:\n• Escalated to Level 3 Technical Support (Priority Ticket #CRIT-2024-156)\n• Notified on-call engineering team about $5K/hour business impact\n• Created emergency incident with highest priority status\n\nNEXT STEPS:\n• Senior technical engineer will contact you within 15 minutes\n• Real-time updates via email every 30 minutes until resolved\n• Direct emergency line: 1-800-EMERGENCY for immediate updates\n\nWe're treating this with maximum urgency and will provide continuous updates until full resolution.\n\nEmergency Contact: emergency@company.com\nIncident Manager: [On-call engineer will be assigned]\n\nWe will resolve this as quickly as possible.",
  "escalation_needed": true,
  "escalation_department": "IT Support",
  "next_actions": ["Level 3 engineer contacts customer within 15 min", "30-minute status updates", "Continuous monitoring"],
  "estimated_resolution": "Immediate escalation - technical team engaged"
}
```

**Validation Criteria:**
- ✅ Apologetic tone appropriate for critical issue
- ✅ Acknowledges business impact ($5K/hour)
- ✅ Details immediate escalation actions taken
- ✅ Provides multiple contact methods
- ✅ Sets clear expectations for updates

---

### Test 3: Product Inquiry - Sales Opportunity  
**Scenario:** Friendly customer interested in upgrading

**Input:**
```json
{
  "customer_name": "Mike Chen",
  "classification": {"category": "Product Inquiry", "urgency": "Low"},
  "kb_solution": {
    "title": "Product Plans and Upgrade Information", 
    "steps": ["Basic vs Premium comparison", "Upgrade process", "Educational discount available"],
    "confidence": "High"
  },
  "original_message": "Student interested in Premium features, what's included?"
}
```

**Expected Output:**
```json
{
  "customer_response": "Hi Mike,\n\nThank you for your interest in our Premium plan! I'm happy to help you understand what you'd get with an upgrade.\n\nPREMIUM PLAN INCLUDES:\n• Advanced analytics and reporting tools\n• 50GB storage (vs 5GB in Basic)\n• Priority chat support\n• API access for integrations\n• Advanced collaboration features\n\nSTUDENT DISCOUNT:\nGreat news! We offer 50% off Premium for verified students - that's just $14.50/month instead of $29.\n\nUPGRADE PROCESS:\n1. Verify your student status at account.company.com/student\n2. Go to Account Settings > Subscription > Change Plan\n3. Your discount will be applied automatically\n4. Changes take effect immediately\n\nThe Premium features are especially valuable for academic projects and research. Would you like me to help you get started with the student verification process?\n\nFeel free to reply with any other questions!\n\nBest regards,\nSales Team",
  "escalation_needed": false,
  "next_actions": ["Customer considers upgrade options", "Follow up on student verification if interested"],
  "estimated_resolution": "immediate - information provided"
}
```

**Validation Criteria:**
- ✅ Enthusiastic, sales-friendly tone
- ✅ Highlights value proposition clearly
- ✅ Mentions student discount opportunity
- ✅ Provides clear next steps
- ✅ Invites further engagement

---

### Test 4: Billing Issue Resolution
**Scenario:** Straightforward billing problem with clear resolution path

**Input:**
```json
{
  "customer_name": "Lisa Rodriguez",
  "classification": {"category": "Billing", "urgency": "Medium"},
  "kb_solution": {
    "title": "Billing and Payment Issues",
    "steps": ["Refund duplicate charges", "Contact billing team", "3-5 day processing"],
    "confidence": "High"
  },
  "original_message": "Charged twice for subscription this month, need refund"
}
```

**Expected Output:**
```json
{
  "customer_response": "Hi Lisa,\n\nI apologize for the billing error that resulted in the duplicate charge on your account. I can help you resolve this right away.\n\nHERE'S WHAT I'LL DO:\n• I've reviewed your account and confirmed the duplicate charge\n• Initiated a refund request for the extra $29 charge\n• Flagged your account to prevent future duplicate billing\n\nREFUND DETAILS:\n• Processing time: 3-5 business days\n• Refund will appear on the same payment method\n• You'll receive email confirmation once processed\n• Reference number: REF-2024-0912-156\n\nI've also reviewed your billing settings to ensure this doesn't happen again. Your subscription will continue normally with single monthly charges going forward.\n\nIf you don't see the refund within 5 business days, please reply with your reference number and I'll expedite the process.\n\nThank you for bringing this to our attention!\n\nBest regards,\nBilling Support Team",
  "escalation_needed": false,
  "next_actions": ["Process refund request", "Monitor for customer confirmation", "Follow up if refund delayed"],
  "estimated_resolution": "24 hours"
}
```

**Validation Criteria:**
- ✅ Apologizes for billing error
- ✅ Provides specific refund details
- ✅ Includes reference number for tracking
- ✅ Explains preventive measures
- ✅ Sets clear expectations for timeline

---

## Running These Tests

### Test Environment Setup
1. Deploy individual agents with provided system prompts
2. Configure test data inputs
3. Run each test scenario individually
4. Compare outputs against expected results
5. Validate all criteria checkboxes

### Success Metrics
- **Accuracy**: >90% of tests pass all validation criteria
- **Consistency**: Same input produces same output across multiple runs
- **Response Time**: Each agent responds within 5 seconds
- **JSON Format**: All responses follow exact JSON schema

### Troubleshooting Common Issues
- **Wrong Categories**: Review keyword detection in classifier prompts
- **Missing KB Articles**: Verify knowledge base upload and indexing
- **Poor Response Quality**: Adjust response generator temperature and examples
- **Escalation Logic**: Review confidence thresholds and escalation triggers

---

[← Back to Main README](../README.md) | [Next: Orchestrator Scenarios →](./orchestrator-scenarios.md)
