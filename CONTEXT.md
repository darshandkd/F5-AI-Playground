# F5 AI Playground — Complete Project Context

> **Last updated:** 2025-03-25
> **File:** `index.html` — ~7,459 lines, single-file React application
> **Backups:** `index-v1-backup.html` (vanilla JS), `index-v2-backup.html` (pre-redesign React), `index-v3-backup.html` (pre-context-engineering), `index-v4-backup.html` (pre-MHA-redesign)
> **Purpose:** Interactive marketing demo showcasing F5's AI product portfolio through animated simulations and educational visualizations.

---

## 1. Architecture

### Stack (all CDN, zero build step)
| Library | Version | Role |
|---------|---------|------|
| React 18 | `react@18` | UI framework |
| Tailwind CSS | CDN | Utility-first styling |
| Framer Motion | `10.12.16` | Layout animations, AnimatePresence |
| GSAP + Flip + ScrollTrigger | `3.12.2` | Canvas animations, number counters |
| Three.js | `r128` | 3D vector space visualization |
| Babel Standalone | latest | In-browser JSX compilation |
| Phosphor Icons | `@phosphor-icons/web` | Icon system |
| Fonts | DM Sans + JetBrains Mono | Typography |

### File Structure
```
F5-AI-Playground/
  index.html              ← THE file (~7,459 lines)
  index-v1-backup.html    ← Vanilla JS backup
  index-v2-backup.html    ← Pre-redesign React backup
  index-v3-backup.html    ← Pre-context-engineering backup
  index-v4-backup.html    ← Pre-MHA-redesign backup
  project-details.md      ← Full spec: products, use-cases, metrics
  CONTEXT.md              ← This file
  content/
    f5-bnk-simulator.html
    llm-visualizer.html
    llm_architecture_deep_dive.md
  inspiration/
    llm-simulator.html
    hero.png
    web-page-inspiration.png
```

### State Management
- **ThemeContext** (React Context) — `isDark` boolean, persisted to `sessionStorage`
- All component state via React hooks (`useState`, `useEffect`, `useRef`, `useCallback`, `useMemo`)
- No external state library

---

## 2. Application Structure

### Navigation Flow
```
App
 ├─ LandingPage (5 glassmorphic tiles)
 │   └─ Click tile → TabView
 ├─ TabView
 │   ├─ Header (tab bar + theme toggle + F5 logo 48px)
 │   └─ TAB_COMPONENTS[activeTab]
 │       ├─ [0] InferenceTab      (6 use-cases)
 │       ├─ [1] SafetyTab         (5 use-cases)
 │       ├─ [2] DataDeliveryTab   (5 use-cases)
 │       ├─ [3] PlaygroundTab     (7 concepts) — renamed to "AI Concepts"
 │       └─ [4] ProductsTab       (8 product cards)
 └─ Footer (fixed bottom: "Feedback or ideas? d.doshi@f5.com")
```

### Top-Level Tabs (TAB_DATA)
| id | Label | Icon | Notes |
|----|-------|------|-------|
| 0 | Inference | ph-lightning | 6 simulation use-cases |
| 1 | Safety & Security | ph-shield-check | 5 simulation use-cases |
| 2 | Data Delivery | ph-database | 5 simulation use-cases |
| 3 | AI Concepts | ph-atom | 7 interactive concept explainers (was "AI Playground") |
| 4 | F5 Products | ph-package | 8 product cards (scrollable layout) |

### Reusable Layout Components
| Component | Purpose |
|-----------|---------|
| `UseCase` | Wrapper: title + description + toggle + simulation + metrics. Supports `onStats` callback for live metrics. |
| `ProductToggle` | Accessible switch with keyboard support |
| `MetricsPanel` | Before/after animated metrics cards. Supports `displayMetrics` override from live sim stats. |
| `Sidebar` | Scroll-spy nav |
| `TabLayout` | Sidebar + animated content column |
| `PlaygroundConcept` | Section wrapper for AI Concepts tab |
| `HelpTooltip` | Portal-based tooltip (escapes overflow). `?` icon, themed, `onClick={e => e.stopPropagation()}` |
| `AnimatedCounter` | GSAP-powered number animation with snap |

---

## 3. Key Components Added (2025-03-25 Session)

