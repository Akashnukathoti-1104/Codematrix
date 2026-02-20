# StyleSense 👗✨
### Generative AI–Powered Fashion Recommendation System

A full-stack fashion intelligence platform using **FastAPI**, **Gemini**, **Groq**, and **Hugging Face**.

---

## 🚀 Quick Start (5 minutes)

### 1. Clone & Install
```bash
git clone <your-repo>
cd stylesense
pip install -r requirements.txt
```

### 2. Set Up API Keys
```bash
cp .env.example .env
# Edit .env and add your keys:
```

| Key | Where to get it |
|-----|----------------|
| `GEMINI_API_KEY` | https://aistudio.google.com/ (free) |
| `GROQ_API_KEY` | https://console.groq.com/ (free) |
| `HF_API_KEY` | https://huggingface.co/settings/tokens (free) |

### 3. Run
```bash
uvicorn main:app --reload --port 8000
```

### 4. Open in Browser
```
http://localhost:8000
```

---

## 📁 Project Structure

```
stylesense/
├── main.py                          # FastAPI app entry point
├── requirements.txt
├── .env.example                     # API key template
├── backend/
│   ├── routes/
│   │   ├── recommendations.py       # /api/recommend, /api/trends, /api/chat
│   │   └── image_analysis.py        # /api/analyze-image
│   └── services/
│       ├── gemini_service.py        # Gemini 1.5 Flash (vision + text)
│       ├── groq_service.py          # Groq LLaMA3 (fast text AI)
│       └── huggingface_service.py   # HF CLIP + color detection
└── frontend/
    └── index.html                   # Complete single-file UI
```

---

## 🌐 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/recommend` | Personalized outfit recommendations |
| `POST` | `/api/analyze-image` | Upload & analyze outfit photo |
| `GET`  | `/api/trends` | Current fashion trend report |
| `POST` | `/api/occasion-outfit` | Outfit for specific occasion |
| `POST` | `/api/chat` | Conversational style assistant |
| `GET`  | `/health` | Health check |

---

## 🤖 AI Stack

| Feature | Technology |
|---------|-----------|
| Image analysis & outfit generation | **Gemini 1.5 Flash** |
| Fast recommendations & chat | **Groq + LLaMA3-8B** |
| Clothing classification | **HuggingFace CLIP** |
| Color extraction | **Pillow (PIL)** |

---

## 🎨 Features

- **Home** — Animated landing with capability overview
- **Recommend** — Style preference form → 5 personalized outfit cards
- **Analyze** — Drag & drop image → Gemini vision analysis + color palette
- **Trends** — AI-generated current trend report with season color
- **Occasion** — Pick an event → complete outfit with pieces & budget
- **Stylist Chat** — Multi-turn conversational AI style advisor

---

## 🛠 Troubleshooting

**HuggingFace 503 error** → Model is cold-starting. Wait 20s and retry.

**Gemini quota error** → You've hit the free tier limit. Wait or use a different key.

**CORS error in browser** → Make sure backend is running on `localhost:8000`.

**Slow responses** → Groq is fastest; Gemini can take 3-8 seconds for images.

---

## 📝 License
MIT — Built for hackathon. Good luck! ✨
