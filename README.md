# VerboDrill 🇪🇸
### AI-Powered Spanish Verb Trainer

An audio-lingual Spanish verb conjugation trainer powered by Google Gemini AI. Built for active oral retrieval drills — every card gives you a natural English prompt, you produce the Spanish conjugation, then Gemini explains the grammar in plain English.

---

## What It Does

- Drills **20 core Spanish verbs** across **4 tenses** and **6 pronouns** — 480 total conjugations
- All irregular forms are **hardcoded** (no algorithmic guessing): `hice`, `fui`, `voy`, `sé`, `digo`, `hizo`, `vine`, `saldré`, and more
- After every answer, **Gemini AI explains** the grammar — why it's irregular, what rule applies, and where you went wrong if you missed it
- Tracks your **streak** and **session score** in real time
- Works for **typed practice** or **spoken/oral drills** via the "Spoke It" button

---

## The 20 Verbs

| # | Infinitive | English |
|---|-----------|---------|
| 1 | ser | to be (permanent) |
| 2 | estar | to be (temporary) |
| 3 | tener | to have |
| 4 | hacer | to do / make |
| 5 | ir | to go |
| 6 | poder | to be able to / can |
| 7 | saber | to know (facts) |
| 8 | decir | to say / tell |
| 9 | querer | to want / love |
| 10 | pasar | to pass / happen |
| 11 | deber | to owe / must / should |
| 12 | llegar | to arrive |
| 13 | creer | to believe / think |
| 14 | encontrar | to find / meet |
| 15 | hablar | to speak / talk |
| 16 | llevar | to carry / wear / take |
| 17 | quedar | to stay / remain / meet up |
| 18 | venir | to come |
| 19 | pensar | to think / plan |
| 20 | salir | to leave / go out |

## The 4 Tenses

| Tense | Example (hablar, yo) | Usage |
|-------|---------------------|-------|
| **Presente** | hablo | Current / habitual actions |
| **Pretérito** | hablé | Completed past actions |
| **Imperfecto** | hablaba | Ongoing / habitual past actions |
| **Futuro** | hablaré | Future actions |

---

## Project Structure

```
verbodrill/
├── server.js          # Express server — proxies Gemini API calls securely
├── package.json       # Dependencies and start script
├── .gitignore
├── README.md
└── public/
    └── index.html     # Complete single-file frontend (HTML + CSS + JS)
```

**Why a server?** Your Gemini API key lives in a Railway environment variable and never touches the browser. The frontend calls `/api/chat` on your own server, which proxies the request to Google's API.

---

## Local Development

### Prerequisites
- Node.js 18 or higher
- A Gemini API key ([get one free at aistudio.google.com](https://aistudio.google.com))

### Setup

```bash
# 1. Install dependencies
npm install

# 2. Set your API key (Mac/Linux)
export GEMINI_API_KEY=your_key_here

# Windows (Command Prompt)
set GEMINI_API_KEY=your_key_here

# Windows (PowerShell)
$env:GEMINI_API_KEY="your_key_here"

# 3. Start the server
npm start

# 4. Open in browser
# → http://localhost:3000
```

---

## Deploy to Railway

### Step 1 — Push to GitHub

```bash
git init
git add .
git commit -m "Initial VerboDrill commit"
git remote add origin https://github.com/YOUR_USERNAME/verbodrill.git
git push -u origin main
```

### Step 2 — Create Railway Project

1. Go to [railway.app](https://railway.app) and sign in
2. Click **New Project**
3. Choose **Deploy from GitHub Repo**
4. Select your `verbodrill` repository
5. Railway auto-detects Node.js and runs `npm start`

### Step 3 — Add Your API Key

1. In your Railway service, click the **Variables** tab
2. Click **New Variable**
3. Set: `GEMINI_API_KEY` = `your_gemini_api_key_here`
4. Railway automatically redeploys with the new variable

### Step 4 — Generate a Public URL

1. Go to your service's **Settings** tab
2. Under **Networking**, click **Generate Domain**
3. Your app is live at something like:
   `https://verbodrill-production.up.railway.app`

---

## How the Gemini Integration Works

```
Browser                    server.js                  Google Gemini
   |                           |                            |
   |  POST /api/chat           |                            |
   |  { prompt, model }  ───►  |                            |
   |                           |  POST generateContent      |
   |                           |  + GEMINI_API_KEY    ───►  |
   |                           |                            |
   |                           |  ◄─── AI response          |
   |  ◄─── JSON response       |                            |
   |                           |                            |
```

- Model used: `gemini-2.5-flash`
- Each explanation prompt is ~60 words max, keeping responses fast and focused
- The API key **only exists on the server** — never sent to or visible in the browser

---

## App Features

### Filter Controls
- **Tense dropdown** — drill All Tenses, or lock to one (Presente, Pretérito, Imperfecto, Futuro)
- **Verb dropdown** — drill All Verbs, or focus on a single verb

### Flashcard Prompts
Natural English phrases, not literal translations:
- `"I used to speak"` → hablaba
- `"We will go"` → iremos
- `"He said"` → dijo
- `"You all have"` → tenéis

### Answer Checking
- Type your answer and hit **Enter** (or click Check)
- Accent-tolerant: typing `hable` matches `hablé` — you still see the correct accented form
- **🎙 Spoke It** — mark correct without typing, for oral drill mode
- **Skip →** — move on without counting it (also bound to **Tab**)

### AI Grammar Breakdown
After every answer you see:
1. **Conjugation chain:** `hablar → Imperfecto → nosotros → hablábamos`
2. **Gemini explanation:** A 2–3 sentence note explaining the rule, irregularity, or your specific mistake

### Session Tracking
- **Streak counter** — resets on any wrong answer, turns gold at 10+
- **Session score** — correct/total with a live progress bar
- **Reset button** — clears stats and loads a new card

### Keyboard Shortcuts
| Key | Action |
|-----|--------|
| `Enter` | Check answer |
| `Tab` | Skip card |
| `Esc` | Clear input |

---

## Getting a Free Gemini API Key

1. Visit [aistudio.google.com](https://aistudio.google.com)
2. Sign in with a Google account
3. Click **Get API Key** → **Create API Key**
4. Copy the key
5. Add it as `GEMINI_API_KEY` in Railway (or your local environment)

The free tier is generous enough for personal study sessions.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Server | Node.js + Express |
| Frontend | Vanilla HTML/CSS/JavaScript |
| Fonts | Space Mono + DM Sans (Google Fonts) |
| AI | Google Gemini 2.5 Flash |
| Hosting | Railway |

No build step. No bundler. No framework. Just `node server.js`.

---

## Troubleshooting

**"API key not configured on server"**
→ Make sure `GEMINI_API_KEY` is set in Railway's Variables tab and the service has redeployed.

**"Could not load AI explanation"**
→ Your API key may be invalid or over quota. Check [aistudio.google.com](https://aistudio.google.com).

**App won't start on Railway**
→ Confirm `package.json` has `"start": "node server.js"` and `"node": ">=18.0.0"` in engines.

**Blank page locally**
→ Make sure you're visiting `http://localhost:3000` (not opening `index.html` directly as a file).

---

*¡Buena suerte con tus conjugaciones!*
