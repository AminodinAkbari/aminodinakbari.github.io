---
title: "AI - Subtitle Translation Microservice"
date: 2024-05-16
description: "An async FastAPI microservice that translates video subtitles between any languages using OpenRouter's Gemini API – with background job tracking and MongoDB persistence."
tags: ["Python", "FastAPI", "OpenRouter", "MongoDB", "Gemini", "LLM", "Docker"]
weight: 3
status: "complete"
featured: false
---

## Overview

A focused, production-ready translation API that accepts WebVTT subtitle files and returns fully translated versions in real time. It leverages **OpenRouter** to access Google's Gemini 2.5 Flash model, ensuring high-quality, context-aware translations. The service processes jobs asynchronously using FastAPI's background tasks, persists status in MongoDB, and guards against duplicate submissions. All components are designed to be containerised with Docker for effortless deployment.

## Key Technical Decisions

- **Structured LLM prompting** – The original subtitle cues are converted into a strict JSON array of `[ID, text]`. The model is instructed to preserve IDs exactly and return only the JSON array, eliminating parsing errors and keeping timestamp alignment intact.
- **Background task design** – Translation requests are enqueued immediately and processed via FastAPI's `BackgroundTasks`. The endpoint returns `202 Accepted` while MongoDB tracks job status (`pending → processing → completed/failed`), enabling polling by the client without blocking.
- **Idempotency check** – Duplicate translation requests for the same video + language pair are rejected if a job is already pending/in‑progress, preventing wasted API calls.
- **Robust error recovery** – Auto‑fixes common JSON formatting issues from the LLM output and logs detailed failure information. ID mismatches trigger immediate job failure to avoid corrupted timestamps.
- **Minimal dependencies** – Uses only `openai`, `fastapi`, `pymongo`, and standard libraries, keeping the service lightweight and easy to maintain.

## Stack

| Layer    | Technology                                |
| -------- | ----------------------------------------- |
| API      | FastAPI (Python)                          |
| Database | MongoDB                                   |
| AI/LLM   | OpenRouter (Gemini 2.5 Flash)             |
| Auth     | (None internal – external only if needed) |
| Deploy   | Docker (assumed)                          |

## What I Learned

Crafting a bullet‑proof prompt that forces the LLM to maintain exact data structures taught me the importance of defensive engineering in AI‑integrated systems. I also deepened my understanding of asynchronous job processing with FastAPI and MongoDB, and learned how to gracefully handle idiosyncratic model outputs (like stray markdown fences) in a production pipeline.
