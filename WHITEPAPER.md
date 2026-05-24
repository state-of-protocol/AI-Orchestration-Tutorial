## 📄 Fail Baharu: `WHITEPAPER.md`

```markdown
<pre>
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║                    ORCHESTRA AI-AGENT TUTORIAL                               ║
║                    Whitepaper & Technical Architecture                       ║
║                                                                              ║
║                    Version 1.0  ·  May 2026                                  ║
║                    State of Protocol                                         ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
</pre>

---

## 1. EXECUTIVE SUMMARY

```
Orchestra AI-Agent Tutorial is a dual-repository educational ecosystem designed
to teach developers how to build, orchestrate, and deploy autonomous AI agents
across three dominant platforms: Google AI Studio, Anthropic Claude, and
DeepSeek.

The project consists of two interconnected repositories:

  ┌─────────────────────────────────────────────────────────────┐
  │  REPO A: orchestra-website                                  │
  │  ─────────────────────────────────────────────────────────  │
  │  Role: Interactive Web Interface                            │
  │  URL:  https://state-of-protocol.github.io/orchestra-website│
  │  Contents: index.html, style.css, script.js,                │
  │            cost-calculator.html                             │
  │  Purpose: Serves as the "front door" — an engaging,         │
  │           interactive gateway with live demos, cost         │
  │           calculator, streaming agent simulation,           │
  │           comparison matrix, and Vibe Code Playground.      │
  └─────────────────────────────────────────────────────────────┘
                              │
                              │  Cross-links via footer & buttons
                              ▼
  ┌─────────────────────────────────────────────────────────────┐
  │  REPO B: AI-Orchestration-Tutorial                          │
  │  ─────────────────────────────────────────────────────────  │
  │  Role: Content & Documentation Hub                          │
  │  URL:  https://github.com/state-of-protocol/                │
  │        AI-Orchestration-Tutorial                            │
  │  Contents: 7 specification files (SKILL.md → USER_FLOW.md), │
  │            academy directories (google-ai-studio/,          │
  │            anthropic-claude/, deepseek/), boilerplate code. │
  │  Purpose: Serves as the "reference library" — in-depth      │
  │           technical specifications, coding rules, API       │
  │           schemas, architecture diagrams, and step-by-step  │
  │           learning paths for each platform.                 │
  └─────────────────────────────────────────────────────────────┘
```

---

## 2. PROJECT VISION & PHILOSOPHY

```
The core philosophy of Orchestra AI-Agent is:

  "Learning one platform does not teach you the others."

Every AI agent platform has a fundamentally different architecture:

  · Google AI Studio  — Managed sandbox (Antigravity)
  · Anthropic Claude  — Model Context Protocol (MCP)
  · DeepSeek          — Mixture-of-Experts (MoE) + Engram Memory

We do not attempt to unify them under one abstraction. Instead, we
respect their differences and teach each platform on its own terms.
Our 7-file specification framework ensures consistency while allowing
platform-specific depth.
```

---

## 3. TWO-REPOSITORY ARCHITECTURE RATIONALE

```
Why separate the website from the documentation?

  REASON 1: Separation of Concerns
  ─────────────────────────────────
  The website is a PRODUCT (interactive, visual, engaging).
  The documentation is a REFERENCE (detailed, technical, exhaustive).
  They have different update cycles, different audiences, and different
  maintenance requirements.

  REASON 2: GitHub Pages Optimization
  ────────────────────────────────────
  Repo orchestra-website:
    · Uses .nojekyll to serve raw HTML/CSS/JS
    · Optimized for browser rendering
    · Contains all interactive components

  Repo AI-Orchestration-Tutorial:
    · No HTML/CSS/JS needed
    · Rendered natively by GitHub's Markdown engine
    · Clean, readable, searchable via GitHub's interface

  REASON 3: Contributor Friendliness
  ────────────────────────────────────
  A contributor who wants to improve the DeepSeek tutorial does not
  need to understand the website's JavaScript. They only need to edit
  Markdown files in the deepseek/ directory.

  A contributor who wants to improve the UI does not need to read
  through all the tutorial content. They work exclusively in the
  orchestra-website repository.