### HelpTooltip (Portal-based)
- Renders via `ReactDOM.createPortal` to `document.body` — escapes all parent `overflow: hidden/auto`
- Fixed positioning calculated from `getBoundingClientRect()`
- Smart direction: shows below if <80px space above
- z-index 9999, theme-aware bg/text
- 27 tooltips across 6 AI Concepts + all sub-tabs

### ContextEngineering (7th AI Concept: "Context - all about")
5 sub-tabs:
| Tab | Component | Animation |
|-----|-----------|-----------|
| What is Context? | `WhatTab` | Auto-stacking cards with token counter, replay |
| The Window | `WindowTab` → `ContextOverflowSim` | Play/Reset, 3 presets (8K/16K/32K), block fill with eviction |
| Attention Fades | `AttentionTab` | Fixed 8×8 grid, slider controls fill density, recall % meter |
| Building Blocks | `BlocksTab` | Toggle components on/off, stacked bar, "right altitude" examples |
| Staying Sharp | `SharpTab` | 3 strategy demos with Play/Reset: Compaction (5-phase), Notes, Sub-Agents |

### Context Overflow Simulator (inside WindowTab)
- 12 context blocks (system prompt, 5 conversation turns, docs, tools)
- 3 preset window sizes: 8K, 16K, 32K
- Play/Reset controls (no auto-play)
- Scrollable block list (max-h-[260px])
- FIFO eviction with red "evicted" badges
- 3 consequence cards on overflow (Forgets Instructions, Loses Context, Hallucinations)

### MHAFFNDiagram (Redesigned with 3 tabs)
| Tab | Content |
|-----|---------|
| The Flow | Step-by-step animated walkthrough with Play/Reset. Click any reached layer to expand/collapse description. |
| Attention vs FFN | Side-by-side comparison table (6 rows) with header cards |
| How It Scales | 3 model cards (GPT-3, Llama 70B, Llama 405B) with animated layer bars + stat grids |

---

## 4. AI Gateway Removal (Complete)

**F5 AI Gateway** product has been fully removed from the entire app:
- Removed from `F5_PRODUCTS` array (was 9 products, now 8)
- Removed `{ key: 'gateway' }` from all UseCase product arrays (Inference + Safety)
- Removed `gatewayActive` variable from all 5 simulation components
- Simplified all conditional branches (status messages, tier logic)
- Changed all `tier: 'gateway'` data annotations to `tier: 'guardrails'`
- Updated all description text to remove AI Gateway mentions
- Replaced "AI Gateway" references in SimAPIProtection with "App Protect"

### Updated Product Toggle Dependencies
| Simulation | Product Toggles |
|-----------|----------------|
| SimRouting | `toggles.bnk` |
| SimCaching | `toggles.bnk` |
| SimGPU | `toggles.bnk` |
| SimTokenGov | `toggles.bnk` |
| SimRAG | `toggles.bnk`, `toggles.bigip` |
| SimMCP | `toggles.bnk` |
| SimPromptDefense | `toggles.guardrails` |
| SimRedTeam | `toggles.redteam` |
| SimDLP | `toggles.guardrails` |
| SimShadowAI | `toggles.sslo` |
| SimCompliance | `toggles.guardrails` |
| SimIngestion | `toggles.bigip` |
| SimEncryptedTraffic | `toggles.sslo` |
| SimAPIProtection | `toggles.distcloud`, `toggles.nginx` |
| SimObservability | `toggles.eob` |
| SimEdge | `toggles.distcloud`, `toggles.nginx` |

---

## 5. Input Field Standardization

All user input fields across the app now follow a consistent format:

```jsx
<div className="relative">
    <textarea value={val} onChange={e => setVal(e.target.value)}
        maxLength={40}
        placeholder="Type a prompt... e.g. 'The future of AI is'"
        className={`w-full h-24 p-4 rounded-xl font-mono text-sm resize-none border
            focus:outline-none focus:border-f5-red transition-colors ${
            isDark ? 'bg-surface-900 border-white/10 text-white placeholder-gray-600'
                   : 'bg-gray-50 border-gray-200 text-gray-900 placeholder-gray-400'
        }`} />
    <span className={`absolute bottom-2 right-3 text-[10px] font-mono ${
        val.length >= 40 ? 'text-f5-red font-bold' : (isDark ? 'text-gray-600' : 'text-gray-400')
    }`}>{val.length}/40</span>
</div>
```

