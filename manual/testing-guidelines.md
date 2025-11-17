# NeuraCraft Testing Guidelines

Ensure robust and reliable automation pipelines by following these testing practices tailored for NeuraCraft's modular architecture.

---

## 🔬 Testing Philosophy

**Test Early, Test Often**:  
Validate plugin and pipeline behavior at every development stage.  
**Isolate Components**:  
Treat plugins as independent units with mocked dependencies.  
**Emulate Production**:  
Test pipelines end-to-end in environments mirroring real-world conditions.

---

## 🧩 Unit Testing

Focus on individual plugin logic and core utilities.

### Core Utilities
Example using pytest:
```python
# tests/test_data_utils.py
from neura_craft.utils import data_cleaner

def test_data_cleaner_removes_special_chars():
    dirty = "Hello|World®!"
    clean = data_cleaner(dirty)
    assert clean == "HelloWorld"
```

### Plugin Validation
```python
# tests/plugins/test_sentiment_analyzer.py
from neura_craft.plugins.sentiment import SentimentAnalyzer

def test_sentiment_positive_verdict():
    analyzer = SentimentAnalyzer()
    result = analyzer.run("This product is amazing!")
    assert result["sentiment"] > 0.7
```

**Best Practices**:
- Mock external API calls
- Validate input/output schemas
- Test edge cases (empty inputs, null values)

---

## 🔗 Integration Testing

Verify plugin interactions and pipeline composition.

### Pipeline Integration
```python
# tests/pipelines/test_news_analysis.py
def test_news_pipeline_flow():
    pipeline = NeuraPipeline.load("config/news_analyzer.yaml")
    test_articles = [...]
    
    with mock.patch('plugins.news_fetcher.NewsAPI') as mock_api:
        mock_api.fetch.return_value = test_articles
        results = pipeline.execute()
        
    assert "sentiment_summary" in results[0]
    assert len(results) == len(test_articles)
```

**Critical Checks**:
- Data format compatibility between plugins
- Error handling across plugin boundaries
- Resource cleanup after pipeline execution

---

## 🚀 End-to-End Testing

Validate complete workflow execution.

### Real-World Scenario Test
```bash
# Run in staging environment
export NEURA_ENV=staging
neura run --pipeline customer_feedback_analyzer \
  --input-path ./test_data/reviews.json \
  --output-path ./results/
```

**Validation Checklist**:
✅ Input ingestion works with production-like data volumes  
✅ All plugins initialize with actual configurations  
✅ Output matches expected schema and quality standards  
✅ System resources stay within limits  

---

## 🛠️ Test Automation

**CI/CD Pipeline Example**:  
```yaml
# .github/workflows/test.yml
jobs:
  neura-tests:
    runs-on: ubuntu-latest
    steps:
      - name: Run unit tests
        run: pytest tests/unit --cov=neura_craft
        
      - name: Integration tests
        run: pytest tests/integration --cov=neura_craft.plugins
        
      - name: E2E smoke test
        run: |
          neura deploy-test-environment
          neura run-smoke-tests
```

**Required Gates**:
- 90%+ code coverage
- Linting (ruff/flake8)
- Pipeline execution time benchmarks
- Security scans (SAST/DAST)

---

## 🐞 Debugging Tips

### Pipeline Tracing
```python
# Enable debug mode
from neura_craft import set_debug

set_debug(True)  # Prints execution path and data snapshots
```

### Common Issues & Fixes:
| Symptom | Likely Cause | Solution |
|---------|--------------|----------|
| Missing output fields | Schema mismatch | Validate plugin I/O contracts |
| Unexpected results | Dirty input data | Add preprocessing step |
| Performance drop | Plugin resource leaks | Check connection pooling/threading |

---

## 🔒 Security Testing

**Essential Verifications**:
1. Input validation tests for all data entry points
2. Permission checks for external service connections
3. Secrets management validation
4. Penetration testing for publicly exposed endpoints

```python
# Test data sanitization
def test_plugin_resists_sql_injection():
    malicious_input = "'; DROP TABLE users;--"
    result = some_plugin.run(malicious_input)
    assert not result.contains("error")
```

---

## 📈 Performance Testing

**Key Metrics to Monitor**:
```markdown
| Metric          | Acceptable Threshold |
|-----------------|----------------------|
| Plugin Latency  | < 300ms per operation |
| Memory Usage    | < 500MB per pipeline |
| Throughput      | > 50 req/sec/core    |
| Cold Start Time | < 1.5 seconds        |
```

Use `neura bench` tool for profiling:
```bash
neura bench --pipeline image_processor --iterations 1000
```

---

## ✅ Final Checklist

Before releasing any component:
1. All public methods have validation tests
2. Error conditions are explicitly tested
3. Cross-plugin compatibility verified
4. Performance baselines recorded
5. Security audit completed
6. Documentation examples validated against test cases

**Happy Testing!** 🧪✨  
Your thoroughness ensures NeuraCraft remains stable at scale.