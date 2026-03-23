# OTC Graph System - Full Build Task

## Phase 1: Backend Setup
- [x] Create `otc-graph/backend/` folder structure
- [x] Create `database.py` - SQLite connection + schema creation
- [x] Create `ingest.py` - JSONL → SQLite ingestion (all 18 folders)
- [x] Run `python ingest.py` and verify row counts
- [x] Create `graph.py` - build nodes/edges from real DB data
- [x] Create `guardrails.py` - keyword + LLM classification
- [x] Create `llm.py` - Gemini 1.5 Flash + Groq fallback
- [x] Create `sql_agent.py` - NL→SQL with full schema + conversation memory
- [x] Create `main.py` - FastAPI (7 endpoints), CORS, lifespan

## Phase 2: Frontend Setup
- [x] Create React + Vite app
- [x] Create `App.jsx` - 2-panel layout
- [x] Create `GraphView.jsx` with Cytoscape.js
- [x] Create `ChatPanel.jsx`, `NodeDetail.jsx`, `SearchBar.jsx`
- [x] Connect API, hooks, routing

## Phase 3: Light Mode & Premium UI Overhaul
- [x] Change global theme to Light Mode (#f8fafc)
- [x] Update node colors to soft theme (SalesOrder: #3b82f6, etc)
- [x] Node styles: 25-35px, 2px white border, smooth edges
- [x] Edge styles: thin (1px), light gray (#cbd5e1), bezier, no sharp arrows
- [x] Layout: bubble-like cose-bilkent (nodeRepulsion: 500000)
- [x] Hover Animation: scale 1.2x, glow, tooltip, highlight edges
- [x] Click Animation: zoom focus, center, highlight neighbors, dim others
- [x] Custom floating interactive tooltip on hover
- [x] Redesign Chat UI: clean white SaaS-style cards, right/left message bubbles
- [x] Smooth transitions, cursor pointer, micro-interactions

## Phase 4: Verification
- [ ] Verify light mode colors
- [ ] Test hover animations (scale, tooltip)
- [ ] Test click animation (zoom, isolate)
- [ ] Test entity query animations
## Phase 5: Space Mode & Dual Theme
- [x] Implement `theme` state and toggle button in `App.jsx` (🌙 / 🚀)
- [x] Create `Starfield.jsx` component for animated parallax stars
- [x] Define `SPACE_STYLES` for Cytoscape (glowing planets, faded orbits) in `GraphView.jsx`
- [x] Update `GraphView.jsx` to dynamically apply `cy.style().fromJson(...)`
- [x] Update `ChatPanel.jsx`, `NodeDetail.jsx`, `SearchBar.jsx` for Space Mode dark glassmorphism
- [x] Ensure tooltips and micro-interactions adjust to current theme

## Phase 6: Production-Grade Conversational System
- [x] Guardrails Module (Keyword & LLM fallback domain verification)
- [x] Context Memory Implementation (6-message history injection)
- [x] Intent Detection Logic (Aggregation, Flow, Anomaly, Lookup via JSON mapping)
- [x] SQL Generation Logic (Zero-hallucination Schema specific logic)
- [x] SQL Pre-Flight Validation (Execution validation via `EXPLAIN QUERY PLAN`)
- [x] End-to-End Formatting (Returns answer, sql, rows, highlighted nodes, and explanation)
