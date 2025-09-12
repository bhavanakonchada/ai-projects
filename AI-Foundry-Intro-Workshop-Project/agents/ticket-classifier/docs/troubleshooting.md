# Troubleshooting Guide: Ticket Classifier Agent

## Common Issues and Solutions

### 1. Classification Accuracy Problems

#### Issue: Low confidence scores across multiple tickets
**Symptoms:**
- Confidence scores consistently below 0.7
- Uncertain classifications for clear-cut tickets
- Agent reports low confidence even for obvious cases

**Possible Causes:**
- Ambiguous prompt instructions
- Insufficient or contradictory examples
- Model temperature set too high

**Solutions:**
1. **Review and clarify system prompt:**
   - Make classification criteria more specific
   - Add clearer decision boundaries
   - Remove conflicting instructions

2. **Improve few-shot examples:**
   - Add more diverse, high-quality examples
   - Ensure examples cover edge cases
   - Remove contradictory or low-quality examples

3. **Adjust model parameters:**
   ```json
   {
     "temperature": 0.1,  // Lower for more deterministic output
     "top_p": 0.8        // Reduce for more focused responses
   }
   ```

#### Issue: Incorrect department routing
**Symptoms:**
- Technical issues routed to billing
- Billing questions sent to sales
- General questions misclassified as critical bugs

**Solutions:**
1. **Update department classification guidelines in system prompt**
2. **Add specific keyword detection logic**
3. **Include more examples for problematic categories**
4. **Review and update department definitions**

### 2. Performance Issues

#### Issue: Slow response times
**Symptoms:**
- Agent takes >5 seconds to respond
- Timeouts in orchestrator integration
- Users experiencing delays

**Possible Causes:**
- Insufficient compute resources
- Large prompt context
- Network latency

**Solutions:**
1. **Scale up deployment:**
   ```yaml
   # In deployment.yaml
   resources:
     requests:
       cpu: "200m"      # Increase from 100m
       memory: "512Mi"  # Increase from 256Mi
   ```

2. **Optimize prompt length:**
   - Reduce verbose instructions
   - Trim unnecessary examples
   - Use more concise language

3. **Enable caching:**
   - Cache common classification patterns
   - Use response caching for similar tickets

#### Issue: High resource usage
**Symptoms:**
- CPU/memory limits exceeded
- Frequent pod restarts
- High costs

**Solutions:**
1. **Optimize model parameters:**
   ```json
   {
     "max_tokens": 500,  // Reduce from 1000
     "temperature": 0.1
   }
   ```

2. **Implement request batching:**
   - Process multiple tickets together
   - Use async processing for non-urgent tickets

### 3. Integration Issues

#### Issue: Orchestrator connection failures
**Symptoms:**
- Failed API calls to classifier
- Authentication errors
- Network timeouts

**Solutions:**
1. **Check API endpoints:**
   ```bash
   # Test connectivity
   curl -X POST https://your-agent-endpoint/classify \
     -H "Content-Type: application/json" \
     -d '{"message": "test ticket"}'
   ```

2. **Verify authentication:**
   - Check API keys and tokens
   - Validate service principal permissions
   - Review network security groups

3. **Implement retry logic:**
   ```python
   import time
   import requests
   from requests.adapters import HTTPAdapter
   from urllib3.util.retry import Retry

   def create_session_with_retries():
       session = requests.Session()
       retry_strategy = Retry(
           total=3,
           status_forcelist=[429, 500, 502, 503, 504],
           method_whitelist=["HEAD", "GET", "POST"]
       )
       adapter = HTTPAdapter(max_retries=retry_strategy)
       session.mount("http://", adapter)
       session.mount("https://", adapter)
       return session
   ```

#### Issue: Schema validation errors
**Symptoms:**
- "Invalid input format" errors
- Missing required fields
- Type mismatch errors

**Solutions:**
1. **Validate input schema:**
   ```python
   import jsonschema

   input_schema = {
       "type": "object",
       "properties": {
           "message": {"type": "string"},
           "customer_id": {"type": "string"}
       },
       "required": ["message"]
   }

   def validate_input(data):
       jsonschema.validate(data, input_schema)
   ```

