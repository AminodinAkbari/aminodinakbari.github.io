---
title: "Payment Service"
date: 2024-10-02
description: "A robust microservice that wraps Stripe, manages subscriptions, and exposes a clean REST API with secure cookie-based authentication."
tags:
  ["Python", "FastAPI", "Stripe", "MongoDB", "Redis", "JWT", "Docker", "Linux"]
weight: 1

# Custom fields for project cards
status: "complete"
featured: true
---

## Overview

An internal payment layer built with FastAPI that handles all Stripe interactions: customer creation, checkout sessions, subscription management, and webhook processing. It persists user subscription states in MongoDB so other services can query status without directly calling Stripe.

## Key Technical Decisions

- **Layered Stripe API** – abstracts Stripe SDK calls behind service-level functions, keeping business logic clean and testable.
- **Dual authentication** – endpoints that redirect users (e.g., checkout) rely on cookies with automatic token refresh and fallback to login; all other endpoints require a header-based JWT.
- **Webhook-driven updates** – Stripe events (subscription changes, invoices, coupons) update the local database immediately, ensuring other services always see the latest state.
- **Dockerised from day one** – single `docker-compose up` brings up the API and its MongoDB instance, using environment variables for all secrets.

## Stack

| Layer    | Technology       |
| -------- | ---------------- |
| API      | FastAPI (Python) |
| Database | MongoDB          |
| Cache    | Redis            |
| Payment  | Stripe SDK       |
| Auth     | JWT              |
| Deploy   | Docker           |

## What I Learned

Implementing secure webhook verification and idempotency for Stripe events taught me a lot about atomicity in callback‑based architectures. I also deepened my understanding of cookie‑based token refresh without violating REST statelessness.
