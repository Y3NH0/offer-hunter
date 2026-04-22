---
name: interview-strategy
description: >
  Build a comprehensive interview strategy document (JD alignment, slide skeleton,
  through-line script, Fit Analysis + 30/60/90 ramp plan, 誘餌地圖 × Q&A prep,
  behavioral STAR prep, risk mitigation) for a specific job interview.
  Trigger when user says "面試策略", "interview strategy", "擬定面試",
  "準備面試", "prepare for interview", or provides a JD and asks for
  interview preparation (not slide generation).
---

# Interview Strategy Builder

Produce a **strategy document** (not slides) for a specific job interview, combining:

1. JD ↔ user material alignment
2. Positioning + Hook design
3. Slide skeleton (content, not implementation)
4. Through-line 口頭銜接 script
5. Fit Analysis + 30/60/90 Ramp Plan for any JD gaps
6. 誘餌地圖 × Q&A technical追問 preparation
7. Behavioral STAR preparation
8. Risk & mitigation
9. Execution checklist

The output is consumed by the user (for interview prep) and can seed the `/interview-slides` skill (for actual deck implementation).

## When to Use This Skill vs. `/interview-slides`

| User Intent | Use |
|-------------|-----|
| 「我要準備 X 公司的面試」、「擬定面試策略」、「幫我分析這個 JD 怎麼打」 | **This skill** — strategy first |
| 「幫我做 Marp 簡報」、「生成 slides」、「把策略做成簡報」 | `/interview-slides` — implementation |
| 「研究這家公司的團隊」、「調查面試官」 | `/job-recon` — external research |

Always do strategy (this skill) BEFORE slides. Strategy informs slides; slides without strategy produce bland output.

---

## Input

The user provides ONE of:

1. **Existing JD path** — e.g. `docs/resumes/{company-position}/jd.md` (from prior `/generate-resume`)
2. **Company-position identifier** — e.g. `novatek-ai-application-engineer-it`
3. **URL** — fetch with WebFetch
4. **Raw JD text** — use as-is

Optional:
- **Interview stage** — 1st-tech / 2nd-manager / final / unspecified
- **Duration** — default 10 min
- **Language preference** — default: slide English + spoken Chinese (for Taiwan tech positions, see `feedback_slide_language.md` memory if available)
- **Special constraints** — e.g. "without speaker notes", "panel of 3", etc.

---

## Process

### Step 0: Prerequisites

1. Verify `material/` exists with at least `experience.md`, `education.md`, `skills.md`. If missing → stop, tell user to run `/setup-resume`.
2. Check for auto-memory at `~/.claude/projects/<project>/memory/` — look for:
   - Presentation direction (e.g. `project_presentation_direction.md`)
   - User gaps (e.g. `user_frontend_gap.md`)
   - Language conventions (e.g. `feedback_slide_language.md`)
3. Check for existing research at `docs/research/` — reuse `core-messaging-draft.md`, `slide-outline.md` if present.
4. Check for existing recon at `docs/research/{company}-*.md`.

### Step 1: Acquire JD

- **If path given**: read the file.
- **If identifier**: look in `docs/resumes/{identifier}/jd.md`.
- **If URL**: WebFetch; on failure, ask user to paste.
- **If raw text**: use as-is.

Save a normalized copy to `docs/resumes/{company-position}/jd.md` if not already there.

### Step 2: JD Parse + Alignment Analysis

Extract from JD:
- **Department/team context** — e.g. IT department, product team, research team. This often changes the whole angle (see `CLAUDE.md` guidance about Novatek IT = internal AI deployment).
- **職掌 / Duties** — numbered items
- **必要條件 / Must-have requirements** — numbered items
- **加分條件 / Nice-to-have** — numbered items

Build a **JD ↔ user material alignment table**:

```
| JD item              | Type      | User's evidence          | Match |
|----------------------|-----------|--------------------------|-------|
| Duty 1: ...          | Duty      | {project / experience}   | ⭐⭐⭐⭐⭐ |
| Duty 2: ...          | Duty      | ...                      | ⭐⭐⭐⭐ |
| Must-have 1: ...     | Required  | ⚠️ Gap — {reality}        | ⚠️    |
| ...                  | ...       | ...                      | ...   |
| Bonus 1: ...         | Bonus     | ...                      | ⭐⭐⭐⭐⭐ |
```

