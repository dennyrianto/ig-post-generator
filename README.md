# Rumah123 - Weekly Instagram Post Generator

![n8n](https://img.shields.io/badge/n8n-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Gemini_API-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white)
![Pollinations.ai](https://img.shields.io/badge/Pollinations.ai-FF6B6B?style=for-the-badge&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google_Sheets-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)
![Google Drive](https://img.shields.io/badge/Google_Drive-4285F4?style=for-the-badge&logo=googledrive&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-lightgrey?style=for-the-badge)

Automated n8n workflow that generates Instagram posts (caption + image) for **Rumah123** based on the latest Indonesian property news — built as part of the **99 Group AI Aptitude Challenge** (Graduate Program).

## What it does

Every week, this workflow:
1. Fetches the latest property/mortgage news from Google News RSS
2. Selects the most recent article
3. Generates an Instagram-ready caption using **Google Gemini API** (`gemini-3.5-flash`), following Rumah123's brand voice (trustworthy, relatable, aimed at Millennials/Gen Z first-time home buyers)
4. Generates a matching visual using **Pollinations.ai** (free, no API key required)
5. Uploads the generated image to Google Drive
6. Saves the final caption + image link + metadata to a Google Sheet for human review before publishing

## Architecture

```
Weekly Schedule Trigger
        │
        ▼
Fetch Property News (RSS)
        │
        ▼
Pilih Artikel Terbaru
        │
        ▼
Generate IG Caption (Gemini API)
        │
        ▼
Susun Prompt Gambar
        │
        ▼
Generate IG Image (Pollinations.ai)
        │
        ▼
Upload to Google Drive
        │
        ▼
Combine Final Output
        │
        ▼
Simpan ke Google Sheet (Draft Review)
```

## Tools & Stack

| Component | Tool | Notes |
|---|---|---|
| Orchestration | [n8n](https://n8n.io) (Community Edition, self-hosted) | Free, workflow automation |
| News source | Google News RSS | No API key needed |
| Caption generation | Google Gemini API (`gemini-3.5-flash`) | Free tier |
| Image generation | [Pollinations.ai](https://pollinations.ai) (`flux` model) | Free, no API key |
| Storage | Google Drive + Google Sheets | Free tier |

## Setup

1. Import `workflow/rumah123_ig_post_generator.json` into your n8n instance
2. Create a Gemini API key at [Google AI Studio](https://aistudio.google.com/apikey)
3. In n8n, create a **Generic Credential → Query Auth** with:
   - Name: `key`
   - Value: your Gemini API key
4. Connect the credential to the "Generate IG Caption (Gemini)" node
5. Connect your Google account for the Google Drive and Google Sheets nodes
6. Replace placeholder values in the workflow:
   - `GANTI_DENGAN_ID_GOOGLE_SHEET_KAMU` → your Google Sheet ID
   - `GANTI_DENGAN_FOLDER_ID_GOOGLE_DRIVE` → your Google Drive folder ID
7. Create a Google Sheet with these column headers in row 1:
   `Judul Artikel | Link Sumber | Caption Generated | Link Gambar | Tanggal Generate`
8. Run the workflow manually to test, then activate the weekly schedule

## Key Prompts

See [`docs/prompts.md`](docs/prompts.md) for the full prompts used for caption and image generation.

## Debugging Notes

- Initially attempted Gemini's native image generation model (`gemini-2.5-flash-image`), but consistently hit a `quota exceeded (limit: 0)` error even with an active API key — indicating a limitation with free-tier API access to Google's image generation models. Switched to Pollinations.ai as a free, no-key alternative to keep the pipeline fully zero-cost.
- Fixed a truncated-caption issue caused by Gemini 3.5 Flash's "thinking" token budget eating into the output token limit — resolved by raising `maxOutputTokens` and explicitly disabling `thinkingBudget`.
- API keys are stored as n8n Credentials (not hardcoded in the workflow JSON) for safe sharing.

## Disclaimer

This is a submission for 99 Group Graduate Program Assessment and not an official Rumah123 product.
