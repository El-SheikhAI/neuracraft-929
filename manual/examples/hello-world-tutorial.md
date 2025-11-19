# NeuraCraft: Hello World Tutorial

Welcome to your first NeuraCraft pipeline! This 5-minute tutorial will walk you through creating a basic automation that generates friendly greetings using AI.

## Before You Start

### Prerequisites
- Python 3.8+ installed
- NeuraCraft installed (`pip install neuracraft`)
- A text editor or IDE
- Terminal/command line access

## Step-by-Step Guide

### 1. Create Project Directory
```bash
mkdir hello-neuracraft
cd hello-neuracraft
```

### 2. Create Pipeline File
Create `greeting_pipeline.yaml` with the following content:

```yaml
# greeting_pipeline.yaml
nodes:
  name_input:
    type: input
    description: "Your name"

  greeting_generator:
    type: llm-processor
    model: gpt-3.5-turbo
    prompt: |
      Create a friendly greeting for someone named {{ name_input.value }}
      Begin with "Hello" and include an enthusiastic compliment.

  output:
    type: output
    format: text
    value: "{{ greeting_generator.result }}"
```

### 3. Run Your Pipeline
Execute this command in your terminal:
```bash
nc run greeting_pipeline.yaml --data '{"name_input": "Alex"}'
```

### 4. View Output
You should see output similar to:
```text
Hello Alex! Your energy and creativity always light up the room - 
today is lucky to have you!
```

## Understanding Your Pipeline

Let's break down what we built:

1. **Input Node** (`name_input`)
   - Collects user-provided data (your name)
   - Serves as the pipeline's entry point

2. **LLM Processor** (`greeting_generator`)
   - Uses GPT-3.5-turbo model
   - Dynamically inserts input using `{{ name_input.value }}`
   - Generates personalized output

3. **Output Node** (`output`)
   - Formats final result as plain text
   - Displays the LLM-generated greeting

## Next Steps

Now that you've created your first pipeline:
- Try changing the prompt template
- Experiment with different input names
- Explore other processor types in our documentation
- Learn how to create custom plugins 

Welcome to the world of AI automation with NeuraCraft! 🚀