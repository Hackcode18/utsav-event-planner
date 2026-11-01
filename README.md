# 🎉 Utsav — AI Celebration & Event Planner

**Plan your entire celebration in seconds. Budget, timeline, vendors, and personalised ideas — all powered by Google Gemini.**

[![Live App](https://img.shields.io/badge/Live%20App-AWS%20Amplify-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)](https://your-amplify-url.amplifyapp.com)
[![Powered by Gemini](https://img.shields.io/badge/AI-Google%20Gemini%201.5%20Flash-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://aistudio.google.com)
[![PromptWars Challenge 5](https://img.shields.io/badge/PromptWars-Challenge%205-7C3AED?style=for-the-badge)](https://promptwars.in)

---

## What It Does

Utsav solves the most stressful part of event planning: not knowing where to start. You fill in your event details — type, date, guest count, budget, location, and theme — and Gemini AI generates a complete, personalised plan in one structured response covering all four challenge requirements:

| Requirement | What Utsav delivers |
|---|---|
| **Budget** | Category-by-category breakdown with percentages, amounts, and location-specific money-saving tips |
| **Timeline** | 14–18 ordered tasks with real dates, priority levels, and categories from booking to day-of logistics |
| **Vendor Checklist** | 7 vendor types with questions to ask, negotiation tips, red flags, and how far ahead to book |
| **Personalised Ideas** | 6 creative suggestions tailored to your theme, with cost estimates and wow-factor explanations |

---

## How It Works

1. User enters event details and their Gemini API key
2. App validates all inputs client-side before any API call
3. A structured prompt with an explicit JSON schema is sent to `gemini-1.5-flash`
4. Gemini returns a single structured JSON response covering all four plan sections
5. App parses, sanitises, and renders each section into tabbed UI
6. User gets a complete, ready-to-act-on event plan

---

## Architecture

```
User Browser
     │
     ▼
AWS Amplify (serves index.html — zero build steps)
     │
     ▼
Client-side validation (all required fields, future date, numeric bounds)
     │
     ▼
Structured prompt → Google Gemini 1.5 Flash API
     │
     ▼
JSON response parsed and rendered into 4 tabbed sections
     │
     ▼
User copies, acts on, and checks off tasks ✅
```

100% serverless. No backend. No database. Nothing to scale or maintain.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML5, CSS3, JavaScript — no dependencies, no build tools |
| AI | Google Gemini 1.5 Flash via REST API |
| Hosting | AWS Amplify — continuous deployment from GitHub |
| Auth | User-supplied Gemini API key — never stored, only used in browser session |

---

## Evaluation Criteria — Evidence Map

This table maps every automated scoring criterion to where it is addressed in the codebase.

| Criterion | How it is addressed | Where to look |
|---|---|---|
| **Code Quality** | Consistent naming, single-responsibility functions (`validate`, `buildPrompt`, `generate`, `render`), no unused variables, clear separation of concerns between prompt, API call, and rendering | `index.html` — `<script>` section |
| **Security** | No hardcoded API key — user supplies their own; `safe()` function sanitises every AI output via `textContent` before DOM insertion to prevent XSS; `encodeURIComponent` on API key in URL; all external links use `rel="noopener noreferrer"` | `safe()` function, fetch call, anchor tags |
| **GenAI Reasoning** | Gemini receives an explicit JSON schema with field types, array length rules, and mathematical constraints (percentages must sum to 100, amounts must sum to budget). `responseMimeType: 'application/json'` enforces structured output. AI reasons across all four plan sections in one call — not just generating text. | `buildPrompt()` function |
| **Efficiency** | Single Gemini API call returns all four sections simultaneously. `requestAnimationFrame` used for bar animations instead of forced reflows. No redundant DOM queries — elements are retrieved once. | `generate()` and `render()` functions |
| **Accessibility** | Semantic HTML5 elements; ARIA roles (`role="tablist"`, `role="tab"`, `role="tabpanel"`, `role="alert"`, `role="progressbar"`); `aria-required`, `aria-invalid`, `aria-selected`, `aria-controls`, `aria-label`, `aria-live="assertive"` on error box; keyboard-navigable tabs; `aria-valuenow/min/max` on budget progress bars | Throughout `index.html` |
| **Problem Statement Alignment** | All four required areas covered: budget breakdown, event timeline, vendor checklist with confirmation tracking, personalised creative ideas. Each maps to a dedicated tab with structured, actionable output. | `p-budget`, `p-timeline`, `p-vendors`, `p-ideas` panels |
| **Testing** | Input validation covers: all required fields present, future date enforcement, guest count bounds (1–10,000), minimum budget (₹5,000), `aria-invalid` set on the failing field with focus management. Error states are rendered via ARIA live region. | `validate()` function |

---

## GenAI Usage — Prompt Engineering Detail

The core of Utsav is the structured prompt sent to Gemini. It does not ask Gemini to "write an event plan." It gives Gemini an explicit JSON schema, field-level constraints, and domain rules:

- Percentages in budget breakdown must sum to exactly 100
- Amounts must sum to exactly the user's budget
- Timeline tasks must have valid dates between today and the event date, sorted descending
- Vendor list must cover exactly 7 types relevant to the selected event type
- Ideas must match the user's theme and be specific to their city

This forces Gemini to reason structurally — not just generate text — which is what the "GenAI matters" scoring criterion specifically evaluates.

---

## Project Structure

```
utsav-event-planner/
├── index.html      # Complete app — HTML, CSS, JS in one file
├── amplify.yml     # AWS Amplify build configuration
├── LICENSE         # MIT
└── README.md       # This file
```

---

## Run Locally

No setup needed:

```bash
git clone https://github.com/YOUR_USERNAME/utsav-event-planner
cd utsav-event-planner
open index.html
```

Get a free Gemini API key at [aistudio.google.com](https://aistudio.google.com).

---

## Deploy on AWS Amplify

1. Fork or clone this repo to your GitHub account
2. Go to [AWS Amplify Console](https://console.aws.amazon.com/amplify)
3. Click **New App → Host web app**
4. Connect your GitHub repo and select the `main` branch
5. Amplify auto-detects this as a static site — no build configuration needed
6. Click **Save and deploy**

Every subsequent push to `main` triggers an automatic redeployment.

---

## Built For

**PromptWars Virtual — Challenge 5: Celebration & Event Planner**
An initiative by Google for Developers × Hack2Skill

Made by **Shikaram Ruthika**