```

---

## 4. CROSS-REFERENCE MAP: WEBSITE COMPONENTS → DOCUMENTATION

```
The website (orchestra-website) contains the following interactive sections.
Each section is linked to the corresponding technical documentation in the
AI-Orchestration-Tutorial repository.

┌─────────────────────────────────────────────────────────────────────────────┐
│  WEBSITE SECTION          │  PURPOSE                    │  DOC REFERENCE    │
├─────────────────────────────────────────────────────────────────────────────┤
│  Hero Section             │  Project introduction       │  README.md        │
│                           │  & value proposition        │                   │
├─────────────────────────────────────────────────────────────────────────────┤
│  Platform Cards           │  Overview of 3 platforms    │  Academy READMEs  │
│  (Google, Claude,         │  with key features          │  in each dir      │
│   DeepSeek)               │                             │                   │
├─────────────────────────────────────────────────────────────────────────────┤
│  Comparison Matrix        │  Feature-by-feature         │  ARCHITECTURE.md  │
│  (filterable table)       │  comparison across          │  (Section 5)      │
│                           │  platforms                  │                   │
├─────────────────────────────────────────────────────────────────────────────┤
│  Cost Calculator          │  Real-time token cost       │  API_SPEC.md      │
│  (interactive)            │  estimation (USD + MYR)     │  (pricing tables) │
├─────────────────────────────────────────────────────────────────────────────┤
│  Streaming Demo           │  Simulated agent thinking   │  ARCHITECTURE.md  │
│                           │  process visualization      │  (data flow)      │
├─────────────────────────────────────────────────────────────────────────────┤
│  Learning Paths           │  Phase 1→2→3 progression   │  USER_FLOW.md     │
│  (tabbed interface)       │  for each platform          │                   │
├─────────────────────────────────────────────────────────────────────────────┤
│  Architecture Diagram     │  Interactive node map of    │  ARCHITECTURE.md  │
│  (hover tooltips)         │  system components          │  (all sections)   │
├─────────────────────────────────────────────────────────────────────────────┤
│  Progress Tracker         │  localStorage-based         │  USER_FLOW.md     │
│                           │  completion checklist       │  (phase tracking) │
├─────────────────────────────────────────────────────────────────────────────┤
│  Vibe Code Playground     │  In-browser code editor     │  Boilerplate code │
│  (Cloud + Local modes)    │  with simulated agent run   │  in each academy  │
├─────────────────────────────────────────────────────────────────────────────┤
│  Footer (Documentation)   │  Direct links to all        │  All 7 spec files │
│                           │  7 specification files      │                   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 5. THE 7-FILE SPECIFICATION FRAMEWORK

```
Every academy follows an identical specification framework designed to
prevent scope creep and ensure production-grade quality.

  ┌──────────────┬──────────────────────────────────────────────────────────┐
  │  FILE         │  QUESTION ANSWERED                                      │
  ├──────────────┼──────────────────────────────────────────────────────────┤
  │  SKILL.md     │  "What do I need installed?"                            │
  │              │  Tech stack, core SDKs, runtime requirements, tools.    │
  ├──────────────┼──────────────────────────────────────────────────────────┤
  │  DESIGN.md    │  "How should the user experience feel?"                 │
  │              │  UI/UX principles, streaming patterns, component specs. │
  ├──────────────┼──────────────────────────────────────────────────────────┤
  │  STRUCTURE.md │  "Where does everything live?"                          │
  │              │  File hierarchy, folder conventions, config locations.  │
  ├──────────────┼──────────────────────────────────────────────────────────┤
  │  ARCHITECTURE │  "How do the pieces connect?"                           │
  │  .md          │  Data flow diagrams, component interactions, agent      │
  │              │  loops, sandbox lifecycle.                              │
  ├──────────────┼──────────────────────────────────────────────────────────┤
  │  API_SPEC.md  │  "What does the API contract look like?"                │
  │              │  Endpoints, request/response schemas, authentication.   │
  ├──────────────┼──────────────────────────────────────────────────────────┤
  │  RULES.md     │  "What must I NEVER do?"                                │
  │              │  Coding constraints, safety thresholds, platform-       │
  │              │  specific gotchas, security requirements.               │
  ├──────────────┼──────────────────────────────────────────────────────────┤
  │  USER_FLOW.md │  "What do I build first?"                               │
  │              │  Phased development roadmap (Phase 1 → 2 → 3) with      │
  │              │  step-by-step instructions and success criteria.        │
  └──────────────┴──────────────────────────────────────────────────────────┘
```

