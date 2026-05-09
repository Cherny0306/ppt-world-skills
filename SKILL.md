---
name: ppt-world-skills
display_name: ppt-world-skills
version: 1.0.0
description: 通用结构化PPT生成（Pitch / Research / Hybrid / Update），包含自动提问、路由与决策树流程化的输出规范与模板。
description_en: Universal structured slide-deck skill (Pitch/Research/Hybrid/Update) with auto-clarifying questions, routing, and a decision-tree workflow.
tags:
  - ppt
  - slides
  - pitch
  - research
  - template
  - workflow
  - routing
language:
  primary: zh-CN
  secondary:
    - en
---

## 

# ppt-world-skills: Universal Structured Slide Deck Skill (Pitch / Research / Hybrid)

> Use case: Turn any project / research / product / platform capability into a **structured slide-deck output** (ready to paste into PPT/Keynote/Google Slides).  
> Supported deck types: **Pitch** (buy-in / funding / approval), **Research** (academic / project review), **Hybrid** (business value + methodological rigor), **Update** (weekly/monthly progress).  
> Deliverables: `Slide-by-slide structure (title + bullets + visuals + speaker notes)` + `One-page executive summary` + `Risks / next steps / asks (if applicable)`.  
> Core principles: **Conclusion-first, layered information, audience-driven, visual-first, reusable templates**.

---

## 1. Inputs (Unified Intake Form)

> Rule of thumb: You can still draft with incomplete info, but you must have at least **goal + audience + current evidence**.

### 1.1 Basics (Required)
- Topic / project name:
- Deck type (choose one): Pitch / Research / Hybrid / Update
- Audience (choose one or more): Executives｜Business｜Engineering｜Clients/partners｜Review committee
- Duration: 5 / 10 / 20 / 30 minutes (or slide limit)
- Desired final action: approve pilot / fund / allocate resources / partner / pass review / status sync

### 1.2 Content Materials (Fill at least 2)
- Background & problem (pain points / opportunity / research question):
- Solution / method (approach, roadmap, key modules):
- Progress & evidence (demo, data, experiments, user feedback, test results):
- Value & impact (cost saving, efficiency, risk reduction, academic contribution, societal impact):
- Risks & constraints (compliance, technical uncertainty, dependencies, data issues):
- Next steps & resource needs:

### 1.3 Optional Enhancers (High leverage)
- Competitors / benchmarks / related work:
- Target users / scenarios / customer list:
- Metrics (KPI / evaluation metrics / acceptance criteria):
- Budget framing (rough range or resource list):
- Diagrams / flowcharts / sample images (upload if available):

---

## 1.4 Auto Clarifying Questions on Trigger (Agent Behavior)

> Purpose: When user info is missing or ambiguous, the agent should **ask a small set of high-impact questions** first, then generate the structured deck to avoid rework.  
> Suggested implementation: after the skill is triggered, check required “slots”; if any are missing, call **AskUserQuestion** (max 4 questions per call) following the priority below.

### 1.4.1 Slot-check rules (check these first)

**A. Universal required slots (must ask if missing)**
1. Deck type: Pitch / Research / Hybrid / Update  
2. Audience: executives / business / engineering / clients / reviewers  
3. Duration or slide limit: 5/10/20/30 minutes (or slide limit)  
4. Desired decision/action: approve / resource / partner / pass review / status sync

**B. Type-specific required slots (must ask if missing)**
- Pitch: “Ask” (what support you want) + a value quantification baseline (at least 1–2 metrics/targets)  
- Research: research question/objective + data/experimental setup + evaluation metrics  
- Hybrid: decision goal (“Ask”) + credibility evidence (data / evaluation / human-in-the-loop review / cases)  
- Update: reporting cadence + 3 key takeaways + blockers & needed support

**C. Safe defaults (can assume if missing)**
- Visual style (tech / government / minimal) → default **minimal & executive**  
- Visual preference → default “**flowchart + comparison table + timeline**”  
- Q&A slide → default include for Pitch/Hybrid

