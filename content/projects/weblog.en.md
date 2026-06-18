---
title: "SudoExplain – Full‑Stack Blog Platform (Personal Weblog)"
date: 2026-06-17
description: "A modern bilingual blog with multi‑author support, full‑text search, nested comments, and a secure dashboard – built with Django REST Framework and React."
tags:
  [
    "Python",
    "Django",
    "DRF",
    "React",
    "Tailwind CSS",
    "PostgreSQL",
    "Redis",
    "JWT",
    "Docker",
    "Vite",
    "OpenRouter",
    "GPT",
    "LLM",
  ]
weight: 4
status: "complete"
featured: true
---

## Overview

**ByteAndWords** is a fully custom, production‑ready blogging platform designed for technical writers. It features a **bilingual (English/Persian) interface**, a rich admin dashboard for authors, threaded comments with likes, and robust search capabilities. The backend is a Django REST Framework API with PostgreSQL full‑text search and Redis‑powered Celery tasks, while the frontend is a React SPA with Tailwind CSS and a terminal‑inspired dark/light theme. The entire project is containerised with Docker and ready for deployment.

This blog also has an Async feature that connects to an AI in the background to receive the post summary and store it in the database.

## Key Technical Decisions

- **Full‑text search with trigram similarity** – Post content and titles are indexed using PostgreSQL GIN indexes and `TrigramSimilarity` for typo‑tolerant searching. Search results are ranked by a combination of `SearchRank` and trigram similarity, delivering fast and relevant results.
- **Bilingual content support** – Persian translations are stored in separate fields (`content_fa`, `title_fa`). A simple toggle in the post detail page switches the displayed language; the button is disabled if no translation exists.
- **Author dashboard with soft‑delete** – Staff users can create, edit, and manage their posts through a custom dashboard. Deleted posts are moved to a “deleted” status rather than permanently removed, ensuring data safety.
- **Threaded comment system with abuse prevention** – Comments support unlimited nesting and are backed by a custom permission system (max 2 comments per user per post, edit timeouts). A like button uses DRF throttling (10 requests/minute) and database constraints to prevent spam.
- **JWT authentication with silent refresh** – Access/refresh tokens are stored in `localStorage`. An Axios interceptor automatically refreshes the access token when a 401 is detected, keeping the user logged in seamlessly.
- **Custom CORS and Content‑Security‑Policy middleware** – A lightweight CORS middleware handles preflight requests and allows the React dev origin, while a CSP middleware protects against XSS attacks. Both were built without external packages for maximum control.
- **Docker‑Compose orchestration** – The backend (Django + Gunicorn), frontend (Nginx), PostgreSQL, and Redis are all managed by a single `docker‑compose up` command.

## Stack

| Layer        | Technology                              |
| ------------ | --------------------------------------- |
| API          | Django REST Framework (Python)          |
| Frontend     | React (Vite) + Tailwind CSS             |
| Database     | PostgreSQL                              |
| Cache/Broker | Redis (Celery)                          |
| Auth         | JWT (Simple JWT)                        |
| Search       | PostgreSQL Full‑Text Search + Trigram   |
| DevOps       | Docker, Docker Compose, Gunicorn, Nginx |
| AI/LLM       | Openrouter (GPT 4o mini)                |

## What I Learned

Building this blog end‑to‑end taught me how to design a secure, multi‑author platform from scratch. I gained hands‑on experience with PostgreSQL full‑text search indexes, atomic view‑count updates, and token‑based authentication with silent refresh. Implementing features like bilingual content and a geeky terminal UI reinforced the importance of clean UX and accessibility. Finally, containerising the entire application solidified my understanding of Docker and multi‑service orchestration – skills that are directly transferable to production environments.
