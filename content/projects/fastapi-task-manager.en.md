---
title: "FastAPI Task Manager"
date: 2024-03-01
description: "A production-ready REST API for task management with async support, JWT auth, and PostgreSQL."
tags: ["Python", "FastAPI", "PostgreSQL", "Docker", "Redis"]
weight: 1

# Custom fields for project cards
projectUrl: "https://github.com/AminodinAkbari/fastapi-task-manager"
status: "complete" # complete | in-progress | archived
featured: true
---

## Overview

A production-ready task management API built with FastAPI and PostgreSQL.
Designed for high-concurrency workloads using async SQLAlchemy and background tasks via Redis.

## Key Technical Decisions

- **Async-first**: all DB operations use `asyncpg` to avoid blocking the event loop
- **JWT auth**: stateless authentication with refresh token rotation
- **Docker Compose**: single-command local setup for API + Postgres + Redis
- **Alembic**: schema migrations with rollback support

## Stack

| Layer    | Technology        |
| -------- | ----------------- |
| API      | FastAPI 0.110     |
| Database | PostgreSQL 16     |
| Cache    | Redis 7           |
| Auth     | JWT (python-jose) |
| Deploy   | Docker + Compose  |

## What I Learned

Handling connection pool exhaustion under load — solved by tuning `asyncpg` pool size
and adding a circuit breaker pattern for the Redis cache layer.
