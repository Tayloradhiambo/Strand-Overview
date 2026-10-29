# Strand — *Guess Less*

**AI-powered hairstyle identification, built for African hair.**

> This is a public overview of Strand. The production codebase and dataset are private.

\---

## The problem

You see a hairstyle you love — on Instagram, on the street, on a friend — and you can't name it. You walk into a salon, show a blurry screenshot, and hope the stylist interprets it the same way you do. Prices, time, and results vary because nobody is speaking the same language.

For braided and protective styles, the problem is worse: there are dozens of distinct styles with overlapping names, and mainstream AI tools weren't built with them in mind.

## What Strand does

Upload a photo → Strand identifies the hairstyle, gives you its name, and helps you ask for exactly what you want.

Current taxonomy: **Creating a database** defined with input from working stylists.

## Why this matters: a real AI bias problem

While building the MVP on a leading general-purpose vision model, I found that the information given is inaccurate and generalised, not just misclassifying the style but failing to process the image.

This is a familiar pattern in AI systems: models trained on data that under-represents some groups perform worse for those groups, and the failure is invisible until someone from that group tests it. Strand's response is to build the data that's missing.

## Approach

**Phase 1 — MVP on a third-party vision model.** Ship fast, learn where general-purpose models fail.

**Phase 2 — Proprietary dataset (in progress).** Partnering with salon-based hairstylists in Nairobi who upload the styles they do daily, labelled against the Strand taxonomy. The dataset is the core asset; the model layer is designed to be swappable.

**Phase 3 — Trends.** Show which styles are trending where and when.

## Architecture

```
┌──────────────────────┐        ┌───────────────────────────────┐
│  React + Vite web app │ ─────▶ │  Supabase Edge Functions       │
│  (Cloudflare Pages)   │        │   • identify-style             │
└──────────────────────┘        │   • submit-feedback            │
                                └──────────────┬────────────────┘
                                               │
                     ┌─────────────────────────┼──────────────────────┐
                     ▼                         ▼                      ▼
             Vision model (Gemini,     Supabase Postgres       Supabase Storage
             swappable via config)     (styles, feedback)      (images)
```

* **Frontend:** React + Vite, auto-deployed to Cloudflare Pages
* **Backend:** Supabase (Postgres, file storage, Edge Functions)
* **AI:** Gemini vision model, kept server-side and swappable, so the product isn't locked to one provider
* **Feedback loop:** users correct wrong identifications, which feeds the dataset

## Status

Early stage, and honest about it. Identification accuracy on general-purpose models is not yet good enough, which is exactly why the dataset is the current focus. Public launch planned in Kenya.

## Screenshots

!\[Strand homepage: upload a photo, get the style name, type, size and length](strand-homepage.png)

*Strand identifies a style from a photo and returns its name, attributes, and a reminder to confirm details with your stylist.*

## Built by

**Taylor Adhiambo**: actuarial analyst and developer, Nairobi.
[LinkedIn](#) · [Website](#)

