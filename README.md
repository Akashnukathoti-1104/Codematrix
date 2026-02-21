# StyleSense — AI Fashion Recommendation System
### Generative AI · 24-Hour Build Plan

---

## ⚡ Quick Start (5 minutes)

```bash
# 1. Clone / unzip your files
# 2. Install Python deps
pip install -r requirements.txt

# 3. Create .env from example
cp .env.example .env
# → Fill in your API keys

# 4. Start backend
python main.py
# → Running at http://localhost:8000

# 5. Open frontend
# → Open index.html in browser (or serve via VS Code Live Server)
# → Go to ⚙ Setup tab and enter http://localhost:8000
```

---

## 🗺️ 24-Hour Development Phases

| Phase | Hours | Tasks | APIs Used |
|-------|-------|-------|-----------|
| **1 · Setup** | 0–4h | FastAPI skeleton, CORS, health, .env, folder structure | — |
| **2 · LLM Endpoints** | 4–10h | `/recommendations`, `/trends`, `/occasion-outfit`, `/chat` | Groq |
| **3 · Vision Analysis** | 10–17h | `/analyze-outfit`, HuggingFace classifier, image upload | HuggingFace |
| **4 · Image Generation** | 17–22h | `/generate-outfit-image`, Together AI FLUX, async loading | Together AI |
| **5 · Polish & Deploy** | 22–24h | Error handling, loading states, deploy to Render/Railway | — |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────┐
│            Frontend (index.html)        │
│  • 6 sections: Home/Recommend/Analyze/  │
│    Trends/Occasion/Chat                 │
│  • Direct Together AI calls for images  │
│  • Fallback: calls Groq directly if     │
│    backend is offline                   │
└──────────────┬──────────────────────────┘
               │ HTTP
┌──────────────▼──────────────────────────┐
│         FastAPI Backend (main.py)       │
│  POST /api/recommendations  → Groq LLM  │
│  GET  /api/trends           → Groq LLM  │
│  POST /api/occasion-outfit  → Groq LLM  │
│  POST /api/chat             → Groq LLM  │
│  POST /api/analyze-outfit   → HF + Groq │
│  POST /api/generate-outfit-image → FLUX │
└──────────────┬──────────────────────────┘
               │
    ┌──────────┼──────────┐
    ▼          ▼          ▼
  Groq      HuggingFace  Together AI
 (LLM)     (Classifier)  (FLUX Images)
```

---

## 🔑 API Keys Needed

| Service | Get Key At | Cost |
|---------|-----------|------|
| **Groq** | console.groq.com | Free tier (600 req/min) |
| **HuggingFace** | huggingface.co/settings/tokens | Free |
| **Together AI** | api.together.xyz | Free $25 credits on signup |

---

## 📁 File Structure

```
stylesense/
├── index.html          ← Full frontend (single file)
├── main.py             ← FastAPI backend
├── requirements.txt    ← Python dependencies
├── .env.example        ← API keys template
└── .env                ← Your actual keys (git-ignored)
```

---

## 🖼️ Image Generation Flow

The frontend generates outfit images directly from the browser via Together AI (no backend round-trip needed):

```javascript
// In index.html — generateOutfitImage()
fetch('https://api.together.xyz/v1/images/generations', {
  headers: { Authorization: `Bearer ${TOGETHER_KEY}` },
  body: JSON.stringify({
    model: 'black-forest-labs/FLUX.1-schnell-Free',  // FREE model
    prompt: `Fashion editorial photography, ${outfitDescription}, studio white background`,
    width: 512, height: 768, steps: 4
  })
})
```

Images appear progressively — cards show skeleton loaders while images generate (~5–15s each).

---

## 🚀 Deploy to Render (free)

1. Push to GitHub
2. New Web Service → connect repo
3. Build command: `pip install -r requirements.txt`
4. Start command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Add env vars from `.env`

---

## 🎯 Demo Script (for judges)

1. Open → **Home** — show the hero capabilities card
2. Go to **⚙ Setup** — show API key config (use your real keys)
3. Go to **Recommend** — select preferences → Generate
   - Show skeleton loading → then image cards appearing one by one
4. Go to **Analyze** — upload a real outfit photo → analyze
   - Show style score, detected items, AI alternative suggestions
5. Go to **Trends** — show seasonal trend report with visuals
6. Go to **Occasion** — pick "Date Night" → generate with image
7. Go to **Stylist Chat** — live Q&A

---

## 💡 Key Technical Decisions

- **FLUX.1-schnell-Free** — 4-step diffusion, fastest free model, good quality
- **Llama 3.3 70B on Groq** — fastest LLM inference available, free tier
- **JSON mode** — all Groq calls use `response_format: json_object` for reliable parsing
- **Graceful degradation** — frontend falls back to direct Groq API if backend is down
- **Progressive image loading** — skeleton → real image, no blocking UX