**Explicitly flag any gaps in must-haves** — these are the gap list that Fit Analysis must address later.

### Step 3: Web Research (Lightweight)

Quick WebSearch on:
- Company interview experiences ("{company} 面試心得")
- Team/department specifics (if unclear from JD)
- Interviewer style / culture signals

Goal: 3-5 insights that inform strategy. Examples:
- Typical interview format (X min slides + Y min Q&A + Z min intro)
- Interviewer priorities ("重視心態", "技術深度高", etc.)
- Specific technical追問 patterns

If user has done `/job-recon` already, lean on that instead.

### Step 4: Clarifying Questions — One at a Time

Ask these in sequence (don't bundle). Use multiple-choice when possible.

**Q1 — Structure preference.** Offer:
- A. Traditional résumé-style (Skills → Experience → Education → Projects → Role → Summary) — safe, familiar
- B. Hook + narrative (existing positioning direction) — differentiated but floral
- C. **Hybrid** (Hook 30s + traditional structure + Fit Analysis closer) — usually recommended
- D. Other

**Q2 — Interview stage + duration.**
- 1st-round technical (usually 10 min + heavy tech Q&A)
- 2nd-round manager (usually focus on character + motivation)
- Unspecified / both
- Other duration

**Q3 — Gap handling for any must-have gap(s) identified in Step 2.**
- A. Traditional SWOT (4 quadrants) — gives weakness too much stage
- B. **Fit Analysis + 30/60/90 Ramp Plan** (recommended) — forward-looking, concrete
- C. Don't address in slides, prep Q&A answer only — risks being caught cold
- D. Other

Each question should:
- **Offer a recommendation with reasoning**, not just list options
- **Ask one at a time** — wait for answer before next

### Step 5: Propose 2-3 Skeleton Variants

Based on confirmed structure (Step 4 Q1) + duration, produce 2-3 skeletons with time allocations. Always include:
- **One safe variant** (fully traditional)
- **One differentiated variant** (fully narrative)
- **One hybrid** (recommended when structure = C)

When user says "the options are too far apart", offer **a blended V1.5** that preserves the safer structure while injecting narrative elements via verbal speaker notes (not additional slides).

Each variant should show:
- Slide count + time budget per slide
- What's unique about it
- Pros / cons vs. other variants

### Step 6: User Selects Skeleton

After user picks, confirm the final skeleton table: `{#, Slide, Duration, Purpose}`.

### Step 7: Produce Full Design

Write the strategy doc at `docs/resumes/{company-position}/interview-strategy.md` with these sections:

#### Required sections

1. **Metadata header** — date, JD link, stage, duration, positioning, skeleton version
2. **JD 解讀與核心觀察** — department context, interview format from research, user-highlighted JD items
3. **JD ↔ user material 對齊表** — the table from Step 2
4. **核心策略** —
   - 一句話定位
   - Through-line (the red thread)
   - Hook (5A format if applicable: reversal + positioning)
   - 混合式執行原則 (if Hybrid)
   - **Language strategy** (slide language + spoken language)
   - Through-line verbal-bridge points (where伏筆 / 揭曉 / 回扣 happen across slides)
5. **簡報骨架 — Slide-by-Slide** — for each slide:
   - **視覺 (English or as-chosen)** — bullet/block layout of what's ON the slide
   - **口說 (spoken-language)** — verbatim speaker script骨架
   - **誘餌** — items the interviewer could追問
   - Time budget
6. **誘餌地圖 × Q&A 技術追問準備** — table mapping every誘餌 to likely questions + user's prep direction
7. **Behavioral / 個人特質準備** — STAR skeletons for common questions:
   - "遇到挫折怎麼處理"
   - "如何快速學習新技術"
   - "跟同事意見不合怎麼處理"
   - "為什麼離開現職 / 為什麼加入 X 公司"
   - Add more based on company signals (e.g. Novatek values 面對問題的心態)
8. **風險與預案** — table of risks + mitigations
9. **執行 Checklist** — concrete actions the user needs to do (補案例 / survey domain / 排練)
10. **成功指標** — what "good" looks like for this interview
11. **素材來源對照** — traceability back to `material/`, `docs/research/`, etc.

### Step 8: Self-Review

Before committing:
- **Placeholder scan**: TBD / 待補 / TODO — convert to explicit user-action items in the Checklist section
- **Consistency**: JD mapping in Section 3 must match references in Section 5 (每張 slide 對到哪條 JD)
- **Through-line check**: 伏筆 → 揭曉 → 回扣 should actually connect
- **Language check**: slides in chosen language, speaker script in chosen language
- **Scope check**: 10 min of content fits in 9 min with 1 min buffer (no overallocation)

Fix inline, no second review needed.

### Step 9: Commit

```bash
git add docs/resumes/{company-position}/interview-strategy.md
git commit -m "feat(docs): add {company} {position} interview strategy"
```

### Step 10: Present Summary to User

```
Interview strategy complete.

  File:     docs/resumes/{company-position}/interview-strategy.md
  Skeleton: {N} slides (~{duration} delivery)
  Language: {slide lang} + {spoken lang}
  Structure: {A/B/C/V1.5 variant}

Key decisions:
  - Through-line: {...}
  - Hook: {...}
  - Gap handling: {B - 30/60/90 for {skill}}

What you need to补 (see Section 9):
  - {specific user action 1}
  - {specific user action 2}
  - ...

Next steps:
  - Review the strategy, request changes
  - Run /interview-slides {company-position} to build the actual deck
  - Run @resume-reviewer to cross-check CV alignment
```

---

## Output Structure

```
docs/resumes/{company-position}/
├── jd.md                     (from /generate-resume or new)
├── cv-*.tex / .pdf           (from /generate-resume)
├── cover-letter-*.tex / .pdf (from /generate-resume)
└── interview-strategy.md     (← this skill's output)
```

---

## Design Principles

These are the non-negotiables when producing the strategy. They reflect what made the reference Novatek execution effective.

### 1. One question at a time

Never bundle multiple clarifying questions. Each question must have options + recommendation. Wait for answer before next.

### 2. Offer a blended middle option

When user pushes back saying "A and B are too far apart", propose a **V1.5** that keeps the safer structure and injects differentiation via verbal bridges (speaker notes) rather than new slides.

### 3. Map every JD item

Every must-have and bonus in the JD should have an explicit cell in the alignment table. Gaps flagged as ⚠️. No quiet omissions.

### 4. Through-line is a verbal thread, not a slide

Don't waste a slide on "my through-line". Weave it through 3 moments:
- **伏筆 (seed)**: at the end of an earlier slide, hint that a pattern will recur
- **揭曉 (reveal)**: at the connecting slide, make the pattern explicit
- **回扣 (callback)**: at a later slide, confirm the recurrence

Each moment should be in the speaker script, not the slide visual.

### 5. Treat gaps as Fit Analysis, not SWOT

SWOT's 4-quadrant format gives weaknesses too much stage. A single "Fit Analysis + 30/60/90 Ramp Plan" slide:
- Maps strengths to JD items (positive frame)
- States the gap honestly in one line
- Spends most of the space on the concrete 30/60/90 plan (forward-looking)

### 6. Language strategy: slide + spoken

For Taiwan tech interviews, the default is:
- **Slide visuals in English** — technical keywords are English-native, looks professional, internationally portable
- **Spoken content in Chinese** — natural, richer emotional layers, matches local interview culture

User can override. Always document the choice in the strategy.

### 7. 誘餌地圖 is part of the strategy

A slide with interesting details is a **prompt for the interviewer to追問**. For every such detail, prepare:
- What they're likely to ask
- Your 1-2 minute prepared answer
- Deeper version if they drill further

A strategy without誘餌地圖 is half a strategy.

### 8. Speaker notes for every verbal bridge

Where the speaker script carries key content (through-line verbal bridges, hook, closing), include it verbatim in the strategy. User will rehearse from this, not invent it on the spot.

### 9. Checklist > prose

End with an action list of concrete things the user must do (補案例 / survey domain / rehearse timing). Prose recommendations get lost; checkboxes get done.

### 10. Commit the strategy

The strategy is a first-class artifact. It lives in version control alongside the CV, so future you can diff iterations and learn what worked.

---

## Prerequisites

- `material/` filled in (run `/setup-resume` if not)
- JD available (from `/generate-resume` or provided inline)
- Optional but recommended:
  - `/generate-resume` already run (CV + jd.md in place)
  - `/job-recon` run if company/team context is unclear
  - Existing positioning direction in `docs/research/` or auto-memory

## Reference Execution

See `docs/resumes/novatek-ai-application-engineer-it/interview-strategy.md` in a repo that uses this skill for a full example of the output format and depth.
