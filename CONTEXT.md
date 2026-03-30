# F5 AI Playground — Complete Project Context

> **Last updated:** 2026-03-30
> **File:** `index.html` — ~11,520 lines, single-file React application
> **GitHub:** https://github.com/darshandkd/F5-AI-Playground
> **Backups:** v1 through v13 backup files in project root

---

## 1. Architecture

### Stack (all CDN, zero build step)
| Library | CDN | Role |
|---------|-----|------|
| React 18 | cdn.jsdelivr.net | UI framework |
| Tailwind CSS | cdn.tailwindcss.com | Utility-first styling |
| Framer Motion 10.12.16 | cdn.jsdelivr.net | Layout animations, AnimatePresence |
| Three.js r128 | cdnjs.cloudflare.com (deferred) | 3D vector space visualization |
| Babel Standalone | cdn.jsdelivr.net | In-browser JSX compilation |
| Phosphor Icons | cdn.jsdelivr.net (deferred) | Icon system |
| Fonts | Google Fonts | DM Sans + JetBrains Mono |

**Removed:** GSAP (replaced with requestAnimationFrame-based AnimatedCounter)

### Root font-size: 17px
Set on `html` element. All Tailwind rem-based sizes scale proportionally. Fixed-pixel values (`text-[9px]`, `text-[10px]`) unaffected.

---

## 2. Application Structure

```
App
 ├─ LandingPage (5 tiles, 360px fixed width, 3+2 layout)
 ├─ TabView
 │   ├─ Header (tab bar + search + theme toggle + F5 logo 40px)
 │   └─ TAB_COMPONENTS[activeTab]
 │       ├─ [0] InferenceTab      (8 use-cases)
 │       ├─ [1] SafetyTab         (5 use-cases)
 │       ├─ [2] DataDeliveryTab   (5 use-cases)
 │       ├─ [3] PlaygroundTab     (11 AI concepts)
 │       └─ [4] ProductsTab       (9 product cards)
 └─ Footer ("Feedback or ideas? d.doshi@f5.com")
```

---

## 3. Products (9)

| Product | Color | Toggle Key | Notes |
|---------|-------|------------|-------|
| F5 BNK | #e4002b | bnk | BlueField-3 DPU |
| F5 AI Guardrails | #00b4d8 | guardrails | CalypsoAI |
| F5 AI Red Team | #ef4444 | redteam | CalypsoAI |
| **F5 AI Remediate** | **#00b4d8** | **remediate** | **NEW — dependent toggle** |
| F5 SSL Orchestrator | #a855f7 | sslo | |
| F5 Distributed Cloud | #10b981 | distcloud | |
| F5 EOB (MantisNet) | #f59e0b | eob | |
| F5 BIG-IP v21.0 | #6366f1 | bigip | |
| F5 NGINX | #0ea5e9 | nginx | |