---

## 6. NAVIGATION FLOW: USER JOURNEY

```
A typical user journey through the Orchestra AI ecosystem:

  STEP 1: Discovery
  ─────────────────
  User finds orchestra-website via search, social media, or GitHub.
  They land on the interactive homepage with dark/light theme.

  STEP 2: Exploration
  ───────────────────
  User browses platform cards, experiments with the cost calculator,
  and watches the streaming demo to understand agent behavior.

  STEP 3: Platform Selection
  ──────────────────────────
  Using the comparison matrix filters, user identifies which platform
  suits their project (cost, precision, or scale).

  STEP 4: Deep Dive
  ─────────────────
  User clicks footer links or CTA buttons to access detailed
  specification files in AI-Orchestration-Tutorial.

  STEP 5: Hands-On Learning
  ──────────────────────────
  User follows USER_FLOW.md for their chosen platform, copying
  boilerplate code and running agents locally or in cloud mode.

  STEP 6: Vibe Coding
  ───────────────────
  User returns to the website and uses the Vibe Code Playground
  to edit and test agent code directly in the browser before
  committing to full local development.

  STEP 7: Contribution
  ────────────────────
  User submits improvements via GitHub Issues or Pull Requests,
  following the CONTRIBUTING.md guidelines.
```

---

## 7. DESIGN PRINCIPLES APPLIED

```
The website's design follows principles documented in DESIGN.md:

  · Process Transparency    — Every agent action is visible
  · Immediate Feedback      — Streaming tokens, not loading spinners
  · Visual Hierarchy        — CoT → Tool Call → Output
  · User Control            — Start, stop, reset at any time
  · Cost Awareness          — Real-time USD/MYR cost display
  · Accessibility           — WCAG 2.2 AA, keyboard navigation
  · Responsive              — 375px to 1440px+
  · Bilingual               — Bahasa Melayu & English
```

---

## 8. TECHNOLOGY STACK

```
  WEBSITE (orchestra-website)
  ────────────────────────────
  · HTML5 (semantic markup, ARIA attributes)
  · CSS3 (custom properties, dark/light theme, grid/flexbox)
  · Vanilla JavaScript (no framework — 15 interactive modules)
  · Canvas API (cost comparison chart)
  · localStorage (API keys, progress tracking, theme preference)
  · postMessage API (cross-component communication)

  CONTENT (AI-Orchestration-Tutorial)
  ────────────────────────────────────
  · Markdown (all specification files)
  · YAML (sandbox config, MCP config templates)
  · JSON (API schemas, pricing data)
  · Python & TypeScript (boilerplate code examples)
```

---

## 9. FUTURE ROADMAP

```
  Q3 2026:
  · Multi-repo federation via orchestra-hub
  · WebContainer-based real agent execution in browser
  · Community translations (中文, 日本語, Español)

  Q4 2026:
  · AI model evaluation dashboard
  · Integration with Google Antigravity 2.0 Managed Agents
  · Certification pathway for enterprise teams
```

---

<pre>
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║  This whitepaper serves as the definitive bridge between the interactive     ║
║  web experience (orchestra-website) and the in-depth technical documentation ║
║  (AI-Orchestration-Tutorial).                                                ║
║                                                                              ║
║  For questions, contributions, or community discussions:                     ║
║                                                                              ║
║  GitHub: https://github.com/state-of-protocol                                ║
║  Discord: https://discord.gg/example                                         ║
║                                                                              ║
║  Licensed under MIT. Built with ❤️ in Malaysia.                              ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
</pre>
```
