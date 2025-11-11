# Error Handling Guide

NeuraCraft provides robust error handling to keep your AI automation pipelines running smoothly. This guide covers our fault tolerance approach, built-in recovery mechanisms, debugging tools, and best practices for handling failures.

## Our Error Handling Philosophy
NeuraCraft treats errors as first-class citizens in pipeline design with:
- **High Availability**: Automatic recovery from transient failures
- **Observability**: Rich diagnostic information at every step
- **Graceful Degradation**: Fallback mechanisms when components fail
- **Developer Control**: Customizable error handling strategies

## Core Error Handling Layers

### 1. Pipeline-Level Safety Net
```yaml
# neuracraft.yaml
safety:
  max_retries: 3
  circuit_breaker: 
    failure_threshold: 40%
    reset_timeout: 5m
  global_timeout: 1h
```

### 2. Step-Specific Recovery
```yaml
steps:
  - name: image_processing
    plugin: vision/v1
    error_policy:
      retry:
        attempts: 2
        delay: 5s
      fallback_action: skip_step
      critical: false
```

### 3. Plugin-Specific Handlers
```python
# Custom plugin example
def process(input):
    try:
        return transform(input)
    except TransientError as e:
        raise RetryableError(message="Temporary API glitch") from e
    except InvalidInput as e:
        raise SkipStepError(message="Invalid input format") from e
```

## Built-In Recovery Methods

| Method          | When to Use                         | Configuration Example              |
|-----------------|-------------------------------------|------------------------------------|
| Retry           | Transient errors (network, APIs)    | `{retry: {attempts: 3, backoff: exponential}}` |
| Step Skipping   | Non-critical failures               | `{fallback_action: skip_step}`     |
| Pipeline Pause  | Critical system errors              | `{critical: true}`                 |
| Input Repair    | Recoverable data issues             | `{repair_strategies: [trim, retype]}` |
| Fallback Output | Alternate success paths             | `fallback_value: "N/A"`            |

## Debugging Tools

### 1. Log Verbosity Control
```bash
neuracraft run --log-level=DEBUG
```

### 2. Error Context Capture
```python
# In your plugin code
ctx.record_diagnostic(
    snapshot=input_data,
    metrics=processing_stats,
    error_stack=compact_traceback()
)
```

### 3. Common Failure Patterns

1. **Dependency Failure**
```markdown
SOLUTION: Check plugin requirements with `neuracraft deps verify`
```

2. **Resource Exhaustion**
```markdown
SOLUTION: Set resource limits in pipeline config:
resources:
  memory: 2Gi
  gpu: 1
```

3. **Data Validation Issues**
```markdown
USE: neuracraft validate --sample=dataset_sample.json
```

## Plugin Developer Guidelines

### Error Reporting Standards
```python
# Good error practice
raise PluginError(
    code="API_401",
    message="Invalid credentials",
    details={
        "endpoint": "https://api.service.com/v2",
        "docs": "https://neuracraft.dev/plugins/vision#authentication"
    },
    retryable=True
)
```

### Recommended Error Hooks
```python
class MyPlugin(PluginBase):

    def on_failure(self, context):
        # Custom cleanup logic
        self.release_resources()

    def alternative_path(self):
        # Provide fallback implementation
        return simplified_processing()
```

## Best Practices

1. **Tag Retryable Errors**
```yaml
# Mark transient errors clearly
error_policy:
  retry_tags: [network, timeout]
```

2. **Set Timeout Escalation**
```yaml
timeout_chain:
  - step: 30s
  - stage: 2m
  - pipeline: 15m
```

3. **Implement Health Checks**
```python
# Plugin health endpoint
def health_check():
    return {
        "db_connection": ping_database(),
        "model_loaded": model_ready(),
        "api_status": check_service()
    }
```

4. **Monitor Error Trends**
```bash
neuracraft audit --errors --last-7days
```

---

**Next Steps:**
- [Troubleshooting Checklist](../reference/checklist.md)
- [Plugin Error Reference](../plugins/errors.md)
- [Performance Monitoring Guide](performance-monitoring.md)