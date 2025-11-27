# NeuraCraft Development Setup

Welcome to NeuraCraft! This guide will help you set up your development environment to start building and extending AI automation pipelines. 

## Prerequisites

Before you begin, ensure you have these installed on your system:
- **Python 3.9+** (recommended: 3.11 using [pyenv](https://github.com/pyenv/pyenv))
- **Git** (latest stable version)
- **pip** (Python package manager, included with Python 3.4+)
- **Docker** (optional, required for containerized plugin testing)

Verify your Python installation:
```bash
python3 --version
pip --version
```

## Getting Started

### 1. Clone the Repository
Clone the NeuraCraft core repository and navigate to the project directory:
```bash
git clone https://github.com/neura-craft/core.git
cd core
```

### 2. Create a Virtual Environment
We recommend using a virtual environment to isolate dependencies:
```bash
python3 -m venv .venv
```

Activate the environment:
- **Linux/macOS**:
  ```bash
  source .venv/bin/activate
  ```
- **Windows** (PowerShell):
  ```powershell
  .\.venv\Scripts\Activate.ps1
  ```

### 3. Install Dependencies
Install development requirements and core packages:
```bash
pip install -r requirements-dev.txt
pip install -e .
```

### 4. Configure Environment Variables
Create a `.env` file in the project root:
```bash
cp .env.example .env
```

Edit the file with your preferred text editor. Default values are suitable for local development:
```ini
NEURACRAFT_LOG_LEVEL=DEBUG
NEURACRAFT_PLUGIN_DIR=./plugins
```

### 5. Verify Installation
Confirm everything works by running the test suite:
```bash
pytest tests/ --cov=neurocraft
```

You should see:
```
=========================== test session starts ============================
...
10 passed in 1.25s
```

## First Run

Start a sample pipeline:
```bash
neurocraft run examples/hello_world.yaml
```

Expected output:
```
[INFO] Pipeline initialized
[DEBUG] Loading plugin: text_generator @ v0.1.0
[SUCCESS] Pipeline completed in 0.8s
```

## Advanced Setup

### Plugin Development
1. Create a new plugin template:
   ```bash
   neurocraft plugin create my_plugin --template=basic
   ```
2. Navigate to your plugin directory:
   ```bash
   cd plugins/my_plugin
   ```
3. Install plugin in development mode:
   ```bash
   pip install -e .
   ```

### Docker Setup (Optional)
Build the development container:
```bash
docker build -t neura-craft-dev -f Dockerfile.dev .
```

Run tests inside the container:
```bash
docker run -v $(pwd):/app neura-craft-dev pytest
```

## Troubleshooting

Common issues and solutions:
- **ModuleNotFoundError**: Reactivate your virtual environment and reinstall dependencies
- **Plugin loading issues**: Check your `NEURACRAFT_PLUGIN_DIR` path in `.env`
- **Docker permission errors**: Prepend commands with `sudo` or [configure Docker permissions](https://docs.docker.com/engine/install/linux-postinstall/)

## Next Steps

You're ready to develop! Explore these resources:
- [Plugin Development Guide](./plugin-development.md)
- [Core Architecture Overview](./architecture.md)
- [Contributing Guidelines](../CONTRIBUTING.md)

Happy building! 🚀 For support, join our [Developer Community Forum](https://forum.neura-craft.dev).