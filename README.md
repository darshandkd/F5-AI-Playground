# F5 AI Playground

An interactive, single-page web application that makes AI infrastructure concepts accessible to everyone — from solution engineers demoing F5's AI portfolio to anyone curious about how LLMs, inference, and AI security actually work.

**[Live Demo](#) | Built with React 18, Tailwind CSS, Framer Motion, Three.js — zero build step, all CDN**

---

## What It Does

### For F5 Practitioners
Simulate real F5 product scenarios with live, animated demos:
- **LLM Request Routing** — Watch F5 BNK classify prompts and route to optimal models on BlueField-3 DPUs
- **NIM Prompt Classification** — NVIDIA NIM + F5 iRules classify query complexity for cost-optimized routing
- **Semantic Caching** — See cache hits reduce token costs by 70%+
- **Prompt Injection Defense** — Test guardrails against injection attacks in real-time
- **AI Red Teaming** — Watch 5 autonomous agents attack an LLM with SVG particle animations, live execution logs, and pause/resume controls
- **DLP, Compliance, API Protection** — 18+ interactive simulations across Inference, Safety, and Data Delivery

### For Anyone Learning AI
Explore 13 animated concept explainers — no prior knowledge required:
- **How LLMs Think** — Type a prompt, watch it flow through tokenization → embeddings → 3D vector space → attention → prediction
- **Prompt Engineering** — Anatomy of prompts, zero-shot vs few-shot, chain-of-thought reasoning, temperature sampling, personas, and a cheatsheet of proven patterns
- **Hallucinations & Grounding** — Why LLMs confabulate, error taxonomy, RAG pipeline visualization, grounding techniques (tool use, citations, self-consistency), and the trust spectrum from raw API to production-safe
- **Context** — See what happens when a context window fills up and overflows
- **Vectors & RAG** — Understand similarity search, vector databases, and retrieval-augmented generation
- **Agents** — Watch chatbot vs agent side-by-side, explore workflow patterns and multi-agent orchestration
- **Inference** — Interactive cost calculator, speed optimization techniques, smart model routing
- **Attention, KV Cache, Transformers** — Visual breakdowns of the architecture powering every LLM

### AI Security Lab
Explore how LLM attacks work — and how to defend against them:
- **LLM Abliteration** — 6-step interactive lab: scatter plot separation, neural probing, refusal vector extraction, 3D matrix projection, typewriter output comparison
- **Model Distillation** — 6-step extraction attack: seed query generation, API interrogation, data curation, student initialization, behavior cloning, clone evaluation
- **AI Guardrails** — 6-step defense-in-depth: input filtering with semantic routing, system prompt wrapping, latent activation clamping, output scanning with egress filter, multi-vector stress testing, secured output comparison

---

## Quick Start

```bash
# Clone and open — that's it
git clone https://github.com/darshandkd/F5-AI-Playground.git
open F5-AI-Playground/index.html
```

No npm, no build, no dependencies to install. Everything loads from CDN. Works in any modern browser.

---

## Architecture

| | |
|---|---|
| **Single file** | `index.html` (~14,000 lines) — entire app in one file |
| **Framework** | React 18 (CDN) with Babel in-browser JSX compilation |
| **Styling** | Tailwind CSS (CDN) with custom dark/light theme system |
| **Animations** | Framer Motion for transitions, Three.js for 3D visualizations |
| **Icons** | Phosphor Icons |
| **Build step** | None — open the file and it works |

### App Structure
```
Landing Page (6 tiles)
 ├── Inference (8 use-cases with product toggles)
 ├── Safety & Security (5 use-cases)
 ├── Data Delivery (5 use-cases)
 ├── AI Concepts (13 interactive explainers)
 ├── AI Security (3 attack/defense labs)
 └── F5 Products (9 product cards)
```

---

## Features

- **Dark/Light mode** — Full theme support across every component
- **Interactive simulations** — Toggle F5 products on/off to see before/after impact
- **Play/Reset animations** — Step-by-step walkthroughs for every concept
- **User input** — Type your own prompts to test tokenization, injection defense, DLP, and NIM classification
- **Search** — Full-text search across all sections from the header
- **Responsive** — Works on desktop and tablet
- **Zero backend** — All simulations run client-side with deterministic data

---

## F5 Products Covered

| Product | Simulations |
|---------|-------------|
| F5 BNK (BlueField-3 DPU) | LLM Routing, Semantic Caching, GPU Utilization, Token Governance, RAG Pipeline, MCP Protection, NIM Classification, EPP Inference Router |
| F5 AI Guardrails (CalypsoAI) | Prompt Injection Defense, DLP, Compliance & Governance |
| F5 AI Red Team (CalypsoAI) | Adversarial Testing (SVG multi-agent attack visualization) |
| F5 AI Remediate (CalypsoAI) | Auto-generated guardrails from Red Team findings |
| F5 SSL Orchestrator | Shadow AI Detection, Encrypted Traffic Inspection |
| F5 Distributed Cloud | API Protection, Edge & Multi-Cloud Delivery |
| F5 NGINX | API Protection, Edge & Multi-Cloud Delivery |
| F5 BIG-IP | RAG Pipeline, Secure Data Ingestion |
| F5 EOB (MantisNet) | 5G Observability |

---

## AI Concepts Covered

| Concept | What You'll Learn |
|---------|-------------------|
| How LLMs Think | Tokenization, embeddings, vector space, self-attention, next-token prediction |
| Transformer Layers | Multi-head attention, feed-forward networks, layer flow, model scaling |
| Attention & KV Cache | Causal mask, QKV mechanics, KV cache compression (TurboQuant) |
| Context Windows | Overflow behavior, compaction strategies, note-taking, sub-agents |
| Token Economics | Token counting, API pricing, cost projection, RAG savings |
| Inference Pipeline | Prefill/decode phases, cost calculation, speed tricks, smart routing |
| First Token Latency | TTFT latency, prefill vs decode, batching, mitigations |
| Vectors & RAG | Embeddings, similarity search, vector databases, RAG pipeline |
| MCP Protocol | USB-C analogy, horizontal pipeline, terminal demo, real-world servers |
| AI Agents | Agent loop, workflow patterns, multi-agent orchestration |
| Token Governance | Rate limiting, budgets, access tiers, policy simulation |
| Prompt Engineering | Prompt anatomy, zero/few-shot, chain-of-thought, temperature, personas |
| Hallucinations & Grounding | Why LLMs hallucinate, error types, RAG, tool use, citations, trust spectrum |

---

## AI Security Labs

| Lab | Steps | Key Animations |
|-----|-------|----------------|
| LLM Abliteration | 6 steps | Spring-physics scatter plots, neural network probing, 3D perspective matrix transforms, typewriter output comparison |
| Model Distillation | 6 steps | Seed query expansion, API interrogation flow, funnel curation, weight grid convergence, loss curve animation |
| AI Guardrails | 6 steps | Packet traffic through semantic router, system prompt wrapping, activation clamping graph, egress scanner beam, concentric rotating defense rings with attack vector beams |
