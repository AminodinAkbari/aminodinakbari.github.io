---
title: "Jobinja Web Scraper"
date: 2024-02-14
description: "A Python web scraper that authenticates on Jobinja.ir, extracts job listings, exports them to CSV, and presents insights on a dashboard – built with BeautifulSoup, Pandas, and GitLab CI/CD."
tags:
  [
    "Python",
    "BeautifulSoup",
    "Requests",
    "Pandas",
    "Web Scraping",
    "GitLab CI/CD",
    "Dashboard",
  ]
weight: 5
status: "complete"
featured: false
---

## Overview

A job market analysis tool that scrapes **Jobinja**, a leading Iranian job portal. It logs in through session-based cookies and CSRF token handling, then scrapes job listing pages. The extracted data is cleaned, exported to CSV, and fed into a dashboard for visualisation. The entire workflow is version-controlled on GitLab with CI/CD for scheduled or push-triggered runs.

## Key Technical Decisions

- **Session-based authentication** – `requests.Session` retains cookies across requests, while `login.py` fetches the CSRF token from the login form to mimic a real browser sign‑in.
- **Modular architecture** – Login, page scraping, data processing, and dashboarding are separated into dedicated modules (`login.py`, `pages.py`, `data.py`, `dashboard.py`), making maintenance and testing straightforward.
- **Data pipeline** – Raw HTML is parsed with BeautifulSoup, structured into fields (title, company, location, etc.), validated with Pandas, and then saved as CSV.
- **CI/CD integration** – GitLab CI/CD automates scraping runs, ensuring fresh data without manual intervention.

## Stack

| Layer           | Technology                      |
| --------------- | ------------------------------- |
| Scraping        | Python, Requests, BeautifulSoup |
| Data handling   | Pandas, CSV                     |
| Visualisation   | Dashboard (Matplotlib/Seaborn)  |
| Automation      | GitLab CI/CD, Cron (assumed)    |
| Version control | GitLab                          |

## What I Learned

Working with this website structures taught me to how to scrap data from a dashboard which needs a login. Implementing a robust login flow with CSRF tokens deepened my understanding of session security. Finally, automating the whole pipeline with GitLab CI/CD gave me practical experience in deploying scrapers as recurring data services.
