# NeuraCraft API Reference

## Overview
The NeuraCraft API enables programmatic management of automation pipelines, plugins, and execution workflows. All endpoints return JSON responses and require HTTPS.

**Base URL**: `<service-url>/api/v1`

---

## Pipelines Management

### Create Pipeline
**POST** `/pipelines`

Creates a new automation pipeline configuration.

**Request Body**:
```json
{
  "name": "string",
  "description": "string",
  "plugins": [
    {
      "plugin_id": "string",
      "config": {}
    }
  ]
}
```

**Example**:
```bash
curl -X POST <service-url>/api/v1/pipelines \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Image Processing",
    "description": "Resize and tag images",
    "plugins": [
      {"plugin_id": "image-resizer", "config": {"width": 800}},
      {"plugin_id": "object-detector", "config": {"model": "v2.1"}}
    ]
  }'
```

**Response**:
```json
{
  "id": "pipe_abc123",
  "created_at": "2023-11-15T09:30:00Z"
}
```

---

### List Pipelines
**GET** `/pipelines`

Retrieves all pipeline configurations.

**Parameters**:
| Name    | Type     | Description               |
|---------|----------|---------------------------|
| `limit` | `integer`| Maximum results (default 50)|
| `offset`| `integer`| Pagination offset         |

**Example**:
```bash
curl <service-url>/api/v1/pipelines?limit=20
```

**Response**:
```json
{
  "items": [
    {
      "id": "pipe_abc123",
      "name": "Image Processing",
      "description": "Resize and tag images",
      "modified_at": "2023-11-15T09:32:00Z"
    }
  ],
  "total": 1
}
```

---

### Get Pipeline
**GET** `/pipelines/{id}`

Retrieves detailed configuration of a specific pipeline.

**Example**:
```bash
curl <service-url>/api/v1/pipelines/pipe_abc123
```

**Response**:
```json
{
  "id": "pipe_abc123",
  "name": "Image Processing",
  "description": "Resize and tag images",
  "plugins": [
    {
      "plugin_id": "image-resizer",
      "version": "1.2.0",
      "config": {"width": 800}
    }
  ],
  "created_at": "2023-11-15T09:30:00Z",
  "modified_at": "2023-11-15T09:32:00Z"
}
```

---

### Delete Pipeline
**DELETE** `/pipelines/{id}`

Removes a pipeline configuration.

**Example**:
```bash
curl -X DELETE <service-url>/api/v1/pipelines/pipe_abc123
```

**Response**:
```json
{"status": "deleted"}
```

---

## Plugins Management

### List Available Plugins
**GET** `/plugins`

Displays installed plugins available for pipeline integration.

**Example**:
```bash
curl <service-url>/api/v1/plugins
```

**Response**:
```json
{
  "plugins": [
    {
      "id": "image-resizer",
      "version": "1.2.0",
      "author": "NeuraCraft Team",
      "description": "Image resizing plugin"
    }
  ]
}
```

---

### Register Custom Plugin
**POST** `/plugins/custom`

Uploads a custom plugin package (ZIP format).

**Headers**:
```
Content-Type: multipart/form-data
```

**Form Data**:
| Field  | Type     | Description          |
|--------|----------|----------------------|
| `file` | `binary` | Plugin ZIP package  |
| `meta` | `string` | JSON configuration  |

**Example**:
```bash
curl -X POST <service-url>/api/v1/plugins/custom \
  -F "file=@custom-plugin.zip" \
  -F "meta='{\"author\":\"Your Name\"}'"
```

**Response**:
```json
{
  "plugin_id": "custom-feature",
  "version": "1.0.0",
  "warnings": ["No icon provided"]
}
```

---

## Execution Management

### Start Pipeline Run
**POST** `/pipelines/{id}/run`

Triggers execution of a pipeline with optional payload.

**Request Body**:
```json
{
  "input": {},
  "async": true
}
```

**Example**:
```bash
curl -X POST <service-url>/api/v1/pipelines/pipe_abc123/run \
  -H "Content-Type: application/json" \
  -d '{"input": {"image_url": "https://example.com/image.jpg"}}'
```

**Response**:
```json
{
  "run_id": "run_def456",
  "status": "queued",
  "monitor_url": "<service-url>/api/v1/runs/run_def456"
}
```

---

### Get Run Status
**GET** `/runs/{run_id}`

Retrieves current status and results of a pipeline run.

**Example**:
```bash
curl <service-url>/api/v1/runs/run_def456
```

**Response**:
```json
{
  "id": "run_def456",
  "pipeline_id": "pipe_abc123",
  "status": "completed",
  "output": {
    "resized_image": "s3://bucket/resized.jpg",
    "tags": ["cat", "outdoor"]
  },
  "started_at": "2023-11-15T10:00:00Z",
  "completed_at": "2023-11-15T10:02:30Z"
}
```

---

### Cancel Running Pipeline
**POST** `/runs/{run_id}/cancel`

Attempts to terminate an active pipeline execution.

**Example**:
```bash
curl -X POST <service-url>/api/v1/runs/run_def456/cancel
```

**Response**:
```json
{"status": "cancellation_requested"}
```

---

## Error Responses
All endpoints return standardized errors:

```json
{
  "error": {
    "code": "not_found",
    "message": "Resource not found"
  }
}
```

**Common Status Codes**:
- `400` Bad Request  
- `401` Unauthorized  
- `404` Not Found  
- `429` Rate Limited  
- `500` Internal Server Error