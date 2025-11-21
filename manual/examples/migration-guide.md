# NeuraCraft Migration Guide

Welcome to the NeuraCraft Migration Guide! This document helps you transition between major versions of NeuraCraft with minimal disruption to your automation pipelines. 

---

## Overview
### Migration Path Support
- **Current Stable Version**: 3.1
- **Supported Upgrade Paths**:
  - 1.x → 2.0 → 3.0 → 3.1
  - 2.x → 3.0 → 3.1
  - 3.0 → 3.1

### Prerequisites
1. Backup existing pipelines/configurations
2. Install the target NeuraCraft version in a staging environment
3. Review all custom plugins for compatibility

---

## Version-Specific Migration Steps

### Migrating from 2.x → 3.0
#### Breaking Changes
1. **Plugin Manifest Format**  
   Added required `compatibility` field:
   ```yaml
   # Before
   api_version: "2"
   
   # After
   api_version: "3"
   compatibility:
     min_core: "3.0.0"
   ```

2. **Pipeline Configuration**  
   Executor blocks now use `runtime` instead of `engine`:
   ```yaml
   # Before
   executor:
     engine: tensorflow-light
    
   # After
   executor:
     runtime: tf-lite
   ```

3. **Deprecated Features**
   - Removed legacy `neuro-workers` CLI command
   - Dropped Python 3.6 support

#### Required Actions
1. Run compatibility check:
   ```bash
   neura migrate check --from-version=2.3
   ```
2. Update all plugin references in `neuraconfig.yaml`:
   ```yaml
   plugins:
     - github.com/neura-plugins/text_processor:v3
     - github.com/neura-plugins/image_detector:v2
   ```

---

### Migrating from 3.0 → 3.1
#### Key Changes
1. **New Dependency Management**
   ```bash
   # Before
   pip install plugin_dependency

   # After
   neura deps add plugin_dependency==1.2.0
   ```

2. **Improved Log Format**  
   Add this to your pipeline configuration:
   ```yaml
   monitoring:
     log_format: structured
   ```

3. **Enhanced Security**  
   All external connections now require TLS:
   ```yaml
   connections:
     api_endpoint:
       url: https://api.service.com
       verify_cert: true  # New mandatory field
   ```

#### Recommended Steps
1. Update core schemas:
   ```bash
   neura schema update
   ```
2. Test pipelines with dry-run:
   ```bash
   neura run --validate-only pipeline.yaml
   ```

---

## Plugin Migration Checklist
1. **Update Manifest**
   ```yaml
   capabilities:
     - ai/vision:v2  # New versioned capability model
   ```
2. **Use New Context API**
   ```python
   # Before
   from neura.sdk.context import get_config

   # After
   from neura.runtime.context import PipelineConfig
   ```
3. **Testing**  
   Run validation against new core:
   ```bash
   neura plugin test --runtime-version=3.1
   ```

---

## Troubleshooting Common Issues
### Error: "Schema validation failed"
**Solution**:  
Run migration helper:
```bash
neura migrate fix-schemas ./pipelines
```

### Warning: "Deprecated connector protocol"
**Solution**:  
Update connection blocks to use modern protocol:
```yaml
# Before
database:
  type: mysql-legacy

# After
database:
  type: mysql
  protocol: tls
```

### Error: "Missing runtime dependencies"
**Solution**:  
Use the dependency manager:
```bash
neura deps scan --install
```

---

## Final Validation Steps
1. Execute all test pipelines:
   ```bash
   neura test --full-suite
   ```
2. Verify monitoring output:
   ```bash
   neura logs --validation-mode
   ```
3. Check backward compatibility flags:
   ```bash
   neura config --compat-check
   ```

---

Need help? Join our community forum at [community.neuracraft.io](https://community.neuracraft.io) or open a GitHub discussion. Happy migrating! 🚀