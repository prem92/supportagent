# ⚡ NexusSupport — AI Support Platform

A multi-client AI support agent platform with SOPs, API tools, data transformers, and role-based access control.

---

## 🚀 Quick Start (Local)

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/nexus-support.git
cd nexus-support

# 2. Install dependencies
npm install

# 3. Start the development server
npm start
```

Open [http://localhost:3000](http://localhost:3000)

**Default credentials:**
| Username | Password | Role |
|----------|----------|------|
| admin | admin | Administrator (full access) |
| agent | agent | Support Agent (chat + view) |
| viewer | viewer | Viewer (read-only) |

---

## 🌐 Deploy to GitHub Pages (free hosting)

### Step 1 — Push to GitHub

```bash
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/nexus-support.git
git push -u origin main
```

### Step 2 — Enable GitHub Pages

1. Go to your repo → **Settings** → **Pages**
2. Under **Source**, select **GitHub Actions**
3. The `deploy.yml` workflow will auto-run on every push to `main`
4. Your app will be live at: `https://YOUR_USERNAME.github.io/nexus-support/`

### Step 3 — Set the homepage in package.json

Add this line to `package.json` (replace with your actual URL):

```json
"homepage": "https://YOUR_USERNAME.github.io/nexus-support"
```

---

## 🏗️ Other Deployment Options

### Vercel (Recommended — fastest, free tier)
```bash
npm install -g vercel
vercel
```
Follow the prompts. Vercel auto-detects Create React App.

### Netlify
```bash
npm run build
# Drag & drop the /build folder to netlify.com/drop
```
Or connect your GitHub repo at netlify.com for auto-deploys.

### Railway / Render
Both support Node.js apps. Point build command to `npm run build`
and publish directory to `build/`.

---

## 🧩 Architecture

```
src/
├── App.js                    # Root component, routing, layout
├── index.js                  # Entry point
├── index.css                 # Global styles + CSS variables
├── sidebar.css               # Sidebar styles
│
├── hooks/
│   ├── useAppState.js        # Global state (useReducer + Context)
│   └── useToast.js           # Toast notification hook
│
├── data/
│   └── initialData.js        # Seed data + API simulator + Transformer engine
│
├── components/
│   ├── Modal.jsx              # Reusable modal wrapper
│   └── Sidebar.jsx            # Navigation sidebar
│
└── pages/
    ├── LoginPage.jsx          # Authentication
    ├── ClientSelectPage.jsx   # Client workspace picker
    ├── ChatPage.jsx           # AI support agent chat
    ├── DashboardPage.jsx      # Stats + activity log
    ├── SopsPage.jsx           # SOP CRUD management
    ├── ToolsPage.jsx          # API Tools CRUD management
    ├── TransformersPage.jsx   # Data transformer management
    ├── ClientsPage.jsx        # Client management (admin)
    └── UsersPage.jsx          # User management (admin)
```

---

## 🔄 Agent Pipeline

When a support query is submitted:

```
User Query
    │
    ▼
┌─────────────────────┐
│  Keyword Matching   │  → Matches query against SOP keywords
└─────────────────────┘
    │ Matched SOP
    ▼
┌─────────────────────┐
│  SOP Resolution     │  → Loads troubleshooting steps
│  Steps Retrieved    │
└─────────────────────┘
    │ Linked Tool ID
    ▼
┌─────────────────────┐
│  API Tool Called    │  → Fetches live data (simulated in demo)
└─────────────────────┘
    │ Raw API Response
    ▼
┌─────────────────────┐
│  Transformer Engine │  → Extracts allowed fields, masks sensitive data
│  Applied            │     (card numbers, PAN, DOB, CVV)
└─────────────────────┘
    │ Filtered payload
    ▼
┌─────────────────────┐
│  Claude LLM         │  → System prompt + SOP + filtered data → response
│  (claude-sonnet-4)  │
└─────────────────────┘
    │
    ▼
Support Answer with trace
```

---

## 🔧 Connecting Real APIs

In `src/data/initialData.js`, replace `simulateApiCall()` with real `fetch()` calls:

```javascript
export async function callRealApi(tool, params) {
  const response = await fetch(tool.endpoint, {
    method: tool.method,
    headers: {
      'Authorization': tool.auth.replace('{{API_KEY}}', process.env.REACT_APP_API_KEY),
      'Content-Type': 'application/json',
    },
    body: tool.method !== 'GET' ? JSON.stringify(params) : undefined,
  });
  return response.json();
}
```

Set your API keys in a `.env` file:
```
REACT_APP_API_KEY=your_key_here
REACT_APP_ANTHROPIC_KEY=your_anthropic_key
```

---

## 🔐 Role Permissions

| Feature | Admin | Agent | Viewer |
|---------|-------|-------|--------|
| Chat with AI agent | ✅ | ✅ | ✅ |
| View SOPs / Tools / Transformers | ✅ | ✅ | ✅ |
| Add / Edit / Delete SOPs | ✅ | ❌ | ❌ |
| Add / Edit / Delete Tools | ✅ | ❌ | ❌ |
| Manage Transformers | ✅ | ❌ | ❌ |
| Client Management | ✅ | ❌ | ❌ |
| User Management | ✅ | ❌ | ❌ |

---

## 📦 Tech Stack

- **React 18** — UI framework
- **useReducer + Context** — State management (no Redux needed)
- **Claude API** (`claude-sonnet-4-20250514`) — LLM for response generation
- **Create React App** — Build tooling
- **GitHub Actions** — CI/CD pipeline
- **CSS Variables** — Theming (dark mode first)
