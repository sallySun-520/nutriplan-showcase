# NutriPlan — AI-Powered Supplement Plan Generator

NutriPlan is a full-stack application that generates personalised supplement plans using LLM technology. Designed for pharmaceutical companies, it automates the creation of clinically-informed supplement recommendations based on consumer health profiles.

## Demo

[Watch the full demo on Loom](https://www.loom.com/share/18dafd52183e405b854dfd54e8ba57c9)

## Key Features

- **AI Plan Generation** — Generates personalised supplement plans using GPT-4o, tailored to each consumer's health conditions, medications, allergies, and dietary restrictions
- **Async Processing** — Orders return instantly; plans are generated asynchronously via Celery workers, with real-time status polling on the frontend
- **LLM-as-Judge Evaluation** — Built-in quality scoring system that evaluates generated plans across 4 dimensions: Completeness, Safety (double-weighted), Personalisation, and Format Compliance
- **Prompt Versioning & A/B Comparison** — Track prompt iterations and compare evaluation scores across versions to measure improvements
- **Duplicate Order Prevention** — Blocks same-day duplicate submissions and warns on repeat orders within 30 days
- **Dead Letter Queue** — Failed generation tasks are captured with full tracebacks for debugging and retry
- **Monitoring** — Prometheus metrics collection and Grafana dashboards for system observability

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | Next.js, React, TypeScript, Tailwind CSS |
| **Backend** | Python, Django, Django REST Framework |
| **Async Tasks** | Celery, Redis |
| **Database** | PostgreSQL |
| **AI** | OpenAI GPT-4o (generation + evaluation) |
| **Infrastructure** | Docker, Terraform (AWS) |
| **Monitoring** | Prometheus, Grafana |

## Architecture

```
Browser (Next.js)
    |
    v
Django REST API  --->  Celery Worker  --->  OpenAI GPT-4o
    |                      |
    v                      v
PostgreSQL              Redis (broker)
    |
    v
Prometheus  --->  Grafana
```

1. User submits a consultation order via the frontend
2. Django validates the request, creates the order, and enqueues a Celery task
3. The API returns immediately with a plan ID; the frontend polls for status
4. Celery worker calls GPT-4o to generate the supplement plan asynchronously
5. Once complete, the frontend displays the plan with download and evaluation options

## Source Code

Source code available upon request.
