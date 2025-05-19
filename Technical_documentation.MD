# SermonSync Backend & AI System – Technical Documentation

##  Overview

SermonSync is an AI-powered backend service that processes sermon content to deliver transcription, scripture detection, term definitions, summaries, and more. This system integrates OpenAI’s GPT-4/GPT-5 models, Whisper for transcription, and FastAPI as the backend framework.

---

## System Architecture

```plaintext
Client (Web App / Plugin)
        |
        v
  FastAPI Backend (API Layer)
        |
        ├── GPT Services (GPT-4/GPT-5)
        ├── Whisper Transcription (Optional local inference or OpenAI API)
        ├── MongoDB (Optional sermon storage)
        |
        v
    JSON Responses (Scripture, Summary, Definitions, etc.)
```

---

## Technology Stack

| Layer         | Tool/Framework         | Purpose                                     |
| ------------- | ---------------------- | ------------------------------------------- |
| Backend       | **FastAPI**            | Python web API framework                    |
| AI/ML         | **OpenAI GPT-4/5**     | Natural language understanding              |
| Transcription | **OpenAI Whisper**     | Speech-to-text (can run locally or via API) |
| Data Storage  | **MongoDB** (optional) | Sermon and metadata persistence             |
| Deployment    | **Render.com**         | Cloud-based deployment                      |
| Environment   | **Python 3.11+**       | Runtime                                     |

---

## Project Structure

```
sermonsync-backend/
├── app/
│   ├── api/              # FastAPI route handlers
│   ├── core/             # Config and environment setup
│   ├── db/               # MongoDB integration (optional)
│   ├── models/           # Request/Response schemas (Pydantic)
│   ├── services/         # GPT & Whisper logic
│   └── main.py           # FastAPI application entry point
├── requirements.txt      # Dependencies
└── .env                  # API keys and secrets
```

---

## AI & ML Modules

### 1.  Transcription (Whisper)

* **Input**: Audio file (.mp3/.wav)
* **Output**: Transcribed sermon text

#### Service Flow:

1. Upload audio file.
2. Pass to Whisper (local or via OpenAI).
3. Return text output to user.

```python
# whisper_service.py
def transcribe_audio(file: UploadFile) -> str:
    audio_bytes = file.file.read()
    result = whisper_model.transcribe(audio_bytes)
    return result["text"]
```

---

### 2. Scripture Reference Extraction (GPT)

* **Input**: Sermon paragraph
* **Output**: JSON array of Bible references

#### Example Prompt:

```text
Extract any scripture references mentioned in the following text:
"Today we reflect on Romans 8:28 and Philippians 4:13..."
Return as a JSON array with book, chapter, and verse.
```

#### Example Output:

```json
[
  {"book": "Romans", "chapter": 8, "verse": 28},
  {"book": "Philippians", "chapter": 4, "verse": 13}
]
```

---

### 3. Sermon Summarization (GPT)

* **Input**: Full sermon or paragraph
* **Output**: Bullet-point or paragraph summary

#### Example Output:

```json
{
  "summary": [
    "God works all things for the good of those who love Him.",
    "We must trust Him through hardship.",
    "Romans 8:28 serves as a foundation of faith."
  ]
}
```

---

### 4. Theological Term Definition (GPT)

* **Input**: Sentence or phrase with complex terms
* **Output**: Definitions of detected terms

#### Prompt Logic:

```text
Identify any theological or complex terms and define them simply for a new Christian.
Text: "Justification by faith means we are declared righteous through belief in Jesus."
```

---

### 5. Multilingual Support

* Translate sermons to multiple languages using GPT-4.
* Detect language preference via query param or browser header.

---

## 🔐 Authentication & Rate Limiting

For production use, consider adding:

* API Key auth or OAuth2
* Request throttling for OpenAI endpoints
* Abuse prevention with CAPTCHA or token quotas

---

## Deployment on Render

### Environment Variables

```
OPENAI_API_KEY=your_openai_key
MONGO_URI=your_mongo_db_url (optional)
```

### Render Config:

* Runtime: Python 3.11
* Build Command: `pip install -r requirements.txt`
* Start Command: `uvicorn app.main:app --host 0.0.0.0 --port 10000`

---

## API Reference

### `POST /api/v1/transcribe`

* Input: Audio file (multipart/form)
* Output: Transcribed text

### `POST /api/v1/scripture`

* Input: `{ "content": "..." }`
* Output: List of scripture references

### `POST /api/v1/summarize`

* Input: `{ "content": "..." }`
* Output: Sermon summary

### `POST /api/v1/define`

* Input: `{ "content": "..." }`
* Output: Term definitions

---

## Database Schema

```json
{
  "title": "Hope in Suffering",
  "transcript": "...",
  "summary": ["...", "..."],
  "references": [
    { "book": "Romans", "chapter": 8, "verse": 28 }
  ],
  "createdAt": "2025-05-19T14:00:00Z"
}
```


## Plugin / Browser Extension Integration

* Send real-time content from browser to `/summarize`, `/define`, or `/scripture`
* Inject tooltip or highlight UI based on API results
* Handle auth via JWT or API keys


## GPT Prompt Engineering Notes

* Use **few-shot examples** to improve consistency
* For scripture detection, instruct GPT to return only structured JSON
* For definitions, specify the audience (e.g., “young Christian”)



## Roadmap

* [ ] Real-time audio streaming with WebSocket
* [ ] Browser extension (Chrome & Edge)
* [ ] GPT-5 upgrade for better abstraction
* [ ] User accounts with saved sermon history


