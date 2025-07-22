# Audio2Note REST API Reference v1.0.0

Welcome to the Audio2Note REST API. Our API allows you to integrate powerful, AI-driven clinical documentation directly into your applications.

## Authentication

All API requests must be authenticated using a license key. The key must be provided in the `x-license-key` header.

```
x-license-key: YOUR_LICENSE_KEY
```

---

## Endpoints

The base URL for the production server is: `https://backend.audio2note.org/webhook/`

### **Job Submission**

#### `POST /api_initiate_transcription`

Initiates a new transcription and note generation job by uploading an audio file.

**Request Body:** `multipart/form-data`

**Parameters:**

| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `audio` | File | **Yes** | The audio file to be transcribed. Supported formats: `.wav`, `.mp3`, `.m4a`, `.flac`, `.ogg`. |
| `note_type` | String | **Yes** | The type of clinical note to generate. Must be one of `SOAP`, `History and Physical`, or `Summary`. |
| `visit_category` | String | **Yes** | The category of the visit. Required if `note_type` is not 'Summary'. See below for valid options. |
| `visit_class` | String | **Yes** | The class of the visit. Valid options are determined by `visit_category`. |
| `visit_type` | String | **Yes** | The specific type of visit. Valid options are determined by `visit_category`. |
| `min_speakers` | Integer | No | The minimum number of speakers in the audio. Default: `1`. |
| `max_speakers` | Integer | No | The maximum number of speakers in the audio. Default: `2`. |

**Responses:**

*   **`202 Accepted`**: The job has been successfully accepted for processing.
    ```json
    {
      "job_id": "123e4567-e89b-12d3-a456-426614174000",
      "status": "queued"
    }
    ```
*   **`400 Bad Request`**: The request body is invalid or missing required fields.
*   **`403 Forbidden`**: The license key is invalid or expired.

---

### **Job Status**

#### `GET /api_status`

Retrieves the current status and results of a transcription job.

**Query Parameters:**

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `job_id` | String | **Yes** | The unique identifier of the job to check. |

**Responses:**

*   **`200 OK`**: The job status and results.
    ```json
    {
      "job_id": "123e4567-e89b-12d3-a456-426614174000",
      "status": "completed",
      "results": {
        "subjective": "The patient reports...",
        "objective": "On examination...",
        "assessment": "Assessment details...",
        "plan": "Plan details...",
        "billing_content": "ICD-10: ..., HCPCS: ..."
      }
    }
    ```
*   **`404 Not Found`**: The specified `job_id` does not exist.
*   **`429 Too Many Requests`**: The user has exceeded the rate limit.

---

## Visit Details Parameter Combinations

The `visit_category`, `visit_class`, and `visit_type` parameters are interdependent. The selection of a `visit_category` filters the available options for the other two fields. Below is a complete list of valid combinations.

<details>
<summary><strong>Office or Other Outpatient Services - New Patient</strong></summary>

*   **`visit_class`**: `"Outpatient"`
*   **`visit_type`**:
    *   `"Office o/p new sf 15 min"`
    *   `"Office o/p new low 30 min"`
    *   `"Office o/p new mod 45 min"`
    *   `"Office o/p new hi 60 min"`
</details>

<details>
<summary><strong>Office or Other Outpatient Services - Established Patient</strong></summary>

*   **`visit_class`**: `"Outpatient"`
*   **`visit_type`**:
    *   `"Off/op est may x req phy/qhp"`
    *   `"Office o/p est sf 10 min"`
    *   `"Office o/p est low 20 min"`
    *   `"Office o/p est mod 30 min"`
    *   `"Office o/p est hi 40 min"`
</details>

<details>
<summary><strong>Hospital Inpatient and Observation Care Services</strong></summary>

*   **`visit_class`**: `"Inpatient"`
*   **`visit_type`**:
    *   `"1st hosp ip/obs sf/low 40"`
    *   `"1st hosp ip/obs moderate 55"`
    *   `"1st hosp ip/obs high 75"`
    *   `"Sbsq hosp ip/obs sf/low 25"`
    *   `"Sbsq hosp ip/obs moderate 35"`
    *   `"Sbsq hosp ip/obs high 50"`
    *   `"Hosp ip/obs sm dt sf/low 45"`
    *   `"Hosp ip/obs same date mod 70"`
    *   `"Hosp ip/obs same date hi 85"`
    *   `"Hosp ip/obs dschrg mgmt 30/<"`
    *   `"Hosp ip/obs dschrg mgmt >30"`
</details>

<details>
<summary><strong>Consultations</strong></summary>

*   **`visit_class`**: `"Consultation"`
*   **`visit_type`**:
    *   `"Off/op consltj new/est sf 20"`
    *   `"Off/op cnsltj new/est low 30"`
    *   `"Off/op cnsltj new/est mod 40"`
    *   `"Off/op consltj new/est hi 55"`
    *   `"Ip/obs consltj new/est sf 35"`
    *   `"Ip/obs cnsltj new/est low 45"`
    *   `"Ip/obs cnsltj new/est mod 60"`
    *   `"Ip/obs consltj new/est hi 80"`
</details>

<details>
<summary><strong>Emergency Department Services</strong></summary>

*   **`visit_class`**: `"Emergency"`
*   **`visit_type`**:
    *   `"Emr dpt vst mayx req phy/qhp"`
    *   `"Emergency dept visit sf mdm"`
    *   `"Emergency dept visit low mdm"`
    *   `"Emergency dept visit mod mdm"`
    *   `"Emergency dept visit hi mdm"`
    *   `"Direct advanced life support"`
</details>

<details>
<summary><strong>Nursing Facility Services</strong></summary>

*   **`visit_class`**: `"Nursing Facility"`
*   **`visit_type`**:
    *   `"1st nf care sf/low mdm 25"`
    *   `"1st nf care moderate mdm 35"`
    *   `"1st nf care high mdm 50"`
    *   `"Sbsq nf care sf mdm 10"`
    *   `"Sbsq nf care low mdm 20"`
    *   `"Sbsq nf care moderate mdm 30"`
    *   `"Sbsq nf care high mdm 45"`
    *   `"Nf dschrg mgmt 30 min/less"`
    *   `"Nf dschrg mgmt 30 min+"`
</details>

<details>
<summary><strong>Home or Residence Services</strong></summary>

*   **`visit_class`**: `"Home or Residence"`
*   **`visit_type`**:
    *   `"Home/res vst new sf mdm 15"`
    *   `"Home/res vst new low mdm 30"`
    *   `"Home/res vst new mod mdm 60"`
    *   `"Home/res vst new high mdm 75"`
    *   `"Home/res vst est sf mdm 20"`
    *   `"Home/res vst est low mdm 30"`
    *   `"Home/res vst est mod mdm 40"`
    *   `"Home/res vst est high mdm 60"`
</details>

<details>
<summary><strong>Prolonged Services</strong></summary>

*   **`visit_class`**: `"Prolonged Services"`
*   **`visit_type`**:
    *   `"Prolong service w/o contact"`
    *   `"Prolong serv w/o contact add"`
</details>
