---
title: "Affiliate Service"
date: 2024-10-02
description: "An affiliate marketing backend that handles referral links, click tracking, commission logic, and Stripe webhooks — built for scalability with MongoDB and Redis."
tags:
  ["Python", "FastAPI", "Stripe", "MongoDB", "Redis", "JWT", "Docker", "Linux"]
weight: 2
status: "complete"
featured: true
---

## Overview

The Affiliate Service is the engine behind referral-based campaigns. It lets influencers generate personalised links, tracks every click/sign-up/purchase in real time, and calculates commissions automatically. Stripe webhooks keep the referral stats consistent, while Redis-powered rate limiting protects the system from click spam. The service integrates with a centralized authentication layer so users can sign up and log in seamlessly across the product ecosystem.

## Key Technical Decisions

- **Webhook-driven referral attribution** – When a user signs up via a payment service, the affiliate code is stored in Stripe customer metadata. an event from stripe webhooks hits this service, which then increments the correct influencer’s sign-up statistics — no polling, no race conditions.
- **Rate limiting with Redis** – All referral link clicks are throttled (for example in our test environment it's 2 clicks per IP per 10 minutes, I can not say what is the rate limit in production) using Redis counters. This prevents bots from artificially inflating numbers without adding latency for real users.
- **Dual-role dashboard support** – Separate logic for publishers (influencers) and marketers, though the marketer side is currently out of scope. The data model is built to scale when that role is reintroduced.
- **Centralised authentication** – User sign-up/login is delegated to a dedicated auth service. The affiliate service only consumes verified JWT tokens, keeping authentication concerns completely decoupled.
- **Dockerised monorepo-friendly** – A single `docker-compose up` spins up the app and its dedicated MongoDB instance, making local development and deployment predictable.

## Stack

| Layer    | Technology        |
| -------- | ----------------- |
| API      | FastAPI (Python)  |
| Database | MongoDB           |
| Cache    | Redis             |
| Payments | Stripe SDK        |
| Auth     | JWT (centralized) |
| Deploy   | Docker            |

## What I Learned

Designing the referral‑tracking pipeline over Stripe webhooks taught me how critical idempotency and event ordering are in asynchronous systems. I also gained hands‑on experience with Redis‑based rate limiting as a lightweight anti‑abuse layer, and saw how a properly decoupled auth service simplifies multi‑service architectures.
