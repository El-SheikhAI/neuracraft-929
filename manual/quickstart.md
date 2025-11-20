# NeuraCraft Quickstart Guide  
  
Welcome to NeuraCraft! This guide will help you create your first AI automation pipeline in **under 5 minutes**.  
  
---  
  
## 🔧 Installation  
1. **Prerequisites**  
   - Python 3.8+  
   - `pip` package manager  

2. **Install NeuraCraft**  
   ```bash  
   pip install neurcraft  
   ```  

---  
  
## ⚙️ Configure Your First Pipeline  
Create `config.yaml`:  

```yaml  
# Base configuration  
core:  
  logging_level: INFO  
  plugins:  
    - "builtin.extractors"  
    - "builtin.processors"  

# Define your pipeline  
pipeline:  
  name: "Sentiment Analysis Demo"  
  steps:  
    - type: "text_input"  
      params:  
        source: "User provided text"  
    - type: "sentiment_analyzer"  
      params:  
        model: "default"  
    - type: "json_output"  
```  

---  
  
## 🚀 Run Your Pipeline  
Execute via CLI:  

```bash  
neurcraft run --config config.yaml --input "NeuraCraft makes AI automation delightful!"  
```  

**Expected Output**:  
```json  
{  
  "input": "NeuraCraft makes AI automation delightful!",  
  "sentiment": {  
    "label": "POSITIVE",  
    "score": 0.96  
  }  
}  
```  

---  

## 🧩 Extend with Plugins  
Install community plugins:  

```bash  
neurcraft plugin install https://github.com/neurcraft/plugin-webscraper  
```  

Use the plugin in your `config.yaml`:  

```yaml  
pipeline:  
  steps:  
    - type: "web_scraper"  
      params:  
        url: "https://example.com"  
```  

---  

## ➡️ Next Steps  
1. Explore our [pre-built plugins](https://docs.neurcraft.io/plugins)  
2. Learn to [create custom plugins](https://docs.neurcraft.io/advanced/plugin_development)  
3. Configure [cloud deployment](https://docs.neurcraft.io/deployment)  

---  

## FAQ  
**Q:** Missing plugin dependencies?  
**A:** Run `neurcraft dependencies install --all`  

**Q:** How to debug pipelines?  
**A:** Use `--debug` flag:  
```bash  
neurcraft run --config config.yaml --debug  
```  

---  

✨ **Tip:** Join our [community forum](https://forum.neurcraft.io) for real-time support!  

Need help? Email support@neurcraft.io or read the [full documentation](https://docs.neurcraft.io).