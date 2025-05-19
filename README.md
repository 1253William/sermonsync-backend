# sermonsync-backend
SermonSync is an AI-powered backend service built with FastAPI that processes sermon content in real time to:

- 🎙️ Transcribe audio using Whisper
- 📜 Detect and extract scripture references
- 📚 Define theological terms
- 📝 Summarize sermons using GPT-4
- 🌐 (Optional) Translate content for multilingual congregations

This repository powers the backend APIs and AI integrations for the SermonSync platform.

## Tech Stack

| Tool       | Purpose                               |
|------------|----------------------------------------|
| FastAPI    | API framework                          |
| GPT-4 / GPT-5 | Semantic understanding + summarization |
| OpenAI Whisper | Audio transcription                |
| Pydantic   | Request/response validation            |
| MongoDB    | Sermon data storage (optional)         |
| Render     | Cloud deployment platform              |

---

## Project Structure
sermonsync-backend/
├── app/
│ ├── api/ # Route handlers
│ ├── core/ # Configuration and security
│ ├── db/ # MongoDB setup (optional)
│ ├── models/ # Request and response schemas
│ ├── services/ # GPT/Whisper business logic
│ └── main.py # FastAPI entry point
├── .env # Environment variables
├── requirements.txt # Dependencies
└── README.md # You're here!

## Cluster APIs

| Endpoint                  | Description                            |
| ------------------------- | -------------------------------------- |
| `POST /api/v1/transcribe` | Transcribes audio using Whisper        |
| `POST /api/v1/scripture`  | Extracts scripture references with GPT |
| `POST /api/v1/definition` | Defines complex terms with GPT         |
| `POST /api/v1/summarize`  | Summarizes sermon into bullet points   |

## Exmaple
Request:
``POST /api/v1/scripture
{
  "content": "Today we reflect on Romans 8:28 and how it applies to our daily struggles..."
}``

Response:
``
{
  "references": [
    {"book": "Romans", "chapter": 8, "verse": 28}
  ]
}
``


