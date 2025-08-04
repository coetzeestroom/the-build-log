This project implements a serverless Text-to-Speech (TTS) API on Google Cloud, along with a podcast feed generator.

Here's a breakdown of the different pieces:

*   **`src/function`**: This directory contains the core Text-to-Speech Cloud Function.
    *   `main.py`: This Python script defines an HTTP-triggered Cloud Function that takes text as input, uses the Gemini API to convert it into speech (MP3 format), and then uploads the generated audio file to a Google Cloud Storage bucket. It retrieves the Gemini API key from Google Secret Manager.
    *   `requirements.txt`: Lists the Python dependencies for the `main.py` function (e.g., `functions-framework`, `google-generativeai`, `google-cloud-secret-manager`, `google-cloud-storage`).

*   **`src/podcast-function`**: This directory contains a Cloud Function that generates an RSS feed for a podcast.
    *   `main.py`: This Python script defines an HTTP-triggered Cloud Function that authenticates requests using Basic Authentication (credentials stored in Secret Manager). It then lists all audio files (MP3, WAV) in a specified Google Cloud Storage bucket, sorts them by creation time, and generates an RSS 2.0 feed with each audio file as a podcast episode.
    *   `requirements.txt`: Lists the Python dependencies for the `main.py` function (e.g., `google-cloud-storage`, `google-cloud-secret-manager`).

*   **`src/terraform`**: This directory contains Terraform configurations for deploying the Google Cloud infrastructure.
    *   `main.tf`: Defines the main Google Cloud resources, including Cloud Functions, API Gateway, Google Cloud Storage buckets, and Secret Manager secrets. It likely references the Python functions in `src/function` and `src/podcast-function`.
    *   `openapi.yaml.tftpl`: A Terraform template for the OpenAPI specification of the Text-to-Speech API, used by API Gateway.
    *   `podcast-openapi.yaml.tftpl`: A Terraform template for the OpenAPI specification of the Podcast API.
    *   `variables.tf`: Defines input variables for the Terraform configuration (e.g., project ID, secret names, bucket names).
    *   `outputs.tf`: Defines output values from the Terraform deployment, such as the API Gateway URL.

*   **`build.sh`**: This shell script automates the deployment process. It likely uses `gcloud` commands to enable necessary APIs and `terraform` commands to provision the infrastructure defined in `src/terraform`.

*   **`README.md`**: Provides instructions on how to deploy and use the Text-to-Speech API.

In summary, this project provides a complete solution for:
1.  Converting text to speech using the Gemini API and storing the audio in Google Cloud Storage.
2.  Generating a podcast RSS feed from the audio files stored in Google Cloud Storage, with basic authentication for access.
3.  Automating the deployment of all necessary Google Cloud resources using Terraform.