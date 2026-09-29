# TransformX AI

> **One source, every message.**  
> Transform any source document (research reports, security advisories, PR announcements, policy briefs) into ready-to-publish multi-channel deliverables in parallel, backed by automated cross-channel consistency auditing and hallucination detection powered by Google Gemini.

---

## 🚀 Overview

Modern communications teams, executives, and technical leaders waste hours manually adapting a single source report into emails, executive summaries, LinkedIn posts, slide decks, and tweet threads. 

**TransformX AI** solves this by:
1. **Parallel Transformation**: Producing up to 10 tailored deliverables simultaneously from one source text.
2. **Strict Factual Guardrails**: Preventing AI hallucinations by binding outputs exclusively to source facts, marking informational gaps with `[NEEDS SOURCE]`.
3. **Automated Quality Gate**: Auditing all completed deliverables against the source document and against each other to flag mismatched numbers, dates, contradictory recommendations, or unsupported claims.
4. **Iterative Refinement & Versioning**: Supporting in-place editing, version history branches, and targeted AI adjustments.

---

## ✨ Features

- **Multi-Channel Delivery Matrix**:
  - 💼 **LinkedIn Post**: Scannable hook, takeaway, discussion prompt, hashtags (120–170 words).
  - 🐦 **X / Twitter Thread**: 5–7 numbered tweets formatted for viral engagement (≤280 chars per tweet).
  - ⚡ **Executive Summary**: Situation, business exposure, options, decision, next steps (<250 words).
  - ✉️ **Email Briefing**: Subject line, preheader, 3–5 bullet points, actionable deadline.
  - 📑 **Formal Advisory**: Executive summary, numbered findings, impact analysis, prioritized recommendations.
  - 📽️ **Presentation Deck**: 8 structured slides with key takeaways, visual suggestions, and speaker notes.
  - 📝 **Blog Article**: SEO title, meta description, structured H2/H3 body, key takeaways.
  - 📊 **Infographic Blueprint**: Headline, data points with icon suggestions, layout guidance.
  - 🎬 **Video Package**: 90-second script, storyboard table, narration text, and SRT subtitles.
  - ❓ **FAQ / Q&A Sheet**: Stakeholder objection handling and authoritative source-grounded answers.

- **Real-Time SSE Streaming**: Live character-by-character generation via Server-Sent Events.
- **Deep Quality Gate**:
  - **Consistency Score (0–100)**: Measures numerical, date, and factual alignment across outputs.
  - **Style Alignment Score (0–100)**: Verifies tone, audience fit, and brand voice adherence.
  - **Cross-Output Conflict Detection**: Identifies disagreements between deliverables.
  - **Unsupported Fact Flags**: Detects claims not grounded in the source text.
  - **Actionable Editorial Fixes**: Specific recommendations to resolve inconsistencies.
- **Version History & Editor**: Edit deliverables inline, save versions, switch between raw and rendered preview.
- **1-Click Bundling**: Download single deliverable `.md` files or export a unified multi-channel markdown dossier.
- **Audit Trail**: Real-time timestamped event log of all generations, refinements, audits, and exports.

---

## 🛠️ Tech Stack

- **Backend**: Node.js, Express, TypeScript, [`@google/genai`](https://www.npmjs.com/package/@google/genai) SDK
- **AI Models**: Google Gemini 3.8 Flash (`gemini-3.8-flash`) with automatic fallback resilience (`gemini-3.1-flash-lite`)
- **Frontend**: React 19, TypeScript, Vite 8, Tailwind CSS v4, Lucide React, Motion
- **Architecture**: Unified full-stack application (Express dev server proxying Vite middlewares with direct API routes)

---

## 📦 Prerequisites

- **Node.js**: `v20.x` or `v22.x` (recommended `v22.x`)
- **npm**: `v9.x` or newer
- **Gemini API Key**: Obtainable free from [Google AI Studio](https://aistudio.google.com/)

---

## ⚙️ Setup & Installation

### 1. Clone the repository
```bash
git clone https://github.com/your-username/transformx-ai.git
cd transformx-ai
```

### 2. Install dependencies
```bash
npm install
```

### 3. Configure Environment Variables
Create a `.env` file in the root directory (based on `.env.example`):
```bash
cp .env.example .env
```

Open `.env` and add your Gemini API key:
```env
GEMINI_API_KEY="your_gemini_api_key_here"
PORT=3000
```

> **Note**: Never commit your `.env` file to version control. It is already included in `.gitignore`.

### 4. Start the Development Server
```bash
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) in your browser. The application and API backend will both be live!

---

## 🏗️ Production Build & Deployment

To compile the application for production:

```bash
# 1. Build client bundle
npm run build

# 2. Start production server
npm start
```

The Express server will serve static assets from `dist/` and handle all `/api/*` endpoints.

### Docker Deployment (Optional)

You can containerize TransformX AI with a standard Node.js Dockerfile:

```dockerfile
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:22-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY package*.json ./
RUN npm ci --omit=dev
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/server.ts ./
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 3000
CMD ["node", "--loader", "tsx", "server.ts"]
```

---

## 📡 API Reference

| Endpoint | Method | Description |
| :--- | :--- | :--- |
| `/api/health` | `GET` | Health check and Gemini configuration status |
| `/api/generate` | `POST` | Non-streaming generation for a specific deliverable |
| `/api/generate-stream` | `POST` | Server-Sent Events (SSE) streaming generation |
| `/api/quality-check` | `POST` | Cross-deliverable factual consistency & hallucination audit |
| `/api/refine` | `POST` | Targeted AI refinement of an existing output |

### Example Generation Request

```bash
curl -X POST http://localhost:3000/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "deliverableKey": "linkedin",
    "deliverableTitle": "LinkedIn Post",
    "deliverablePrompt": "Write a 150-word post with hook and CTA",
    "sourceText": "Critical security advisory released for FileBridge v7.4.2...",
    "config": {
      "audience": "Senior decision-makers",
      "tone": "Professional",
      "language": "English",
      "detail": "Medium",
      "purpose": "Awareness"
    }
  }'
```

---

## 🧪 Verification & Testing

To test the application build and linting:

```bash
# Typecheck and linting
npm run lint

# Client production build
npm run build
```

---

## 📄 License

This project is licensed under the [Apache-2.0 License](LICENSE).
