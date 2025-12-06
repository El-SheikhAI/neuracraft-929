# NeuraCraft Installation Guide

## Prerequisites
Before installing NeuraCraft, ensure your system meets these requirements:
- Python 3.10 or higher
- PIP (Python Package Installer) version 21.0 or newer
- Recommended: Virtual environment tool (venv, conda, or poetry)

## Installation Methods

### 1. Install via PIP (Recommended)
```bash
python -m pip install neuracraft
```

### 2. Install from Source
For developers wanting to contribute or access the latest features:
```bash
git clone https://github.com/neuralabs/neuracraft.git
cd neuracraft
python -m pip install -e .
```

## Initial Setup
After installation, verify your installation with:
```bash
neuracraft --version
```

Create a configuration file (optional for basic usage):
```bash
neuracraft init-config
```

## Quick Start
Run a sample pipeline to test your installation:
```bash
neuracraft run --plugin-sample "Hello, NeuraCraft!"
```

## Troubleshooting
Common issues and solutions:
- **"Command Not Found" Error**: Ensure Python scripts directory is in your system PATH
- **Missing Dependencies**: Run `pip install --upgrade neuracraft`
- **Version Conflicts**: Use a virtual environment
- **Plugin Loading Issues**: Check your `config.yaml` file structure

For detailed logging, run with debug flag:
```bash
neuracraft run --verbose
```

## System Configuration
While NeuraCraft works out-of-the-box, you can configure:
- Execution environment variables (.env files supported)
- Custom plugin directories
- GPU acceleration settings

Refer to [Configuration Documentation](configuration.md) for advanced setup.

## Support
Need help? Check our:
- [Community Forum](https://forum.neuralabs.ai)
- Issue Tracker: github.com/neuralabs/neuracraft/issues
- Support Email: support@neuralabs.ai

First time with Python environments? See our [Beginner's Guide](https://docs.neuralabs.ai/getting-started).