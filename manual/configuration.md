# Configuration Guide

NeuraCraft uses a simple YAML configuration format to define automation pipelines. All configuration files should be placed in your project's `/config` directory.

## Configuration Structure

The base configuration file (`config/base.yml`) defines global settings and pipeline definitions:

```yaml
# Required NeuraCraft version
neura_version: "1.0+"

# Core system settings
system:
  log_level: INFO  # DEBUG, INFO, WARNING, ERROR
  log_format: json # text or json
  max_workers: 4   # Max parallel workers
  timezone: UTC    # System timezone

# Environment variables (.env file reference)
environment:
  - API_KEY
  - DB_CONNECTION_STRING

# Pipeline definitions
pipelines:
  my_data_pipeline:
    description: "Processes incoming data streams"
    steps:
      - plugin: data_loader
        config: data_loader.yml
      - plugin: ai_processor
        config: ai_model.yml
      - plugin: output_handler
        config: s3_upload.yml
```

## Plugin Configuration

Each plugin requires its own configuration file. Create these in `/config/plugins`:

```yaml
# Example: config/plugins/data_loader.yml
name: csv_loader
version: 1.2

parameters:
  input_path: "/data/raw"
  file_pattern: "*.csv"
  chunk_size: 1000
  fail_on_empty: true

credentials:
  username: ${DB_USER}  # Reference .env variables
  password: ${DB_PASS}
```

## Environment Variables

1. Create `.env` in your project root:
```bash
# .env example
API_KEY=your_api_key_here
DB_USER=admin
DB_PASS=secure_password
```

2. Reference variables in configurations:
```yaml
api_endpoint: "https://api.example.com?key=${API_KEY}"
```

## Logging Configuration

Customize logging in `config/logging.yml`:
```yaml
version: 1
disable_existing_loggers: false

formatters:
  simple:
    format: "%(asctime)s - %(name)s - %(levelname)s - %(message)s"

handlers:
  console:
    class: logging.StreamHandler
    formatter: simple
  file:
    class: logging.handlers.RotatingFileHandler
    filename: neura.log
    maxBytes: 10485760 # 10MB
    backupCount: 3

root:
  level: INFO
  handlers: [console, file]
```

## Common Configuration Patterns

### Minimal Configuration
```yaml
neura_version: "1.0+"
system:
  log_level: INFO

pipelines:
  basic_flow:
    steps:
      - plugin: http_fetcher
        config: plugins/fetcher.yml
```

### Advanced Configuration
```yaml
neura_version: "1.0+"
system:
  log_level: DEBUG
  max_workers: 8
  cache_ttl: 3600

environment:
  - CLOUD_API_SECRET
  - MONITORING_ENDPOINT

pipelines:
  ai_processing_pipeline:
    schedule: "0 * * * *"  # Hourly cron schedule
    steps:
      - plugin: data_ingestor
        config: plugins/ingestor.yml
      - plugin: gpt_processor
        config: plugins/gpt4_model.yml
        timeout: 300  # 5 minute timeout
      - plugin: quality_check
        config: plugins/quality.yml
      - plugin: results_exporter
        config: plugins/bigquery.yml
```

## Validation

Check your configuration before running:
```bash
neura validate --config path/to/config.yml
```