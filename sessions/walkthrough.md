# OTC Graph Explorer — Light Mode Premium Edition

## What Was Built

A full-stack **SAP Order-to-Cash Graph Query System** with:
- **Backend**: FastAPI + SQLite + Gemini 1.5 Flash
- **Frontend**: Premium Light Mode React + Cytoscape.js with bubble-like floating physics and SaaS-styled chat.

---

### Dual Theme System (Light & Space Mode)

*Seamlessly swap between a clean minimal SaaS layout and an immersive, dark animated visualization.*

![Space Mode UI View](/C:/Users/Nikhil/.gemini/antigravity/brain/b0cd0771-c1bc-41b8-9709-ff98a6c46edc/space_mode_verification_final_1774257566513.png)

*Space Mode: Features a dark radial vignette background, animated parallax stars, glowing realistic 3D planet nodes, translucent orbit edges, and premium dark glassmorphism chat panels with frosted blurs.*

### Realistic SVG Planets

![Realistic 3D SVG Planets](/C:/Users/Nikhil/.gemini/antigravity/brain/b0cd0771-c1bc-41b8-9709-ff98a6c46edc/space_mode_planets_1774258487847.png)

*Zoomed in view of the 3D-generated SVG planets. Each node type features unique radial light sources, deep atmospheric inner shadows, and turbulent cloud/surface textures. The planets feature a subtle floating 'alive' jitter animation to complete the immersive space vibe.*

---

## Deployment Guide (Vercel & Render)

The entire `otc-graph` project has been successfully pushed as a standalone project to your designated repository: `Graph-Intelligence-System-for-Enterprise-Data-SAP-O2C-`!

### 1. Deploy Frontend (Vercel)
1. Navigate to your [Vercel Dashboard](https://vercel.com/new).
2. Click **Add New Project** and import the `Graph-Intelligence-System-for-Enterprise-Data-SAP-O2C-` repository from GitHub.
3. Under **Project Settings**, set the **Root Directory** to `frontend`.
4. Leave the Framework preset as **Vite** (build command `npm run build` and output `dist` will autofill).
5. Exapnd **Environment Variables** and add:
   - `VITE_API_URL`: `https://YOUR-RENDER-BACKEND.onrender.com/api` *(You will get this URL from Render shortly)*.
6. Click **Deploy**.

### 2. Deploy Backend (Render)
1. Navigate to your [Render Dashboard](https://dashboard.render.com).
2. Click **New +** and select **Web Service**. Connect the same GitHub repository.
3. Scroll down and set the **Root Directory** to `backend`.
4. Configure the environment fields:
   - **Environment**: Python 3
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Click **Advanced** and add your **Environment Variables**:
   - `GEMINI_API_KEY`: `AIzaSy...` (your key)
   - `GROQ_API_KEY`: `gsk_e5...` (your key)
6. Click **Create Web Service**. 

Once Render is active, grab the `.onrender.com` URL, head back to Vercel, and paste it into the `VITE_API_URL` environment variable if you haven't already! The frontend is strictly configured to intercept this cloud API dynamically.

![Light mode initial state](/C:/Users/Nikhil/.gemini/antigravity/brain/b0cd0771-c1bc-41b8-9709-ff98a6c46edc/initial_graph_state_light_1774253793805.png)

*Light Mode: Breathable bubble-like layout (nodeRepulsion: 500000). Soft background gradient (`#ffffff` to `#f1f5f9`). Redesigned Chat UI with SaaS cards and rounded pills.*

### Animation State — "show products" query

![Product node animation](/C:/Users/Nikhil/.gemini/antigravity/brain/b0cd0771-c1bc-41b8-9709-ff98a6c46edc/show_products_focused_final_1774254187416.png)

*Teal product nodes highlighted across the graph, others dimmed to low opacity. Notice the chat response in the modern message bubble layout.*

---

## Interactive Light Mode Features

### 1. Controlled Viewport & Floating Physics
- **Cytoscape config:** `cose-bilkent` with `nodeRepulsion: 500000`, `gravity: 0.05`, and `fit: false`.
- Nodes feel like floating, organic bubbles rather than rigid structures.
- **Viewport Constraints:** Strict minimum and maximum zoom clamping (`minZoom: 0.5`, `maxZoom: 2`) ensures nodes are never lost or rendered as tiny unclickable dots, efficiently consuming 70-80% of the screen.

### 2. Micro-Interactions
- **Hover:** Nodes scale 1.2x on hover, glowing, connected edges highlight, and a clean white floating tooltip appears.
- **Click:** The camera zooms in (1.5x) securely on the clicked node, dims all unrelated nodes instantly, and slides the modern detail card in from the left.

### 3. Redesigned Chat Panel
- Transitioned to a clean SaaS structure (white background, very subtle shadow).
- **User messages:** Soft blue bubbles.
- **AI responses:** Clean flat white cards with sleek drop-down SQL/Data viewers.
- **Input:** Pill-like rounded input box bordered with `#cbd5e1` that highlights blue on focus.

### 4. Color Palette
| Type | Color |
|---|---|
| **SalesOrder** | `#3b82f6` (Soft Blue) |
| **Delivery** | `#10b981` (Soft Green) |
| **Billing** | `#f59e0b` (Soft Amber) |
| **Customer** | `#8b5cf6` (Soft Purple) |
| **Product** | `#06b6d4` (Soft Cyan) |
| **Plant** | `#f43f5e` (Soft Rose) |

---

## How to Run

```bash
# Backend (otc-graph/backend/)
python -m uvicorn main:app --host 0.0.0.0 --port 8000

# Frontend (otc-graph/frontend/)
npm run dev    →  http://localhost:5173
```

---

## Recording

![Animation Demo](file://C:\Users\Nikhil\.gemini\antigravity\brain\b0cd0771-c1bc-41b8-9709-ff98a6c46edc\otc_light_mode_ui_1774253736396.webp)