### AI Remediate Toggle Behavior
- **Dependent:** Grayed out (40% opacity, pointer-events: none) unless parent toggle (guardrails/redteam) is active
- **Auto-disables:** When parent toggle turns off, remediate auto-turns off
- **Accent color:** Cyan (#00b4d8) via `accentColor` prop on ProductToggle
- **ProductToggle** now accepts optional `accentColor` prop (defaults to F5 red #e4002b)

---

## 4. Simulation Patterns

### Chat-Style Sims (Prompt Injection, DLP)
- Persistent messages in scrollable container (fixed height: 280-320px)
- Messages stay until Reset clicked
- Pause/Resume + Reset buttons
- Auto-cycle with immediate first run (500ms kickoff)
- Message roles: attacker/agent (left, slate bg), system (centered pill), blocked/vulnerable/remediate (right, colored)
- `scanning` messages replaced with static checkmark after response arrives

### SVG Particle Sim (AI Red Teaming)
- 5 autonomous agents fire attacks via SVG cubic Bezier curves to LLM target
- Base paths use `<linearGradient>` per agent (color fades 0.25→0.7 opacity), stroke-width 4
- Dashed flow overlay via CSS `.rt-flow-line` (stroke-dasharray: 6 12, 0.8s animation)
- Particles: SVG `<circle>` r=4 animated along path via `getPointAtLength()` + `requestAnimationFrame` (800ms, ease-in-out)
- Agent cards with color-tinted backgrounds, origin dots, scale+glow on fire
- LIVE_EXECUTION_LOGS terminal (180px, dark bg, timestamped color-coded entries)
- Pause/Resume/Reset controls; log scroll-lock (user scroll-up disables auto-scroll)
- Stable callback refs (`drawPathsRef`, `fireAttackRef`) prevent useEffect re-trigger loops
- Middle agent forced arc: `bow = dy < 60 ? 50 : 0` on first Bezier control point
- Sim container minHeight: 680px

### Log-Style Sims (all others)
- Top-to-bottom prepend: `[entry, ...prev].slice(0, 6)`
- `isNewest = i === 0`
- Green/red row backgrounds
- Fixed-height log containers where applicable

### ProductToggle Component
```jsx
<ProductToggle label="AI Remediate" icon="ph-first-aid" accentColor="#00b4d8" active={...} onChange={...} />
```
- `accentColor` controls bg tint, border, icon, and label color when active
- Without `accentColor`: defaults to F5 red (#e4002b)

---

## 5. AI Concepts (11 items, ordered for learning progression)

| # | Sidebar Label | Component | Sub-tabs |
|---|--------------|-----------|----------|
| 01 | How LLMs Work | HowLLMsThink | 6-stage pipeline |
| 02 | Transformer Layers | MHAFFNDiagram | The Flow, Attention vs FFN, How It Scales |
| 03 | Attention & Cache | AttentionKVCache | The Mask, QKV Mechanics, KV Cache (with compression) |
| 04 | Context Windows | ContextEngineering | What is Context?, The Window (overflow sim), Attention Fades, Building Blocks, Staying Sharp |
| 05 | Token Economics | TokenEconomy | Tokenizer, Cost Pipeline, At Scale, Why RAG? |
| 06 | Inference Pipeline | InferenceExplainer | What is Inference?, The Cost, Speed Tricks, Smart Routing |
| 07 | First Token Latency | TheFirstTokenProblem | The Wait, Why It Happens, Batching, Fix It |
| 08 | Vectors & RAG | VectorExplainer | What are Vectors?, Similarity Search, Vector Database, RAG Pipeline |
| 09 | MCP Protocol | MCPExplainer | What is MCP?, How it Works, The 3 Primitives, Real World |
| 10 | AI Agents | AgentExplainer | What is an Agent?, The Agent Loop, Workflow Patterns, Multi-Agent |
| 11 | Token Governance | TokenGovernancePlayground | Policy Builder, Live Simulation, Scenarios |

---

## 6. Global CSS Rules

### Scrollbars
```css
.dark ::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.15); }
.bg-surface-25 ::-webkit-scrollbar-thumb { background: rgba(0,0,0,0.15); }
```
Target specific elements (div, nav, textarea, etc.), NOT `*` wildcard.

### Range Sliders
`.range-visible` class: 6px track, 16px red thumb, no border. Track: `rgba(255,255,255,0.08)` dark / `rgba(0,0,0,0.08)` light.

### Footer
Fixed bottom, gradient bg via `--footer-bg` CSS var. Scrollable tabs need `pb-16`.

---

## 7. Dark/Light Mode Design Rules

| Rule | Details |
|------|---------|
| Minimum text gray | `text-gray-500` in light mode (never gray-400 for labels) |
| Minimum border gray | `border-gray-200` in light mode (never gray-100 for cards) |
| Inline hex grays | `isDark ? '#6b7280' : '#6b7280'` (gray-500 both modes) |
| Attention matrices | Light: purple `rgba(109,40,217)`, dark: red `rgba(228,0,43)` |
| Stage 5 text | Adaptive: dark text when intensity < 0.45, white otherwise |
| Scrollbar CSS | Never use `*` wildcard (affects range inputs) |
| Range inputs | Always `.range-visible` class, never inline track styles |
| ProductToggle | Uses inline `style` with accent color for bg/border tints |

---

## 8. Delimiter Balance Checker
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

---

## 9. Gotchas
- Use `\x28` for literal parens in regex patterns (delimiter counter counts raw parens)
- `useState` with function calls must be after all hook declarations
- Portal-based tooltips needed for anything inside `overflow: hidden/auto` containers
- Framer Motion `AnimatePresence mode="wait"` requires unique `key` on children
- Landing tiles use fixed 360px pixel widths (immune to root font-size)
- `motion.button` closing tag must match opening (common copy-paste error)
