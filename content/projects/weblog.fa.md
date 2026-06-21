---
title: "SudoExplain – پلتفرم فول استک وبلاگ (پروژه وبلاگ شخصی خودم)"
date: 2026-06-17
description: "یک پلتفرم وبلاگ‌نویسی مدرن و دو زبانه با پشتیبانی از چند نویسنده، جستجوی full‑text، نظرات تو در تو و یک داشبورد امن – ساخته‌شده با Django REST Framework و React."
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
link: https://test/com
status: "تکمیل شده"
featured: true
---

## مرور کلی

**Sudo Explain** یک پلتفرم وبلاگ‌نویسی کاملاً سفارشی و آماده production است که برای نویسندگان فنی طراحی شده است. این پلتفرم دارای یک **رابط کاربری دو زبانه (انگلیسی/فارسی)**، یک داشبورد مدیریتی غنی برای نویسندگان، نظرات تو در تو (threaded comments) با قابلیت لایک و قابلیت‌های جستجوی قدرتمند می‌باشد. بک‌اند یک API مبتنی بر Django REST Framework با جستجوی full‑text PostgreSQL و تسک‌های Celery با Redis است و فرانت‌اند یک SPA با React و Tailwind CSS با تم دارک/لایت الهام‌گرفته از ترمینال می‌باشد. کل پروژه با Docker containerized شده و برای deployment آماده است.

این وبلاگ یک قابلیت Async نیز دارد که در Background به یک هوش مصنوعی متصل می باشد تا خلاصه متن پست را دریافت کرده و در دیتابیس ذخیره می کند.

## تصمیمات فنی کلیدی

- **جستجوی full‑text با trigram similarity** – محتوا و عناوین پست‌ها با استفاده از GIN indexes PostgreSQL و `TrigramSimilarity` برای جستجوی مقاوم به اشتباه تایپی ایندکس شده‌اند. نتایج جستجو با ترکیبی از `SearchRank` و trigram similarity رتبه‌بندی می‌شوند و نتایج سریع و مرتبط را ارائه می‌دهند.
- **پشتیبانی از محتوای دو زبانه** – ترجمه‌های فارسی در فیلدهای جداگانه (`content_fa`, `title_fa`) ذخیره می‌شوند. یک toggle ساده در صفحه جزئیات پست زبان نمایش داده شده را تغییر می‌دهد؛ در صورت نبود ترجمه، دکمه غیرفعال می‌شود.
- **داشبورد نویسنده با soft‑delete** – کاربران staff می‌توانند پست‌های خود را از طریق یک داشبورد سفارشی ایجاد، ویرایش و مدیریت کنند. پست‌های حذف‌شده به وضعیت “deleted” منتقل می‌شوند نه اینکه به‌طور دائمی حذف شوند، که ایمنی داده‌ها را تضمین می‌کند.
- **سیستم نظرات تو در تو با جلوگیری از سوءاستفاده** – نظرات از nesting نامحدود پشتیبانی می‌کنند و یک سیستم permission سفارشی (حداکثر ۲ کامنت برای هر کاربر در هر پست، زمان‌های timeout ویرایش) از آن‌ها پشتیبانی می‌کند. دکمه لایک از DRF throttling (۱۰ درخواست در دقیقه) و constraintهای دیتابیس برای جلوگیری از اسپم استفاده می‌کند.
- **احراز هویت JWT با silent refresh** – توکن‌های access/refresh در `localStorage` ذخیره می‌شوند. یک interceptor در Axios به‌طور خودکار access token را هنگام دریافت خطای 401 رفرش می‌کند و کاربر را بدون وقفه در حالت لاگین نگه می‌دارد.
- **middleware سفارشی CORS و Content‑Security‑Policy** – یک middleware سبک CORS درخواست‌های preflight را مدیریت می‌کند و origin توسعه React را مجاز می‌سازد، در حالی که یک middleware CSP در برابر حملات XSS محافظت می‌کند. هر دو بدون پکیج‌های خارجی و برای کنترل حداکثری ساخته شده‌اند.
- **orchestration با Docker‑Compose** – بک‌اند (Django + Gunicorn)، فرانت‌اند (Nginx)، PostgreSQL و Redis همگی با یک دستور `docker‑compose up` مدیریت می‌شوند.

## فناوری ها

| لایه        | فناوری                                  |
| ----------- | --------------------------------------- |
| API         | Django REST Framework (Python)          |
| فرانت‌اند   | React (Vite) + Tailwind CSS             |
| پایگاه داده | PostgreSQL                              |
| کش/Broker   | Redis (Celery)                          |
| احراز هویت  | JWT (Simple JWT)                        |
| جستجو       | PostgreSQL Full‑Text Search + Trigram   |
| DevOps      | Docker, Docker Compose, Gunicorn, Nginx |
| AI/LLM      | Openrouter (GPT 4o mini)                |

## آموخته‌ها

ساخت این وبلاگ به‌صورت end‑to‑end به من آموخت که چگونه یک پلتفرم امن و چندنویسنده را از پایه طراحی کنم. من تجربه عملی با full‑text search indexes PostgreSQL، به‌روزرسانی atomic شمارش بازدیدها و احراز هویت مبتنی بر token با silent refresh را کسب کردم. پیاده‌سازی ویژگی‌هایی مانند محتوای دو زبانه و یک رابط کاربری ترمینالی جالب، اهمیت UX تمیز و accessibility را تقویت کرد. در نهایت، containerized کردن کل برنامه درک من را از Docker و orchestration چندسرویسی مستحکم کرد — مهارت‌هایی که مستقیماً به محیط‌های production قابل انتقال هستند.
