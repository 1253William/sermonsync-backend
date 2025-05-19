
# FastAPI Features for SermonSync Backend

## Directory Structure

```
sermonsync_backend/
├── app/
│   ├── main.py
│   ├── api/
│   │   ├── v1/
│   │   │   ├── routes/
│   │   │   │   ├── sermon_routes.py
│   │   │   │   ├── auth_routes.py
│   │   │   │   ├── user_routes.py
│   │   │   │   └── ai_routes.py
│   │   │   └── controllers/
│   │   │       ├── sermon_controller.py
│   │   │       ├── auth_controller.py
│   │   │       ├── user_controller.py
│   │   │       └── ai_controller.py
│   ├── models/
│   │   ├── user_model.py
│   │   ├── sermon_model.py
│   │   └── auth_model.py
│   ├── schemas/
│   │   ├── user_schema.py
│   │   ├── sermon_schema.py
│   │   └── auth_schema.py
│   ├── services/
│   │   ├── auth_service.py
│   │   ├── user_service.py
│   │   └── sermon_service.py
│   └── utils/
│       ├── database.py
│       └── gpt_handler.py
```

## Example Routes (sermon_routes.py)
```python
from fastapi import APIRouter, Depends
from app.api.v1.controllers.sermon_controller import create_sermon, get_sermon_by_id

router = APIRouter(prefix="/sermons", tags=["Sermons"])

@router.post("/")
def add_sermon(data: dict = Depends(create_sermon)):
    return data

@router.get("/{sermon_id}")
def retrieve_sermon(sermon_id: str):
    return get_sermon_by_id(sermon_id)
```

## Example Controller (sermon_controller.py)
```python
from app.services.sermon_service import SermonService

def create_sermon(data):
    return SermonService.create_sermon(data)

def get_sermon_by_id(sermon_id):
    return SermonService.get_sermon_by_id(sermon_id)
```

## Example Model (sermon_model.py)
```python
from pydantic import BaseModel

class Sermon(BaseModel):
    title: str
    preacher: str
    date: str
    transcript: str
    summary: str = None
    takeaways: list = []
```

## GPT Integration (gpt_handler.py)
```python
import openai

def summarize_sermon(transcript: str):
    prompt = f"Summarize this sermon: {transcript}"
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message["content"]
```

## Main Application Entry (main.py)
```python
from fastapi import FastAPI
from app.api.v1.routes import sermon_routes

app = FastAPI(title="SermonSync API")

app.include_router(sermon_routes.router)
```