### 1.4.2 Question priority (ask only what matters most)
1. Ask **deck type + audience** (drives structure and language)  
2. Ask **duration/slide limit + desired action** (drives density and final “Ask”)  
3. Ask **evidence + metrics** (drives credibility and quantification)  
4. Ask optional enhancers last (benchmarks/budget/style), and avoid if possible

---

## 1.5 Question Bank (ready for AskUserQuestion)

> How to use: pick 1–4 questions based on missing slots to form a single AskUserQuestion call.  
> Tips: keep options mutually exclusive and short; put the recommended option first.

### 1.5.1 Universal first round (start here)

**Q1 (Type)**: What type of deck do you want?  
- Pitch (Recommended): approval / funding / resources / partnership  
- Research: academic / project review  
- Hybrid: business value + methodological credibility  
- Update: weekly/monthly progress

**Q2 (Audience)**: Who is the primary audience?  
- Executives / leadership (Recommended)  
- Business stakeholders / clients  
- Engineering / R&D  
- Review committee / academic audience

**Q3 (Time)**: How long is the presentation?  
- 10 minutes (Recommended)  
- 5 minutes (elevator)  
- 20 minutes (standard)  
- 30 minutes (full review)

**Q4 (Action)**: What do you want them to do at the end?  
- Approve pilot / approve project (Recommended)  
- Allocate resources / budget / governance  
- Agree to partner / sign-off  
- Status update only / collect feedback

### 1.5.2 Pitch/Hybrid (fill “Ask” and value framing)

**Q (Support)**: What 3 support items should appear on the final “Ask” slide?  
- Positioning + governance + minimal resource package (Recommended)  
- Budget + compute/server + data/permissions  
- Pilot authorization + cross-team support + external endorsement  
- Other (user specifies)

**Q (Value axis)**: Which value should the deck emphasize most?  
- Efficiency & cost reduction (Recommended)  
- Risk shift-left / compliance & auditability  
- Revenue growth / productized service  
- Brand impact / demonstration value

### 1.5.3 Research (fill experiment framing)

**Q (Focus)**: Which part should be emphasized?  
- Method + results (Recommended)  
- Related work and differentiation  
- Dataset / labeling framework  
- Discussion, limitations & future work

**Q (Maturity)**: What is the current maturity of results?  
- Strong main results available (Recommended)  
- Early experiments / small sample  
- Design only / no experiments yet  
- Replication / comparison in progress

### 1.5.4 Update (fill reporting framing)

**Q (Cadence)**: What kind of progress update is this?  
- Weekly/current period (Recommended)  
- Monthly  
- Milestone review  
- Ad-hoc special update

---

## 1.6 Routing (Deck Router)

> Purpose: If the user doesn’t explicitly specify deck type / duration / audience, the agent should **auto-select the best backbone** and **auto-set slide count and information density**.  
> Principle: default to a **decision-ready** deck (Pitch/Hybrid) rather than producing fragmented material that executives can’t act on.

### 1.6.1 Deck-type routing rules (top-down; first match wins)

1. **Route to Update** if the user says weekly/monthly update, milestone review, status sync, blockers, or “what support is needed”.  
2. **Route to Pitch** if the goal is approval / funding / resources / partnership, and the audience is executive-heavy; emphasize value and decision.  
3. **Route to Research** if the goal is academic/project review, with experimental design, metrics, baselines, and result comparisons; audience is reviewers.  
4. **Route to Hybrid (default priority)** if the deck must both drive a decision and prove credibility (evaluation, evidence chain, governance), or if the audience mixes executives and technical/reviewers.

Quick heuristic:
- “Need a decision” → Pitch / Hybrid  
- “Need a review” → Research  
- “Need progress visibility” → Update  
- “Need decision + proof” → Hybrid

### 1.6.2 Audience routing (sets language level)

- **Executives**: decision language first (value, risk, governance, acceptance); tech details ≤20%  
- **Review committee**: rigor first (setup, baselines, metrics, ablation/error analysis)  
- **Business/clients**: scenarios & deliverables first (workflow change, service model, ROI)  
- **Engineering**: architecture & execution first (modules, interfaces, dependencies, data governance)

### 1.6.3 Duration → slide count routing (defaults)

