# Docs API Reference and Fathom Integration Update Plan

Date: 2026-02-01
Scope: Update Mintlify docs to point API reference at https://api.objectionly.com, document the C# queue call endpoint, refresh Fathom integration docs with step by step setup and image placeholders, and align docs theme colors with the Objectionly app.

## Sources Reviewed
- docs/docs.json
- docs/api-reference/introduction.mdx
- docs/api-reference/queue-call-processing.mdx
- docs/integrations/fathom.mdx
- objectionly-app/src/styles/globals.css (theme tokens)
- objectionly-app/src/app/_components/dashboard/integrations/FathomIntegration.tsx (UI labels and flow)
- objectionly-app/src/app/api/call-processing/queue/route.ts (legacy proxy and ping)
- objectionly-CallProcessingAPI/src/CallProcessing.Api/Controllers/CallProcessingQueueController.cs
- objectionly-CallProcessingAPI/src/CallProcessing.Api/Program.cs (service config)
- objectionly-CallProcessingAPI/src/CallProcessing.Api/Controllers/TranscriptionController.cs (internal)
- objectionly-CallProcessingAPI/tests/CallProcessing.Api.Tests/CallProcessingApiTests.cs (queue responses and 415 behavior)

## Color Scheme Mapping for docs.json
From objectionly-app/src/styles/globals.css:
- --primary (light theme): oklch(0.3 0.15 245) -> #002C72
- --ring (light theme): oklch(0.623 0.214 259.815) -> #2B7FFF
- --primary (dark theme): oklch(70.369% 0.1034 221.345) -> #48AECD
- --sidebar-primary (dark theme): oklch(0.546 0.245 262.881) -> #155DFC

Planned mapping for Mintlify colors:
- primary: #002C72
- light: #2B7FFF
- dark: #155DFC

(If you prefer the dark accent to match the dark theme primary, we can use #48AECD instead of #155DFC.)

## Implementation Steps

### 1) Update API Reference Introduction
Files:
- docs/api-reference/introduction.mdx

Actions:
- Rename from Zapier focused language to a general Call Processing API overview.
- State base URL as https://api.objectionly.com.
- Keep workspace API key location guidance (Integrations -> Zapier) unless we want to move this to a more general API key section.

### 2) Update Queue Endpoint Documentation
Files:
- docs/api-reference/queue-call-processing.mdx

Actions:
- Update Base URL to https://api.objectionly.com.
- Remove Zapier specific header guidance (X-Objectionly-Source is not used by the C# endpoint).
- Add two request variants:
  - JSON body with transcript text.
  - multipart/form-data with file and optional transcript.
- Align field validation with C#:
  - title required
  - repEmail required and must be valid
  - callDate required ISO string
  - callDuration optional integer minutes, must be positive
  - transcript or file required
  - externalMeetingId optional for idempotency
- Update responses to include duplicate behavior and error codes:
  - 200 queued
  - 200 duplicate
  - 400 validation errors or missing headers
  - 404 invalid workspace API key
  - 415 unsupported audio type
  - 422 no transcript provided or generated

### 3) Update Fathom Integration Documentation
Files:
- docs/integrations/fathom.mdx

Actions:
- Replace the placeholder note with a complete setup guide using Mintlify Steps.
- Use the exact user flow you provided, aligning labels with the UI (button text is "Connect to Fathom"):
  1. Go to https://app.objectionly.com/dashboard/integrations
  2. Click Connect on the Fathom card (include image placeholder)
  3. Open https://fathom.video/api_clients/new and create a new API key (recommend name "Objectionly API key")
  4. Copy the key, paste into the Objectionly text box, and click "Connect to Fathom" (include image placeholder)
- Add a short "What happens next" section (auto sync and optional historical import) to match other integration docs.

### 4) Add Image Placeholders
Files:
- docs/images/integrations/fathom (new directory)
- docs/integrations/fathom.mdx

Actions:
- Add placeholder images for each step and reference them in the doc:
  - fathom-step-1-dashboard-integrations.png
  - fathom-step-2-connect.png
  - fathom-step-3-api-client.png
  - fathom-step-4-paste-key.png
- Use a simple placeholder style (light gray background, label text) so they can be swapped later.

### 5) Apply Theme Colors in docs.json
Files:
- docs/docs.json

Actions:
- Replace the current green palette with the Objectionly app colors from globals.css.
- Keep existing theme and mode toggle settings unchanged.

## Verification
- Run mint dev from /docs to validate navigation, colors, and image paths.
- Spot check API reference pages for correct base URL and endpoint coverage.

## Open Questions
- Confirm dark accent color: #155DFC (sidebar primary) vs #48AECD (dark theme primary).
