
# GPT-4 Handling Services for SermonSync

This file contains modular Python functions that handle the core GPT-4 features using OpenAI's API via `openai` package.

## Prerequisites

Install OpenAI SDK:
```bash
pip install openai
```

Set your environment variable or `.env` file:
```
OPENAI_API_KEY=your_api_key_here
```

## Code Snippets

### 1. Setup OpenAI Client

```python
import os
import openai
from dotenv import load_dotenv

load_dotenv()
openai.api_key = os.getenv("OPENAI_API_KEY")
```

### 2. Scripture Reference Extraction

```python
def extract_scriptures(text: str) -> list:
    prompt = f"""Extract any scripture references mentioned in the following text:
    "{text}"
    Return as a JSON array with book, chapter, and verse."""

    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.3
    )
    return response['choices'][0]['message']['content']
```

### 3. Sermon Summarization

```python
def summarize_sermon(text: str) -> str:
    prompt = f"""Summarize the following sermon in bullet points for easy understanding:

{text}"""

    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.5
    )
    return response['choices'][0]['message']['content']
```

### 4. Theological Term Definition

```python
def define_terms(text: str) -> str:
    prompt = f"""Identify any theological or complex terms in this text and define them simply for a new Christian:

{text}"""

    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.4
    )
    return response['choices'][0]['message']['content']
```

### 5. Language Translation (Optional)

```python
def translate_text(text: str, target_language: str = "Spanish") -> str:
    prompt = f"""Translate this sermon into {target_language}:

{text}"""

    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.3
    )
    return response['choices'][0]['message']['content']
```