- 5 min: 5–7 slides (elevator; only conclusions + ask/next)  
- 10 min: 8–10 slides (standard decision deck)  
- 20 min: 12–16 slides (solution + plan)  
- 30 min: 16–24 slides (full review)

### 1.6.4 Maturity routing (how deep to show “proof”)

- **Demo / strong results**: must include demo/cases + metrics + error/risk slide  
- **Early results**: emphasize pilot design + acceptance metrics + risk control; avoid over-claiming  
- **Design only**: emphasize why the design + MVP pilot + milestones + resource ask

---

## 1.7 Decision-tree Workflow (Processized Execution)

> Purpose: Make execution **repeatable, automatable, and quality-stable**.

### 1.7.1 Decision tree (text)

```
Start
 ├─(1) Intake (Section 1.1–1.3)
 ├─(2) Slot check (1.4.1)
 │    ├─Missing critical slots? → Yes: AskUserQuestion (≤4) → back to (2)
 │    └─No
 ├─(3) Routing (1.6)
 │    ├─Deck type: Pitch / Research / Hybrid / Update
 │    ├─Audience language level
 │    └─Duration → slide count & density
 ├─(4) Select backbone (Section 2)
 ├─(5) Build one-page executive summary (Why/What/Proof/Value/Ask)
 ├─(6) Generate slide-by-slide output (≤3 bullets + visuals + speaker notes)
 ├─(7) Enforce core visuals (at least: flow + comparison + timeline)
 ├─(8) Quality check (Section 7)
 │    ├─Fail → refine slides (go back to 6)
 │    └─Pass
 └─Deliver structured deck
End
```

### 1.7.2 Minimal pseudo-code (engineering-friendly)

```
if missing(required_slots):
  ask_up_to_4_questions()
  return

deck_type = router(deck_type, audience, desired_action, content_shape)
duration = normalize(duration_or_slide_limit)
slides = map_duration_to_slide_count(duration, deck_type)

backbone = select_backbone(deck_type)
deck = build_exec_summary(deck_type)
deck += build_slides(backbone, slides, constraints={max_bullets:3})
deck = enforce_visuals(deck, ["flow", "comparison", "timeline"])
deck = quality_check_and_fix(deck)
output(deck)
```

## 2. Choose a Structural Backbone by Deck Type

> Choose the backbone first, then fill content. The same material can be re-shaped into different deck types quickly.

### 2.1 Pitch backbone

**Goal**: Make decision-makers believe in the direction, see the value, trust the path, and approve/allocate resources.

Suggested size: 8–10 slides for 10 minutes; 12–16 slides for 20 minutes.

Required modules (in order):
1. Cover (title + one-line positioning)
2. One-page executive summary (Why/What/Value/Ask)
3. Problem & timing (pain points, trends, why now)
4. Solution (one-liner + architecture/flow)
5. Key differentiators (moat, unique capability)
6. Quantified value (KPI/ROI/efficiency/risk)
7. Delivery plan (milestones, pilot scope, acceptance criteria)
8. Risks & mitigations (confidence building)
9. Ask (3 items: positioning / governance / minimal resources)
10. Backup Q&A (common questions & talking points)

### 2.2 Research backbone

**Goal**: Demonstrate a clear research question, rigorous method, credible results, and clear contributions.

Suggested size: 12–18 slides for 20 minutes; 16–24 slides for 30 minutes.

Required modules:
1. Title (topic, authors, affiliation, date)
2. Abstract (contributions + key results on one slide)
3. Background & related work (context, gaps, research question)
4. Objectives & definitions (metrics, constraints, scope)
5. Method overview (framework diagram)
6. Method details (1–3 slides: key modules)
7. Data & experimental setup (data, splits, metrics, baselines)
8. Results (tables/curves/ablation/comparisons)
9. Analysis & discussion (error analysis, cases, failure modes)
10. Conclusion & future work (limitations, next steps, reproducibility)

### 2.3 Hybrid backbone (recommended for AI/platform systems)

**Goal**: Explain value & delivery, while also proving credibility & governance.

Suggested size: 14–20 slides for 20 minutes.

