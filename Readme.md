# Extract and process information directly from PDF using Claude and Gemini

This n8n workflow lets you extract and process information directly from a PDF file in a single step, while comparing the performance of Claude 3.5 Sonnet and Gemini 2.0 Flash. It removes the need to first run an OCR tool and then call an LLM separately.[^1]

## Features

- Automatically downloads a PDF from Google Drive.[^1]
- Converts the PDF to base64 so it can be processed by both APIs.[^1]
- Sends the same PDF and prompt to Claude 3.5 Sonnet and Gemini 2.0 Flash in parallel.[^1]
- Allows you to compare quality, latency, and cost using each provider’s own dashboards.[^1]


## Requirements

- A working n8n instance (self-hosted or cloud).
- A configured Google Drive credential in n8n.[^1]
- A valid Claude (Anthropic) API key.[^1]
- A valid Gemini (Google AI Studio) API key.[^1]


## Workflow setup

1. Import or open the JSON workflow file in n8n.
2. Check and configure the following credentials:
    - Google Drive credential for the **Google Drive** node using operation `download`.[^1]
    - `anthropicApi` credential for the **Call Claude 3.5 Sonnet with PDF Capabilities** node.[^1]
    - `googlePalmApi` credential for the **Call Gemini 2.0 Flash with PDF Capabilities** node.[^1]
3. Select the target PDF file in the **Google Drive** node (`fileId` field).[^1]
4. Save the workflow and click **Test workflow** to run a manual execution.[^1]

## Workflow structure

- **When clicking "Test workflow"**: Manual trigger node that starts the workflow from the n8n UI.[^1]
- **Define Prompt (Set)**: Defines the `prompt` variable used by both API calls (for example: “Extract the VAT numbers for each country”).[^1]
- **Google Drive (download)**: Downloads the PDF and stores it as binary data in n8n.[^1]
- **Extract from File**: Converts the binary PDF into a base64 string stored in the `data` property.[^1]
- **Call Claude 3.5 Sonnet with PDF Capabilities (HTTP Request)**:
    - Sends a POST request to `https://api.anthropic.com/v1/messages`.[^1]
    - Passes the base64 PDF as a `document` and the `prompt` as text.[^1]
- **Call Gemini 2.0 Flash with PDF Capabilities (HTTP Request)**:
    - Sends a POST request to `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash-exp:generateContent`.[^1]
    - Uses the same base64 PDF and the same `prompt`.[^1]


## Prompt customization

In the **Define Prompt** node, you can adapt the `prompt` value to your use case:[^1]

- Extract invoice fields (invoice number, total amount, VAT, date).
- Extract table data (for example, line items).
- Transform the output into JSON or any other structured format.

For Gemini, you can force a JSON response by adding a configuration like this in the request body:[^1]

```json
"generationConfig": {
  "responseMimeType": "application/json"
}
```

For Claude, you can increase JSON consistency using the “Prefill response format” technique described in their documentation.[^1]

## How to use

1. Upload the target PDF to Google Drive.
2. Adjust the `prompt` so it clearly describes which information to extract and in which output format.[^1]
3. Run **Test workflow** and inspect the outputs from both HTTP Request nodes.
4. Compare the results (quality, structure, latency, cost) between Claude and Gemini using their respective dashboards.[^1]

***

Would you like an even more technical version (with example responses and full HTTP payloads), or is this level of detail enough for your repository?

<div align="center">⁂</div>

[^1]: Extract-and-process-information-directly-from-PDF-using-Claude-and-Gemini.json