**Applied to:** Stage 1 (How LLMs Think), Token Economy Tokenizer, Attention KV Mask, SimPromptDefense, SimDLP
- All use `<textarea>` (no `<input type="text">`)
- `maxLength={40}`, character counter, `rounded-xl`, `font-mono text-sm`
- Sim-panel inputs use `h-16` (shorter); standalone inputs use `h-24`
- Enter submits on sim inputs (Shift+Enter for newline)

---

## 6. Global CSS Additions

### Scrollbar Theming
```css
/* Dark mode */
.dark ::-webkit-scrollbar { width: 6px; height: 6px; }
.dark ::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.15); border-radius: 9999px; }
/* Light mode */
.bg-surface-25 ::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.15); }
/* Firefox: scrollbar-color on body + scrollable elements (div, nav, textarea, etc.) */
```

### Range Slider (`.range-visible`)
All `<input type="range">` use the `.range-visible` class:
- Track: 6px, `rgba(255,255,255,0.08)` dark / `rgba(0,0,0,0.08)` light
- Thumb: 16px red circle (#e4002b), no border, subtle shadow
- WebKit + Firefox pseudo-elements

### Footer CSS Variable
```css
.dark { --footer-bg: #0a0a0f; }
.bg-surface-25 { --footer-bg: #eef0f4; }
```
Footer uses `linear-gradient(to top, var(--footer-bg) 60%, transparent)`.

---

## 7. Sidebar Definitions (Updated)

### PLAYGROUND_SIDEBAR (AI Concepts tab)
```
pg-llm-think     → How LLMs Think
pg-token-economy → Token Economy
pg-token-gov     → Token Governance
pg-hol-ttft      → The First Token Problem
pg-attention     → Attention & KV Cache
pg-mha-ffn       → MHA + FFN
pg-context       → Context - all about
```

### Attention & KV Cache Sub-tabs (3 tabs, "At Scale" removed)
```
mask → The Mask
qkv  → QKV Mechanics
kv   → KV Cache
```

---

## 8. F5 Products (Tab 5) — 8 Products

| # | Name | Color | Icon |
|---|------|-------|------|
| 1 | F5 BNK | `#e4002b` | ph-cpu |
| 2 | F5 AI Guardrails (CalypsoAI) | `#00b4d8` | ph-shield-warning |
| 3 | F5 AI Red Team (CalypsoAI) | `#ef4444` | ph-bug |
| 4 | F5 SSL Orchestrator | `#a855f7` | ph-lock-key |
| 5 | F5 Distributed Cloud | `#10b981` | ph-cloud |
| 6 | F5 EOB (MantisNet) | `#f59e0b` | ph-chart-line |
| 7 | F5 BIG-IP v21.0 | `#6366f1` | ph-hard-drives |
| 8 | F5 NGINX | `#0ea5e9` | ph-globe |

**Note:** F5 AI Gateway was removed entirely (2025-03-25).

---

## 9. Token Display Limits (How LLMs Think Pipeline)

| Stage | Display Limit | Grid Type |
|-------|--------------|-----------|
| Stage 2 (Tokens) | All tokens | Flex wrap pills |
| Stage 3 (Embeddings) | 8 tokens | 3-column grid |
| Stage 4 (3D Vector Space) | 8 tokens | Three.js clusters (8 colors) |
| Stage 5 (Attention) | 8 tokens | NxN table (min 3 with padding) |
| Stage 6 (Prediction) | Last 3 tokens | Probability bars |

- **Input maxLength:** 40 characters
- **3-token minimum padding:** If <3 tokens, pad with `...`, `▸`, `▹` placeholders (Stage 5 + MaskTab)
- **8-color palette** for 3D: `#e4002b, #ff6a13, #00b4d8, #10b981, #a855f7, #f59e0b, #ec4899, #6366f1`

---

## 10. Light Mode Attention Matrix Colors

### Stage 5 (PipelineAttention — How LLMs Think)
- Dark: Red `rgba(228,0,43,opacity)` with white text
- Light: Purple `rgba(109,40,217,opacity)`, min opacity 0.18. Text: `#4c1d95` when intensity <0.45, white otherwise.

### MaskTab (Attention & KV Cache)
- Dark: Purple `rgba(168,85,247,intensity)`
- Light: Purple `rgba(109,40,217,intensity)`. Text: `text-white/90` when intensity >0.35, `text-gray-700` otherwise.

---

## 11. Semantic Caching Dynamic Metrics

SimCaching reports live stats to UseCase via `onStats` callback:
- `UseCase` has `liveMetrics` state, passes `setLiveMetrics` as `onStats` to simulation
- `displayMetrics` merges live values over static metrics by matching `m.label`
- SimCaching reports: Token Cost (monthly estimate), Cache Hit Rate (%), Cached Response (<50ms / 1.2s)

Stats row inside SimCaching: 4-column grid (Hit Rate with bar, Tokens Saved, Cost Saved, Queries).

---

## 12. Gotchas & Lessons Learned

### Delimiter Balance
Run after EVERY edit:
```python
python3 -c "
import sys
content = open('index.html', 'r').read()
pairs = {'(': ')', '{': '}', '[': ']'}
stack = []
for i, ch in enumerate(content):
    if ch in pairs: stack.append((ch, i))
    elif ch in pairs.values():
        expected = [k for k,v in pairs.items() if v == ch][0]
        if not stack:
            line = content[:i].count('\n') + 1
            print(f'Unmatched at line {line}'); sys.exit(1)
        top, pos = stack.pop()
        if top != expected:
            line = content[:i].count('\n') + 1
            print(f'Mismatch at line {line}'); sys.exit(1)
if stack:
    print(f'Unclosed at line {content[:stack[-1][1]].count(chr(10))+1}'); sys.exit(1)
print('Balanced!')
"
```

### Regex Literal Parens
Use `\x28` hex escape instead of `\(` for literal parens in regex patterns.

### Edit Uniqueness
When using the Edit tool with `replace_all: false`, include enough surrounding context for uniqueness.

### CSS Gotchas
- `*` wildcard in scrollbar CSS also affects `<input type="range">` — use specific element selectors
- Range input inline `style` for track color doesn't work cross-browser — use CSS pseudo-elements
- Portal-based tooltips needed for anything inside `overflow: hidden/auto` containers

### Animation Intervals
- Auto-only sims: 2000ms | User-input sims: 3000ms | GPU bars: 500ms
- Context Overflow Sim: 800ms per block | MHA Flow: 1200ms per layer
- Staying Sharp Compaction: 400ms per message, 2000ms warning hold, 1500ms compress

### Footer
- Fixed bottom, z-40, gradient bg matching body color
- Scrollable tabs (Products) need `pb-16` to clear footer
- Landing page needs `pb-10`

---

## 13. Change Log (2025-03-25 Session)

1. **Help tooltips** — 27 tooltips across all AI Concepts sub-tabs (portal-based)
2. **"Context - all about"** — New 7th AI Concept with 5 sub-tabs
3. **Help tab → footer** — Replaced full Help tab with fixed-bottom email link
4. **AI Gateway removed** — All references across Safety, Inference, Products, and sim logic
5. **"AI Playground" → "AI Concepts"** tab rename
6. **Input standardization** — All inputs: blank defaults, maxLength=40, char counters, textarea
7. **Token display limits** — Raised to 8 across Stages 3-5; 3-token minimum padding for grids
8. **Light mode fixes** — Purple palette for attention matrices with adaptive text contrast
9. **Scrollbar theming** — Subtle themed scrollbars globally (dark/light)
10. **Range slider theming** — `.range-visible` CSS class for all sliders
11. **MHA+FFN redesign** — 3 animated tabs: The Flow (clickable layers), Attention vs FFN, How It Scales
12. **Semantic Caching** — Live dynamic metrics + clean 4-column stats grid
13. **MHA+FFN height fix** — Removed `h-full` stretching
14. **Staying Sharp demos** — Play/Reset controls, 5-phase compaction, slower pacing
15. **Attention KV "At Scale" tab removed**
16. **F5 logo enlarged** — 38px → 48px
17. **Footer overlap fix** — `pb-16` on scrollable tabs, gradient bg on footer
