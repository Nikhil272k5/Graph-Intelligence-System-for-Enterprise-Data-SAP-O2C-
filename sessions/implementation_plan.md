# Order-to-Cash Graph Query System — Implementation Plan

Build a full-stack SAP O2C Graph Explorer from scratch using the real dataset at `c:\Users\Nikhil\Desktop\Dodge\sap-o2c-data\`. The system ingests JSONL files into SQLite, builds an interactive graph with Cytoscape.js, and provides an AI chat interface powered by Gemini/Groq.

## User Review Required

> [!IMPORTANT]
> **API Keys Needed** — Before starting, please provide:
> 1. **Gemini API Key** → get free at https://ai.google.dev (primary LLM)
> 2. **Groq API Key** → get free at https://console.groq.com (fallback LLM)
> 
> I will prompt you for these before running the backend.

> [!NOTE]
> **Dataset observations from reading the files:**
> - `billing_document_items` uses `referenceSdDocument` / `referenceSdDocumentItem` (not `salesDocument`)  
> - `billing_document_cancellations` is nested inside `customer_company_assignments/` subfolder  
> - `products` has `isMarkedForDeletion` (boolean) and `crossPlantStatusValidityDate` (can be null)  
> - All numeric fields come as strings — need conversion

---

## Proposed Changes

### Backend (`otc-graph/backend/`)

#### [NEW] database.py
Creates 19-table SQLite schema, provides `get_db()` helper.

#### [NEW] ingest.py
- Walks all 18 `sap-o2c-data/` subfolders recursively
- Handles the misplaced `billing_document_cancellations` folder
- Maps `referenceSdDocument` → `salesDocument` for billing_document_items
- Converts booleans to 0/1, numeric strings to REAL
- Prints row counts after ingestion

#### [NEW] graph.py
- `get_graph_data(db, limit=200)` — builds nodes + edges from real DB queries
- Node types: SalesOrder (#4A90D9), Delivery (#7ED321), Billing (#F5A623), Customer (#9B59B6), Product (#1ABC9C), Plant (#E74C3C), JournalEntry (#95A5A6)
- Edges based on verified FK columns from actual JSONL data
- `get_node_detail(db, node_id, node_type)` and `get_neighborhood()` helpers

#### [NEW] guardrails.py
- Keyword list: order/delivery/billing/invoice/payment/customer/product/sap/etc.
- LLM fallback classification if no keywords match
- Returns `REJECTION_MESSAGE` for off-topic queries

#### [NEW] llm.py
- `LLMClient` class, tries Gemini 1.5 Flash first, Groq llama-3.1-8b-instant as fallback
- Both keys read from `.env`

#### [NEW] sql_agent.py
- Full schema embedded as constant
- `generate_sql(question, history)` with conversation context
- `execute_and_explain()` → runs SQL, gets NL answer, extracts `highlighted_nodes`
- Validates table/column existence before execution

#### [NEW] main.py
FastAPI app with:
- `GET /api/graph` — graph data with optional `?limit=` and `?search=`
- `GET /api/graph/search?q=` — search nodes
- `GET /api/node/{type}/{id}` — node detail
- `POST /api/chat` — NL query with history, returns answer + sql + data + highlighted_nodes
- `GET /api/stats` — entity counts
- `GET /api/health` — health check

#### [NEW] requirements.txt + .env.example

---

### Frontend (`otc-graph/frontend/`)

#### [NEW] React + Vite app with Cytoscape.js

**GraphView.jsx** — The centerpiece:
- `cytoscape` + `cytoscape-cose-bilkent` layout (well-spaced, organic)
- Dark gradient background: `linear-gradient(135deg, #0f172a, #1e293b)`
- Node colors by type, size 32px default
- `focusOnType(type)`: staged animation — fade unrelated → zoom in → highlight
- `resetGraph()`: restore all nodes, zoom out
- Hover tooltip showing entity info
- Glow effect on focused nodes
- "Fit All" / "Reset View" button
- Node type filter (checkboxes per type)
- Search integration with `SearchBar`

**ChatPanel.jsx** — Fixed right panel:
- Stays fixed regardless of graph pan/zoom
- Loading states, typing indicator
- Collapsible SQL + data viewer
- Suggested chips disappear after first message
- Conversation history (last 6 pairs)
- On response: detect entity type → call `focusOnType()` → after 3s call `resetGraph()`
- Highlighted nodes from API → yellow in graph

**NodeDetail.jsx** — Slide-in on click:
- All props shown as key:value
- "Ask about this node" prefill button

**SearchBar.jsx** — Live filter:
- Filters graph nodes by ID/label as you type
- Non-matching nodes dimmed

**Responsive design**: Works on phone, tablet, desktop.

---

## Verification Plan

### Automated Tests (run from terminal)

```powershell
# 1. Backend health
Invoke-WebRequest http://localhost:8000/api/health | ConvertFrom-Json

# 2. Stats
Invoke-WebRequest http://localhost:8000/api/stats | ConvertFrom-Json

# 3. Graph data
Invoke-WebRequest "http://localhost:8000/api/graph?limit=50" | ConvertFrom-Json

# 4. Chat - domain query
$body = '{"question":"how many sales orders?","conversation_history":[]}'
Invoke-WebRequest -Method POST -Uri http://localhost:8000/api/chat -Body $body -ContentType "application/json" | ConvertFrom-Json

# 5. Chat - off-topic rejection
$body = '{"question":"who is PM of India?","conversation_history":[]}'
Invoke-WebRequest -Method POST -Uri http://localhost:8000/api/chat -Body $body -ContentType "application/json" | ConvertFrom-Json
```

### Browser Verification (via browser subagent)
- Graph renders with colored nodes at `http://localhost:5173`
- Search bar filters nodes correctly
- Clicking a node opens detail panel
- Chat entity query focuses correct node type
- Chat panel stays fixed when zooming graph
- Responsive on different window sizes