2. **Handle optional fields gracefully:**
   ```python
   def prepare_input(message, customer_id=None, channel=None):
       data = {"message": message}
       if customer_id:
           data["customer_id"] = customer_id
       if channel:
           data["channel"] = channel
       return data
   ```

### 4. Deployment Issues

#### Issue: Agent fails to start
**Symptoms:**
- Pod in CrashLoopBackOff state
- "Failed to load model" errors
- Container exit codes

**Solutions:**
1. **Check resource limits:**
   ```yaml
   resources:
     limits:
       memory: "1Gi"    # Increase if needed
       cpu: "1000m"
   ```

2. **Verify model availability:**
   - Check Azure OpenAI quota
   - Ensure model deployment is active
   - Validate API versions

3. **Review configuration:**
   ```bash
   # Check agent configuration
   kubectl describe pod ticket-classifier-agent
   kubectl logs ticket-classifier-agent
   ```

#### Issue: Model quota exceeded
**Symptoms:**
- "Quota exceeded" errors
- HTTP 429 responses
- Service unavailable errors

**Solutions:**
1. **Request quota increase:**
   - Contact Azure support
   - Upgrade subscription tier
   - Distribute load across regions

2. **Implement rate limiting:**
   ```python
   import time
   from functools import wraps

   def rate_limit(calls_per_minute=60):
       min_interval = 60.0 / calls_per_minute
       last_called = [0.0]
       
       def decorator(func):
           @wraps(func)
           def wrapper(*args, **kwargs):
               elapsed = time.time() - last_called[0]
               left_to_wait = min_interval - elapsed
               if left_to_wait > 0:
                   time.sleep(left_to_wait)
               ret = func(*args, **kwargs)
               last_called[0] = time.time()
               return ret
           return wrapper
       return decorator
   ```

### 5. Data Quality Issues

#### Issue: Inconsistent classification results
**Symptoms:**
- Same ticket classified differently on retry
- Random variations in output
- Confidence scores fluctuate significantly

**Solutions:**
1. **Set consistent parameters:**
   ```json
   {
     "temperature": 0.0,  // Most deterministic
     "seed": 42          // Use fixed seed
   }
   ```

2. **Add input normalization:**
   ```python
   def normalize_ticket(message):
       # Remove extra whitespace
       message = ' '.join(message.split())
       # Convert to lowercase for consistency
       return message.lower().strip()
   ```

### 6. Monitoring and Alerting

#### Setting up Effective Monitoring
1. **Key Metrics to Track:**
   - Classification accuracy rate
   - Average confidence scores
   - Response times
   - Error rates by category
   - Department distribution

2. **Alert Thresholds:**
   ```yaml
   alerts:
     - name: "Low Confidence Rate"
       condition: "avg_confidence < 0.7 for 5 minutes"
       action: "notify_team"
     
     - name: "High Error Rate" 
       condition: "error_rate > 5% for 2 minutes"
       action: "escalate"
   ```

### 7. Debugging Checklist

When issues occur, follow this systematic approach:

1. **Verify Basic Connectivity:**
   - [ ] Can you reach the agent endpoint?
   - [ ] Is authentication working?
   - [ ] Are there any network issues?

2. **Check Input Data:**
   - [ ] Is the input format correct?
   - [ ] Are required fields present?
   - [ ] Is the message content reasonable?

3. **Review Configuration:**
   - [ ] Are model parameters correct?
   - [ ] Is the system prompt up to date?
   - [ ] Are examples relevant and accurate?

4. **Monitor Resources:**
   - [ ] Is the agent within resource limits?
   - [ ] Are there any quota issues?
   - [ ] Is scaling configured properly?

5. **Test Incrementally:**
   - [ ] Start with simple test cases
   - [ ] Gradually increase complexity
   - [ ] Compare with expected outputs

### Contact Support

If issues persist after following this guide:

1. **Gather Information:**
   - Error messages and logs
   - Input data that caused issues
   - Configuration files
   - Timeline of when issues started

2. **Contact Channels:**
   - Workshop facilitators: [email]
   - Azure Support: [portal link]
   - Community forums: [forum link]

3. **Escalation Path:**
   - Level 1: Self-service (this guide)
   - Level 2: Workshop support team
   - Level 3: Azure AI Foundry support
   - Level 4: Product engineering team