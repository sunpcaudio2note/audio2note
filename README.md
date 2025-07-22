# ![Service Logo](https://raw.githubusercontent.com/sunpcaudio2note/audio2note/main/docs/website/pic/service-logo.svg) Audio2Note Service

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/sunpcaudio2note/audio2note)
[![Code Coverage](https://img.shields.io/badge/coverage-95%25-brightgreen)](https://github.com/sunpcaudio2note/audio2note)
[![Version](https://img.shields.io/badge/version-1.0.0-blue)](https://github.com/sunpcaudio2note/audio2note)
[![License](https://img.shields.io/badge/license-MIT-blue)](https://github.com/sunpcaudio2note/audio2note/blob/main/LICENSE)
[![Join Waitlist](https://img.shields.io/badge/join-waitlist-orange)](https://www.audio2note.org/waitlist)

> AI-powered clinical documentation service that transforms audio encounters into structured, accurate, and actionable notes.

![Workflow GIF](https://raw.githubusercontent.com/sunpcaudio2note/audio2note/main/docs/website/pic/workflow.gif)

## Overview

Clinical documentation is a critical but time-consuming task for healthcare professionals. The burden of manual data entry leads to burnout, reduces patient face-time, and increases the risk of errors. **Audio2Note** is a secure, HIPAA-compliant service designed to solve this problem. It provides a seamless workflow to transform audio from patient encounters into structured clinical notes, complete with AI-generated billing codes, which can be integrated into any Electronic Medical Record (EMR) system via a modern REST API.

## Key Features

-   🎙️ **Audio Transcription:** Upload audio files (`.wav`, `.mp3`, `.m4a`, `.flac`, `.ogg`) for fast and accurate transcription.
-   ✍️ **Structured Note Generation:** Automatically generate structured clinical notes (SOAP, H&P) from transcribed text.
-   **AI-Powered Coding:** Leverage advanced AI with Chain-of-Thought and Few-Shot prompting for highly accurate ICD-10, HCPCS, and E/M code suggestions.
-   🔒 **HIPAA-Compliant Security:** End-to-end encryption, secure data handling, and a commitment to patient privacy.
-   ⚙️ **REST API for EMR Integration:** A flexible and well-documented REST API allows for seamless integration with any EMR or clinical software.
-   **Tiered Priority Processing:** A waitlist system prioritizes jobs based on subscription tiers, ensuring faster turnaround for premium users.

## Technology Stack

| Technology | Description |
| :--- | :--- |
| **Python** | Core backend logic and API development. |
| **FastAPI** | High-performance web framework for the REST API. |
| **Docker** | Containerization for consistent development and deployment. |
| **PostgreSQL** | Robust and scalable database for job and user management. |
| **n8n** | Workflow automation for the audio processing pipeline. |
| **Supabase** | Secure, temporary storage for audio files. |

## Architecture

The Audio2Note service is built on a decoupled, dual-workflow architecture to ensure security, scalability, and maintainability. An API Gateway workflow handles all external interactions, including authentication, authorization, and rate-limiting, before passing validated jobs to a core processing workflow.

![Architecture Diagram](https://raw.githubusercontent.com/sunpcaudio2note/audio2note/main/docs/website/pic/architecture-diagram.png)

## Getting Started

Using the Audio2Note service is simple and does not require any local installation or container setup.

1.  **Visit our Website:** Go to [www.audio2note.org](https://www.audio2note.org).
2.  **Start Your Free Trial:** You can begin using the service immediately with our free trial period.
3.  **Start Transcribing:** Use either the secure web form on our website or integrate directly with your own software using our REST API.

## API Usage

The Audio2Note REST API provides endpoints for submitting audio files and checking the status of processing jobs. All requests must be authenticated with a valid license key.

-   `POST /website_initiate_transcription`: Submit a `multipart/form-data` request with an audio file.
-   `GET /website_status`: Check the status of a job using its `job_id`.

For detailed information on endpoints, request/response formats, and authentication, please see our full **[API Reference](API_REFERENCE.md)**.

## Further Documentation

-   **[Frequently Asked Questions](FAQ.md)**
-   **[HIPAA Compliance and Security](HIPAA_Compliance.md)**

## Contributing

Our mission is to provide a clinical documentation solution that is private, resource-efficient, and affordable for healthcare professionals. We are passionate about leveraging lightweight, specialized AI to protect patient data and reduce operational costs.

If you share our vision and are interested in contributing, we would love to hear from you. Please reach out to us through our [Contact Form](https://www.audio2note.org/?page_id=136) to discuss potential collaboration opportunities.