# Audio2Note REST API Reference

The Audio2Note REST API allows developers to integrate our powerful audio transcription and clinical note generation service directly into their applications.

## Authentication

All API requests must be authenticated using a license key. The key must be included in the request header as `x-license-key`.

```
x-license-key: YOUR_LICENSE_KEY
```

## Endpoints

### Job Submission

#### `POST /website_initiate_transcription`

This endpoint is used to submit a new audio file for processing. It accepts `multipart/form-data` requests.

**Parameters:**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `audio_file` | File | The audio file to be processed. Supported formats: `.wav`, `.mp3`, `.m4a`, `.flac`, `.ogg`. |
| `transcription_params` | JSON String | (Optional) A JSON string containing transcription parameters. |

**Example Request:**

```bash
curl -X POST \
  https://api.audio2note.org/website_initiate_transcription \
  -H 'x-license-key: YOUR_LICENSE_KEY' \
  -F 'audio_file=@/path/to/your/audio.mp3'
```

**Example Response (Success):**

```json
{
  "status": "queued",
  "job_id": "123e4567-e89b-12d3-a456-426614174000"
}
```

### Job Status

#### `GET /website_status`

This endpoint is used to check the status of a previously submitted job.

**Parameters:**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `job_id` | String | The ID of the job to check. |

**Example Request:**

```bash
curl -X GET \
  'https://api.audio2note.org/website_status?job_id=123e4567-e89b-12d3-a456-426614174000' \
  -H 'x-license-key: YOUR_LICENSE_KEY'
```

**Example Response (Completed):**

```json
{
  "job_id": "123e4567-e89b-12d3-a456-426614174000",
  "status": "completed",
  "transcription": "The patient presents with...",
  "hnp": "History of Present Illness...",
  "billing": "ICD-10: ..., HCPCS: ...",
  "submitted_at": "2025-07-22T16:00:00Z",
  "completed_at": "2025-07-22T16:05:00Z"
}
```

## Rate Limiting

The API enforces rate limiting on a per-user basis to ensure fair usage. If you exceed the rate limit, you will receive a `429 Too Many Requests` response.