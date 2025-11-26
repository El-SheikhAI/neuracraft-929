# NeuraCraft Basic Usage

## **Installation**

Install NeuraCraft via pip:  
```bash
pip install neuracraft
```

For optional plugin dependencies:  
```bash
pip install neuracraft[plugins]
```

## **Configuration**

Create a configuration file (`pipeline.yaml`):  
```yaml
# pipeline.yaml
name: "Document Processor"
description: "Analyze and summarize documents"

steps:
  - name: File Loader
    type: python_script
    path: "./scripts/load_file.py"
    parameters:
      input_path: "./documents"

  - name: AI Analysis
    type: rest_api
    endpoint: "https://api.ai-service.com/v1/analyze"
    parameters:
      model: "gpt-4-turbo"
      temperature: 0.7

  - name: Results Export
    type: builtin
    command: "export_csv"
    parameters:
      output_path: "./reports/results.csv"
```

## **Running Pipelines**

Execute your pipeline via CLI:  
```bash
nc run --config pipeline.yaml
```

View real-time pipeline execution status:  
```bash
nc run --config pipeline.yaml --verbose
```

## **Extending with Plugins**

1. Create a plugin directory structure:  
```plaintext
my_plugins/
└── text_processor/
    ├── __init__.py
    └── manifest.yaml
```

2. Implement plugin logic (`text_processor/__init__.py`):  
```python
def run(params, context):
    """
    Processes input text through custom logic
    params: Dictionary of user-provided parameters
    context: Pipeline execution context
    Returns processed text
    """
    input_text = params.get("text", "")
    # Custom transformation logic here
    return processed_text
```

3. Define plugin metadata (`manifest.yaml`):  
```yaml
name: "Text Processor"
version: "1.0.0"
description: "Custom text manipulation plugin"
parameters:
  text:
    type: string
    required: true
```

4. Reference in your pipeline configuration:  
```yaml
steps:
  - name: Text Processing
    type: plugin
    plugin: "text_processor"
    parameters:
      text: "{{previous_step.output}}"
```

## **Best Practices**

1. **Atomic Steps**  
   Keep individual pipeline steps focused on single responsibilities

2. **Error Handling**  
   Implement retry logic in your configuration:  
   ```yaml
   steps:
     - name: API Call
       type: rest_api
       retry:
         attempts: 3
         delay: 5
   ```

3. **Version Control**  
   Maintain configuration files in version control with schema validation:  
   ```bash
   nc validate --config pipeline.yaml
   ```

4. **Plugin Testing**  
   Test plugins independently before pipeline integration:  
   ```bash
   nc test-plugin --path ./my_plugins/text_processor
   ```

5. **Execution Monitoring**  
   Enable execution tracking:  
   ```bash
   nc run --config pipeline.yaml --log-file execution.log
   ```

For advanced usage examples and plugin development guides, refer to our [Plugin Development Documentation](./plugin-guide.md) and [API Reference](./api-reference.md).