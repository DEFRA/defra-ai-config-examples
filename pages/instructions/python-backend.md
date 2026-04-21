---
layout: default
title: Python Backend Instructions
---

# Python Backend Instructions — Example

This is an example scoped instruction file for Python backend code. It activates when Copilot is working on Python files. Per [Defra Python standards](https://defra.github.io/software-development-standards/standards/python_coding_standards/){:target="_blank"} (opens in new tab), Python is for AI or data-science backends only — use Node.js + Hapi for general backend and frontend services. Copy it into `.github/instructions/python-backend.instructions.md` in your repository.

## Example file contents

The content below goes into your `.github/instructions/python-backend.instructions.md` file:

---

````markdown
---
applyTo: "**/*.py"
---

# Python Backend

## Language and style

- Use Python 3.12 or later (Active supported version)
- Follow [PEP 8](https://peps.python.org/pep-0008/) for all code style
- Use type hints on all function signatures and class attributes
- Use Pyright for static type checking
- Use `snake_case` for functions, variables, and module names
- Use `PascalCase` for class names
- Use `UPPER_CASE` for constants
- Maximum function length: 50 lines, maximum nesting depth: 3 levels
- Use Google-style docstrings for all public functions and classes
- Indent with 4 spaces, maximum line length 79 characters
- Use `ruff` for formatting, linting, code style, and complexity — the Defra-mandated all-in-one tool. Do not use `black`, `flake8`, `pylint`, or `isort`.
- Use containerisation with Docker with Defra base images

## Framework (FastAPI)

- Use [FastAPI](https://fastapi.tiangolo.com/) for all HTTP services
- Use async functions (`async def`) for all route handlers — do not use synchronous handlers
- Use Pydantic models for request and response schemas — never use plain dicts for API contracts
- Use dependency injection for shared resources (database, config, auth)
- Version APIs in the URL path: `/api/v1/...`
- Use `HTTPException` for all HTTP error responses with appropriate status codes

```python
from fastapi import FastAPI, HTTPException, Depends
from pydantic import BaseModel

app = FastAPI()

class ApplicationRequest(BaseModel):
    name: str
    reference: str

@app.post("/api/v1/applications", status_code=201)
async def create_application(
    payload: ApplicationRequest,
    db = Depends(get_db)
) -> dict:
    result = await db.applications.insert_one(payload.model_dump())
    return {"id": str(result.inserted_id)}
```

## Configuration

- Use [Pydantic Settings](https://docs.pydantic.dev/latest/concepts/pydantic_settings/) for all configuration — never access `os.environ` directly in application code
- Load configuration from environment variables with validation
- Use `UPPER_SNAKE_CASE` for environment variable names

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    port: int = 8080
    log_level: str = "info"
    mongo_uri: str
    service_name: str = "my-service"

    class Config:
        env_file = ".env"

settings = Settings()
```

## Project structure

```
src/
├── api/v1/          # Route definitions and HTTP handlers
│   └── {resource}/  # Resource-grouped routes
├── services/        # Business logic
├── repositories/    # Data access (database queries)
├── models/          # Pydantic data models and schemas
├── config/          # Configuration module
├── utils/           # Shared utility functions
└── main.py          # FastAPI app entrypoint
tests/
├── unit/            # Unit tests for services and repositories
└── integration/     # Integration tests for API endpoints
```

## Database (MongoDB)

- Use the async MongoDB driver (`motor`) — do not use Mongoose-style ODMs
- Define collection names as constants — use `camelCase` plural (e.g. `applications`)
- Use repository classes to encapsulate all database access — never call the DB client directly from route handlers
- Set connection timeouts and pool limits
- Index frequently queried fields

## Security

- Validate all inputs using Pydantic schemas
- Never log PII: no names, addresses, emails, phone numbers, NI numbers, API keys, or tokens
- Load all secrets and credentials from environment variables — never hard-code them
- Use parameterised queries — never concatenate user input into query strings
- Set CORS origins explicitly — never use `allow_origins=["*"]` in production

## Testing

- Use [pytest](https://pytest.org/) with `pytest-asyncio` for async tests
- Use `httpx.AsyncClient` and FastAPI's `TestClient` for route testing
- Test behaviour, not implementation — test the HTTP response, not internal state
- Mock external services and database calls
- Test error scenarios and validation failures
- Target ≥90% test coverage on all application code

```python
import pytest
from httpx import AsyncClient
from src.main import app

@pytest.mark.asyncio
async def test_create_application_returns_201():
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.post(
            "/api/v1/applications",
            json={"name": "Test", "reference": "AB123456"}
        )
    assert response.status_code == 201
    assert "id" in response.json()
```

## Logging

- Use structured JSON logging (`structlog` or `python-json-logger`)
- Include a correlation ID on every log entry for request tracing
- Never log PII
- Use log levels appropriately: `error` for failures, `info` for key events, `debug` for development detail only

## Health endpoint

- Expose a health endpoint at `/health`
- Return `200 OK` and `{"status": "ok"}` when healthy
- Do not perform heavy operations (DB writes, external API calls) in health checks

## References

- [Defra software development standards](https://github.com/DEFRA/software-development-standards){:target="_blank"} (opens in new tab)
- [FastAPI documentation](https://fastapi.tiangolo.com/){:target="_blank"} (opens in new tab)
- [PEP 8 style guide](https://peps.python.org/pep-0008/){:target="_blank"} (opens in new tab)
- [Pydantic Settings](https://docs.pydantic.dev/latest/concepts/pydantic_settings/){:target="_blank"} (opens in new tab)
- [OWASP Secure Coding Practices](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/){:target="_blank"} (opens in new tab)
````

---

[Back to instructions index]({{ "/pages/instructions" | relative_url }}) · [Back to Getting Started]({{ "/pages/getting-started" | relative_url }})
