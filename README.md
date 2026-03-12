# 🟢 FMHY Agent

> An agentic AI-powered web app that searches the [freemediaheckyeah](https://fmhy.net/) directory and automatically opens the best free resource for whatever you're looking for.

---

## 🧠 What It Does

You type something natural — *"I want to watch Inception for free"* or *"download Minecraft"* or *"learn Python for free"* — and the agent:

1. **Parses your intent** using an LLM (gemini - free tier)
2. **Classifies the category** (movies, games, books, music, courses, software, AI tools, etc.)
3. **Searches the FMHY index** (scraped + cached from fmhy.net) for matching resources
4. **Ranks results** by FMHY star rating and community trust score
5. **Auto-opens the top result** in a new tab while showing you alternatives

---

## 🗂️ Project Structure

```
fmhy-agent/
│
├── index.html                  # Entry point
│
├── src/
│   ├── app.js                  # App bootstrap & event wiring
│   │
│   ├── agent/
│   │   ├── index.js            # Main agent orchestrator
│   │   ├── intent-parser.js    # NLP: extract intent + query from user input
│   │   ├── category-router.js  # Maps parsed intent → FMHY category
│   │   └── result-ranker.js    # Scores & sorts results by trust/stars
│   │
│   ├── data/
│   │   ├── fmhy-index.js       # Curated FMHY resource database (categories + sites)
│   │   ├── categories.js       # Category definitions, icons, keywords
│   │   └── trusted-domains.js  # Allow-list of FMHY-starred trusted domains
│   │
│   ├── scraper/
│   │   ├── fmhy-scraper.js     # Fetches + parses fmhy.net pages (client-side via proxy)
│   │   └── cache.js            # LocalStorage cache layer (TTL: 24h)
│   │
│   ├── ui/
│   │   ├── search-bar.js       # Search input component + keyboard shortcuts
│   │   ├── results-grid.js     # Renders site cards grid
│   │   ├── category-browser.js # Category tiles for browsing
│   │   └── toast.js            # Auto-open notification + countdown
│   │
│   └── styles/
│       └── main.css            # All styles (dark theme, grid, animations)
│
├── server/                     # Optional lightweight backend
│   ├── index.js                # Express server (CORS proxy for fmhy.net)
│   ├── routes/
│   │   ├── search.js           # GET /api/search?q= — proxied FMHY search
│   │   └── fetch-page.js       # GET /api/page?url= — scrapes a specific FMHY page
│   └── middleware/
│       └── rate-limit.js       # Basic rate limiting
│
├── .env.example                # API keys template
├── package.json
└── README.md
```

---

## ⚙️ Tech Stack

| Layer | Tech | Why |
|---|---|---|
| **Frontend** | Vanilla JS (ES Modules) | Zero build step, fast, portable |
| **AI / Intent Parsing** | gemini api(free tier) | Natural language → structured intent |
| **FMHY Data** | Scraped + static JSON cache | fmhy.net doesn't have a public API |
| **Proxy Server** | Node.js + Express | Bypass CORS when fetching fmhy.net live |
| **Caching** | LocalStorage (browser) + in-memory (server) | Avoid hammering fmhy.net |
| **Styling** | Plain CSS (no framework) | Lightweight, custom dark theme |

---

## 🤖 How the AI Agent Works

```
User Input
    │
    ▼
┌─────────────────────────────────┐
│  intent-parser.js               │
│  → calls gemini api            │
│  → returns { intent, query,     │
│              category, keywords }│
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  category-router.js             │
│  → maps category to FMHY page  │
│  → loads relevant sites from   │
│    fmhy-index.js                │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  result-ranker.js               │
│  → scores by: star rating,     │
│    keyword match, trust score  │
│  → returns ranked list         │
└────────────┬────────────────────┘
             │
             ▼
┌─────────────────────────────────┐
│  results-grid.js + toast.js    │
│  → renders cards               │
│  → auto-opens #1 result        │
│    after 3s countdown          │
└─────────────────────────────────┘
```

---

## 🚀 Getting Started

### 1. Clone & Install

```bash
git clone https://github.com/yourname/fmhy-agent.git
cd fmhy-agent
npm install
```

### 2. Set Up Environment

```bash
cp .env.example .env
```

Fill in `.env`:

```
GEMINI_API_KEY=your_key_here
PORT=3000
```

### 3. Run

```bash
# Start the proxy server
npm run server

# Open index.html in browser (or use live-server)
npx live-server .
```

---

## 🌐 FMHY Categories Covered

| Category | FMHY Page | Auto-opens? |
|---|---|---|
| 🎬 Movies / TV | fmhy.net/video | ✅ |
| ⛩️ Anime | fmhy.net/video#anime | ✅ |
| 🎵 Music | fmhy.net/audio | ✅ |
| 📚 Books / Ebooks | fmhy.net/reading | ✅ |
| 🎮 Gaming | fmhy.net/gaming | ✅ |
| 🎓 Courses | fmhy.net/educational | ✅ |
| 💾 Software | fmhy.net/downloading | ✅ |
| 🤖 AI Tools | fmhy.net/ai | ✅ |
| 🛡️ Privacy / VPN | fmhy.net/privacy | ✅ |
| 📱 Mobile Apps | fmhy.net/mobile | ✅ |

---

## 🔒 Legal / Disclaimer

- This project is a **navigator/index tool** — it does not host, store, or serve any copyrighted content.
- All links point to third-party sites listed on [fmhy.net](https://fmhy.net/).
- Users are responsible for complying with the laws in their jurisdiction.
- FMHY itself notes: *"This site does not host any files."*

---

## 🙏 Credits

- [freemediaheckyeah](https://fmhy.net/) — the community directory this project is built on
- [Anthropic Claude](https://anthropic.com) — for intent parsing
