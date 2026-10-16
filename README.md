Smart Recycling Assistant (Code Clash 2.0 • Problem Statement 06)
An intelligent, real-time Computer Vision system capable of recognizing Recyclable, Reusable, Biodegradable, and Hazardous materials, paired with a simulated Digital Twin that tracks material flows across circular processing facilities, and a Generative AI advisor providing personalized circular economy recommendations.
---
🌟 Key Pillars & Features
1. 📷 Computer Vision Recognition Engine
Multi-Source Ingestion:
Live Webcam Stream: Real-time camera viewfinder with tracking reticle and dynamic bounding box overlays.
Drag-and-Drop Image Uploader: Accepts any standard image format (PNG, JPG, WEBP).
Curated Specimen Library: 8 pre-calibrated waste specimens for 1-click end-to-end testing.
4-Class Material Taxonomy:
♻️ Recyclable: PET bottles (#1), aluminum cans, cardboard shipping boxes, tin containers.
🔄 Reusable: Cotton tote bags, insulated stainless steel tumblers, glass containers.
🍃 Biodegradable: Fruit peels (banana, apple), food scraps, coffee grounds, organic biomass.
⚠️ Hazardous: Lithium-ion & alkaline batteries, pressurized aerosol spray cans, e-waste.
Diagnostics: Real-time confidence scores, estimated unit mass ($g$), carbon avoidance ($kg\ CO_2e$), and lifecycle decomposition timeline.
2. 🌐 Digital Twin Material Flow Simulation
Animated Facility Schematic:
Real-time animated canvas visualizing the optical inspection node and 4 branching pneumatic sorting chutes.
Visual material packets traversing the conveyor lines to their assigned circular processing depots:
MRF Hub (Materials Recovery Facility for secondary raw commodities)
Second-Life Reuse Depot (Direct reuse and textile exchange)
Anaerobic Composter (Methane biogas extraction and nutrient-dense soil humus)
Hazardous Remediation Vault (Certified quarantine to prevent toxic leaching)
Live Environmental Telemetry:
Landfill Diversion Rate (%)
Cumulative $CO_2$ Avoided ($kg$)
Total Sorted Mass ($kg$)
Water Conserved ($Liters$) & Grid Energy Saved ($kWh$)
Automated continuous stream simulator to demonstrate high-throughput sorting in real-time.
3. 🧠 Generative AI Circular Advisor
Contextual Upcycle Recipes:
Step-by-step DIY tutorials tailored to the currently detected item (e.g., self-watering planters from PET bottles, herb planters from glass jars).
Disposal & Safety Guidance:
Immediate preparation checklists (emptying, rinsing, crushing, taping battery terminals).
Explicit hazardous warnings to prevent compactor truck fires and groundwater contamination.
Interactive Chatbot:
Ask circular economy questions with quick prompt chips:
"Can I recycle greasy pizza boxes?"
"How do I safely dispose of swollen lithium batteries?"
"What are creative upcycles for empty glass coffee jars?"
4. 📊 Material Audit Trail & Export
Chronological event log of all scanned items with timestamps, material type, destination chute, and verification status.
One-click export to CSV and JSON formats.
---
🚀 Quick Start Guide
Option A: Launch Web Dashboard (Recommended)
From the project directory:
```bash
node server.js
```
Then open your browser at:
👉 http://localhost:5173
No npm install required — the server has zero npm dependencies and runs on the built-in Node runtime. Tailwind, TensorFlow.js, the COCO-SSD neural model weights, and web fonts are all vendored under `/vendor`, so the dashboard and live computer vision run fully offline.
---
Option B: Run Standalone Python Computer Vision Engine
A standalone Python CLI companion script is also included:
```bash
python cv_engine.py
```
Or pass an image path:
```bash
python cv_engine.py sample_bottle.jpg
```
Outputs machine-readable JSON classification telemetry, confidence ratings, and Digital Twin routing packets.
---
🧠 GenAI Advisor Setup (Google Gemini)
The Circular Advisor is powered by Google Gemini (a real LLM). It is called
server-side only, so the API key is never exposed to the browser.
Get a free API key at https://aistudio.google.com/apikey
Provide it as an environment variable named `GEMINI_API_KEY`:
Locally: copy `.env.example` → `.env` and paste your key, or set it in your shell.
On Vercel: Project → Settings → Environment Variables → add `GEMINI_API_KEY`.
> **No key? No problem.** If `GEMINI_API_KEY` is unset or Gemini is unreachable
> (e.g. offline), the advisor automatically falls back to a built-in rule-based
> knowledge engine, so the app keeps working 100% of the time.
---
☁️ Deploy to Vercel
The app is Vercel-ready: static frontend + a serverless API function.
Push this repo to GitHub.
In Vercel, Import the repo (framework preset: Other — no build step needed).
Add the `GEMINI_API_KEY` environment variable (see above).
Deploy. The dashboard is served statically; `/api/v1/*` runs as a serverless
function (`api/[...slug].js`).
Notes on the serverless build:
Persistence is ephemeral on Vercel (writes go to the temp dir and reset on
cold start). The browser holds the live demo state, and CSV/JSON export is
generated client-side, so this is intentional and demo-safe.
The real computer vision runs in the browser (TensorFlow.js), so it works
fully on Vercel. The optional Python bridge is skipped in serverless mode.
---
📁 Project Structure
```
smart-recycling-assistant/
├── index.html                 # Main Cyberpunk/Eco-Tech Dashboard UI
├── server.js                  # Zero-dependency local HTTP server (Port 5173)
├── cv_engine.py               # Standalone Python Computer Vision CLI companion
├── package.json               # Project manifest
├── vercel.json                # Vercel serverless configuration
├── .env.example               # Environment variable template (Gemini key)
├── api/
│   └── [...slug].js           # Vercel serverless API (reuses backend modules)
├── src/
│   ├── app.js                 # UI controller, camera feed, animation loop & chat
│   ├── backend/
│   │   ├── storage.js         # Telemetry/audit persistence (file local, temp on Vercel)
│   │   ├── cvBridge.js        # Python CV bridge with heuristic fallback
│   │   └── genAiService.js    # Gemini LLM advisor + rule-based fallback
│   ├── data/
│   │   └── sampleItems.js     # Taxonomy definitions, specs, and sample library
│   └── services/
│       ├── apiClient.js       # Frontend REST client for the backend API
│       ├── classifier.js      # Computer Vision classification & bounding box engine
│       └── digitalTwinState.js# Digital Twin telemetry & material flow simulation
├── vendor/                    # Vendored Tailwind, TF.js, COCO-SSD model & fonts (offline)
└── README.md                  # Project documentation
```