Recommended order:
1. Cover
2. One-page executive summary (Why/What/Value/Ask)
3. Background & pain points
4. Needs & scenarios (users/customers)
5. Solution overview (architecture)
6. Credibility layer (data, evaluation, human-in-the-loop, evidence chain)
7. Demo/cases (before-after, evidence)
8. Quantified value
9. Delivery plan (milestones + acceptance)
10. Risk & governance (compliance, auditability)
11. Ask / partnership model

### 2.4 Update backbone

**Goal**: Help stakeholders quickly understand progress, risks, and next steps.

Suggested size: 5–8 slides.

1. This period’s conclusions (top 3)
2. Key progress (by objective)
3. Metrics (trend)
4. Risks & blockers (who needs to decide)
5. Next plan (milestones)
6. Needed support (who/what/when)

---

## 3. Slide-by-slide Structured Output Format (Delivery Contract)

For each slide, output:
- **Slide # & Title** (a conclusion-style title)
- **Purpose** (what decision/judgment this slide creates)
- **Bullets (≤3)** (each ≤ ~10 words)
- **Recommended visual** (flowchart / table / quadrant / timeline / heatmap)
- **Speaker notes (30–60s)** (conclusion first, then explanation)
- **Optional notes** (sources, definitions, backup)

---

## 4. Reusable Visual Component Library

### 4.1 One-page executive summary (universal)
- Why  
- What  
- Proof  
- Value  
- Ask

### 4.2 Common visual templates
- 2×2 quadrant (open-source vs commercial, short-term vs long-term, risk vs reward)
- Comparison table (baseline vs new approach: cost/efficiency/risk/coverage)
- Flowchart (input → processing → output; emphasize auditability)
- Timeline / Gantt (milestones & deliverables)
- Funnel (leads → pilot → replication → scale)
- Evidence-card (claim + sources + similar cases + human review)
- KPI dashboard (coverage, accuracy, cycle time reduction, asset growth)

---

## 5. Writing & Communication Rules (stay “on point”)

### 5.1 Conclusion-first titles
- Bad: “Solution Overview”  
- Good: “Solution is pilot-ready and can be rolled out incrementally”

### 5.2 One slide = one judgment
- Pitch: direction / investment-worthiness / feasibility / ask  
- Research: importance / novelty / rigor / credibility  
- Hybrid: value / credibility / deliverability

### 5.3 Prefer decision language over jargon
Use: efficiency, cost, risk, compliance, coverage, acceptance, replication, governance, resources.

---

## 6. Fast Fill Guide for Common Slide Types

- Problem: status → contradiction → consequence  
- Solution: one-liner → 3-layer architecture → main flow  
- Value: internal (cost/efficiency/risk) + external (growth/impact) + metrics  
- Plan: milestones + deliverables + acceptance criteria  
- Risk: top ≤4 risks + mitigations  
- Ask: positioning + governance + minimal resource package

---

## 7. Quality Checklist (before delivery)

- Structure matches deck type  
- ≤3 bullets per slide and speakable  
- ≥3 measurable metrics (KPI or evaluation metrics)  
- Must include: 1 flowchart + 1 comparison table + 1 timeline (or equivalents)

---

## 8. Prompt Templates (copy/paste)

### 8.1 Pitch
Using the structured slide format, generate a 10-minute Pitch deck (8–10 slides). Include slide titles, ≤3 bullets, visual suggestions, and 30–60s speaker notes. End with a clear “Ask” slide (3 support items):
- Topic:
- Audience:
- Pain points:
- Solution overview:
- Evidence/progress:
- Value (quantified if possible):
- Risks & mitigations:
- Next steps & resource needs:

### 8.2 Research
Generate a 20-minute Research deck (12–18 slides) using the Research backbone. Emphasize: research question, related work, method framework, experimental setup, main results, ablation/comparisons, error analysis, conclusions & future work. Output slide-by-slide structure.

### 8.3 Hybrid
Generate a 20-minute Hybrid deck (14–20 slides). Requirements:
1) First 3 slides are executive decision slides (Why/What/Value/Ask)  
2) Middle section proves credibility (data/evaluation/human review/evidence chain)  
3) Final section gives milestones + acceptance metrics  
Output slide-by-slide structure and add one Q&A backup slide with 5 questions.
