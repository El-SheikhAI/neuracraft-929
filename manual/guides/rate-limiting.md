# Rate Limiting in NeuraCraft Pipelines

## Why Rate Limiting Matters
When building AI-driven automation pipelines, you’ll often connect to external APIs, databases, or services with strict usage policies. Rate limiting ensures your workflows:
- Avoid overloading downstream systems
- Respect third-party API quotas
- Maintain predictable throughput
- Prevent unintentional resource exhaustion

NeuraCraft implements rate limiting at two levels:
1. **Pipeline Execution** (global throttling)
2. **Plugin Operations** (component-specific controls)

---

## Declarative Rate Limit Configuration
Define limits directly in your pipeline YAML using the `rate_limit` node:

```yaml
pipeline:
  name: social_media_analyzer
  rate_limit:
    strategy: token_bucket
    bucket_size: 20
    refill_rate: 5/60s  # 5 tokens per minute

  plugins:
    - twitter_scraper:
        rate_limit:
          strategy: fixed_window
          max_requests: 300
          interval: 15m
```

### Supported Strategies
| Strategy       | Parameters               | Use Case                     |
|----------------|--------------------------|------------------------------|
| `fixed_window` | max_requests, interval   | Simple API quotas (e.g., 1000 req/hour) |
| `rolling_window` | max_requests, precision | High-precision burst control |
| `token_bucket` | bucket_size, refill_rate | Smooth, burstable throughput |
| `concurrency`  | max_concurrent           | Parallel execution limits |

---

## Dynamic Rate Adjustment
Use runtime variables for adaptive limiting:

```yaml
rate_limit:
  strategy: fixed_window
  max_requests: ${env.API_MAX_REQ}  # Pull from environment
  interval: ${vars.rate_interval}   # Reference pipeline variable
```

---

## Custom Rate Limiter Plugins
Extend NeuraCraft's rate limiting by building your own algorithm:

1. Create a Python class inheriting from `RateLimitBase`
```python
from neura_craft.core.rate_limiting import RateLimitBase

class AdaptiveRateLimiter(RateLimitBase):
    def __init__(self, min_rate, max_rate, sensitivity=0.8):
        self.min_rate = min_rate
        self.max_rate = max_rate
        self.sensitivity = sensitivity

    async def acquire(self):
        # Implement your custom backpressure logic here
        current_load = get_system_load()
        adjusted_rate = self.max_rate * (1 - current_load * self.sensitivity)
        effective_rate = max(adjusted_rate, self.min_rate)
        await self.dynamic_delay(effective_rate)
```

2. Register it in your `plugin_manifest.yaml`
```yaml
rate_limiters:
  adaptive:
    class: path.to.AdaptiveRateLimiter
    config_schema:
      min_rate: int
      max_rate: int
      sensitivity: {type: float, default: 0.7}
```

3. Use it in pipelines
```yaml
rate_limit:
  strategy: adaptive
  min_rate: 10
  max_rate: 100
  sensitivity: 0.85
```

---

## Handling Rate-Limited Requests
Automatically manage throttled operations with the `retry` policy:

```yaml
plugins:
  - api_connector:
      endpoint: https://api.example.com/v1/analyze
      retry:
        strategy: exponential_backoff
        max_attempts: 5
        base_delay: 500ms
        status_codes: [429, 503]
```

### Recommended Retry Strategies
- **Immediate**: Quick retries for transient errors
- **Fixed Interval**: Consistent delay between attempts
- **Exponential Backoff**: Increasing delays to reduce pressure
- **Jitter**: Randomization to prevent thundering herds

---

## Monitoring & Metrics
Track rate limiting behavior through NeuraCraft's telemetry system:

```bash
# View current rate utilization
neuractl metrics query 'rate_limit_utilization{plugin="twitter_scraper}"'

# Alert when thresholds breached
neuractl alerts create \
  --name "Twitter API Limit" \
  --condition 'rate_limit_remaining < 50' \
  --severity warning
```

Key metrics exposed:
- `rate_limit_attempts_total`
- `rate_limit_successful`
- `rate_limit_throttled`
- `rate_limit_remaining`

---

## Best Practices
1. **Test in Dry-Run Mode**: 
   ```bash
   neuractl pipeline test --dry-run --rate-limit-simulation
   ```
2. **Layer Your Limits**:
   - Global pipeline limits
   - Plugin-level quotas
   - Individual API call restrictions
3. **Implement Circuit Breakers**:
   ```yaml
   rate_limit:
     strategy: fixed_window
     max_requests: 100
     circuit_breaker:
       failure_threshold: 50%
       reset_timeout: 5m
   ```
4. **Use Graceful Degradation**:
   ```python
   async def analyze_text(text):
       try:
           return await nlp_plugin.process(text)
       except RateLimitExceeded:
           return fallback_heuristic_analysis(text)
   ```

---

## Troubleshooting Common Issues
**Error: `429 Too Many Requests`**
- Verify pipeline-level vs plugin-level limits aren't conflicting
- Check for overlapping rate limit scopes
- Review retry strategy configuration

**Error: `RateLimitNotConfigured`**
- Ensure at least one rate limit strategy is defined
- Confirm YAML indentation for rate_limit nodes is correct
- Check plugin compatibility with selected strategy

**Symptom: Uneven throttling across parallel nodes**
- Add granularity to rate limit scopes
- Consider switching from `fixed_window` to `token_bucket` strategy
- Adjust concurrency settings in `execution` node

For advanced scenarios, see our [Plugin Development Guide](../developer/plugins.md).