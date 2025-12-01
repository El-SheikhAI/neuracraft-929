# NeuraCraft CLI Command Reference

## Overview
The NeuraCraft Command Line Interface (CLI) helps you create, manage, and execute AI automation pipelines. This guide covers all available commands and their usage.

---

## Core Commands

### `neura init`
Creates a new NeuraCraft project structure.

**Syntax:**
```bash
neura init [PROJECT_NAME] [OPTIONS]
```

**Options:**
| Flag | Description |
|------|-------------|
| `-t, --template` | Specify starter template (default: "basic") |
| `-f, --force` | Overwrite existing directory |

**Example:**
```bash
neura init my_automation_project -t image_processing
```

---

### `neura run`
Executes an automation pipeline.

**Syntax:**
```bash
neura run PIPELINE_NAME [OPTIONS]
```

**Options:**
| Flag | Description |
|------|-------------|
| `-v, --verbose` | Show detailed execution logs |
| `-d, --dry-run` | Validate pipeline without execution |
| `-c, --config` | Path to custom config file |

**Example:**
```bash
neura run text_processing -v -c ./custom_config.yaml
```

---

## Plugin Management

### `neura plugin add`
Installs a plugin package.

**Syntax:**
```bash
neura plugin add PLUGIN_NAME[@VERSION]
```

**Example:**
```bash
neura plugin add ocr-processor@1.2.0
```

---

### `neura plugin remove`
Uninstalls a plugin.

**Syntax:**
```bash
neura plugin remove PLUGIN_NAME
```

**Example:**
```bash
neura plugin remove legacy-image-tools
```

> **Note:** The `--force` flag bypasses dependency checks (use with caution)

---

### `neura plugin list`
Displays installed plugins.

**Syntax:**
```bash
neura plugin list [--verbose]
```

**Example Output:**
```
├── text-analyzer (2.1.0)
├── image-processor (1.4.3)
└── data-exporter (0.9.5)
```

---

## Configuration Management

### `neura config set`
Configures runtime parameters.

**Syntax:**
```bash
neura config set KEY VALUE
```

**Example:**
```bash
neura config set default_engine "gpt-4-turbo"
```

---

### `neura config get`
Retrieves configuration values.

**Syntax:**
```bash
neura config get [KEY]
```

**Example:**
```bash
neura config get api_timeout
```

---

## Advanced Commands

### `neura debug`
Starts interactive debugging session.

**Syntax:**
```bash
neura debug PIPELINE_FILE
```

**Features:**
- Step-through execution
- Variable inspection
- Real-time plugin testing

---

### `neura benchmark`
Tests pipeline performance.

**Syntax:**
```bash
neura benchmark PIPELINE_NAME [--samples NUMBER]
```

**Example Output:**
```
► text_processing (5 samples)
├── Average execution: 4.2s
├── Peak memory: 812MB
└── Success rate: 100%
```

---

## Getting Help

### `neura --help`
Displays general help or command-specific help.

**Examples:**
```bash
neura --help
neura run --help
neura plugin --help
```

### `neura version`
Shows CLI version and core dependencies.

```bash
neura version --full
```

**Example Output:**
```
NeuraCraft CLI 2.3.1
Core Engine: 3.0.0
Plugin API: v4
```