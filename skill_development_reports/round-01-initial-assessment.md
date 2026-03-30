# Round 01 — Initial Assessment

> Date: 2026-03-31
> Evaluator: Claude (autonomous development session)

## Overview

Full evaluation of all skills, agents, and scaffolding in the Offer Hunter Claude Code plugin. Each component rated on: **clarity**, **completeness**, **robustness**, and **user experience**.

---

## Skill Ratings

| Component | Rating | Verdict |
|-----------|--------|---------|
| `setup-resume` | 7.5/10 | Solid structure, vague on template analysis, no validation |
| `generate-resume` | 7.0/10 | Good flow, but JD extraction and bilingual logic underspecified |
| `job-recon` | 8.5/10 | Best-written skill, minor gaps in fallback and privacy |
| `resume-reviewer` | 6.5/10 | Weakest — unverified CLI syntax, no prompt template, no scoring method |
| Material templates | 7.0/10 | Too minimal, regional bias, missing guidance |
| `plugin.json` | 7.0/10 | Functional but missing metadata |

---

## Detailed Findings

### 1. `skills/setup-resume/SKILL.md` — 7.5/10

**Strengths:**
- Clear 8-step sequential process
- Handles 3 template sources (local, Overleaf, GitHub)
- Good `.gitignore` recommendations
- Config schema is well-defined

**Issues (by severity):**

| Severity | Issue |
|----------|-------|
| High | Step 4 "Detect compilation method" is vague — no algorithm for inferring compiler from file content |
| High | No validation if template path doesn't exist or contains no `.tex` files |
| Medium | Overleaf auth guidance is hand-waved ("Guide the user if needed") |
| Medium | Step 2 copies `.example` files AND creates `.md` copies — instruction is confusing |
| Medium | No guidance on what to do if template is already a git submodule |
| Low | No mention of font requirements or LaTeX package dependencies |
| Low | No reconfiguration workflow — "reconfigure" just means start over |

**Missing capabilities:**
- No guided material collection — just dumps blank templates
- No validation that materials are filled in before allowing `/generate-resume`
- No support for updating template (re-pull from Overleaf/GitHub)

---

### 2. `skills/generate-resume/SKILL.md` — 7.0/10

**Strengths:**
- User approval gate before generation (Step 3)
- Strategy phase is well-structured (6 dimensions)
- Template learning approach (read all .tex/.cls/.sty)
- Clear output structure
- Handles bilingual output

**Issues (by severity):**

| Severity | Issue |
|----------|-------|
| High | JD extraction is a vague list — no algorithm for how to extract or what to prioritize |
| High | Bilingual detection heuristic undefined — "if materials contain bilingual content" but how is this determined? |
| High | Step 4 says "copy support files" but doesn't specify how to discover ALL needed files (fonts, images, .bst, etc.) |
| Medium | Strategy feedback loop: what happens if user requests changes? No iteration protocol |
| Medium | Compilation retry says "up to 3 attempts" but no strategy (fix what? how?) |
| Medium | No handling of WebFetch failures when fetching JD URL |
| Low | `--skip-strategy` flag mentioned but not explained (when should user use it?) |
| Low | No guidance on output file naming when company/position has special characters |

**Missing capabilities:**
- No diff/comparison with previous versions of resume for same company
- No way to regenerate just the cover letter or just the CV
- No dry-run mode (show what would be generated without compiling)

---

### 3. `skills/job-recon/SKILL.md` — 8.5/10

**Strengths:**
- Excellent broad-to-deep research framework
- 7 research dimensions with clear descriptions
- Multi-language search strategy
- Regional platform awareness (Global, Taiwan, China, Japan, Korea)
- Strict quality standards (source URLs, fact vs inference distinction)
- Well-structured output template
- Parallel query execution instruction

**Issues (by severity):**

| Severity | Issue |
|----------|-------|
| Medium | Says "at least 3-4 rounds" then defines 5 rounds — inconsistent |
| Medium | No fallback when regional platforms are inaccessible (PTT requires specific access, Maimai is gated) |
| Medium | No privacy guidance — how to handle sensitive personal info discovered during research |
| Low | No mention of research freshness — how to flag outdated information |
| Low | WebFetch failure fallback is vague ("try alternative URLs") |
| Low | No guidance on combining conflicting information from multiple sources |

**Missing capabilities:**
- No mechanism to update an existing research report with new findings
- No integration with `/generate-resume` (recon findings could inform strategy)

---

### 4. `agents/resume-reviewer.md` — 6.5/10

**Strengths:**
- 7-dimension evaluation framework is comprehensive
- Flexible review methods (Claude Code, Gemini, Claude CLI)
- Language-aware output
- Fallback logic (external tool fails → Claude Code)

**Issues (by severity):**

| Severity | Issue |
|----------|-------|
| **Critical** | Gemini CLI syntax unverified — `cat file \| gemini -p "prompt"` may not be correct |
| **Critical** | Claude CLI syntax unverified — `cat file \| claude -m model -p "prompt"` may not work |
| High | No review prompt template — "construct a review prompt" is too vague for external tools |
| High | No scoring algorithm — how to arrive at "X/10"? No rubric. |
| Medium | Tool detection is vague — "asking the user or detecting available tools" |
| Medium | No error handling for missing files or directories |
| Medium | No handling of PDF-only resumes (user might not have .tex source) |
| Low | Step 6 "repeat for other language versions" lacks detail on how to handle cross-language review |

**Missing capabilities:**
- No structured rubric for scoring (what's a 7 vs 8?)
- No actionable rewrite suggestions (just identifies problems)
- No comparison mode (compare two versions of the same resume)
- No ATS simulation (keyword match percentage)

---

### 5. Scaffolding Material Templates — 7.0/10

**`personal-info.md.example`** (7/10):
- Phone example `+886` is Taiwan-specific — should be generic
- Missing: personal website/portfolio, tagline/summary, social media links
- No guidance on what each field is used for

**`experience.md.example`** (7.5/10):
- Good bilingual structure with action verb reminder
- Missing: guidance on number of bullet points, metrics/impact emphasis
- Only one entry — unclear how to add multiple positions

**`education.md.example`** (6.5/10):
- Weakest template — only shows graduate level
- Missing: GPA, relevant coursework, honors/awards, scholarships
- Undergrad entry has no fields beyond period

**`skills.md.example`** (7/10):
- Good category structure
- Missing: proficiency levels, certification references
- "Core Competencies" is jargon — needs clarification

**`projects/project-template.md.example`** (7/10):
- Good basic structure
- Missing: links (GitHub, demo, paper), team size, role description
- No guidance on how many projects to include

---

### 6. `plugin.json` — 7.0/10

- Missing: `homepage`, `repository`, `license`, `keywords` fields
- Version 1.0.0 while still in active development

---

### 7. Evals Coverage

| Skill | Has Evals | Count | Edge Cases |
|-------|-----------|-------|------------|
| `job-recon` | Yes | 3 | No (all happy path) |
| `setup-resume` | No | 0 | N/A |
| `generate-resume` | No | 0 | N/A |

**Gaps:** No evals for 2 of 3 skills. Existing evals lack negative/edge cases (unknown person, company with no public info, WebFetch failures).

---

## Missing Skills

1. **Interview Slides** — previously existed, was removed during refactoring. Needs to be rebuilt as a Marp-based, template-agnostic skill.

---

## Priority Action Plan

### Phase 1: Fix Critical Issues
1. Rewrite `resume-reviewer.md` — verify CLI syntax, add prompt template, add scoring rubric
2. Improve `generate-resume` — define JD extraction, bilingual detection, compilation strategy

### Phase 2: Strengthen Existing Skills
3. Improve `setup-resume` — add validation, guided material collection, compiler detection algorithm
4. Polish `job-recon` — fix round count, add fallbacks, privacy note
5. Expand all material templates

### Phase 3: Add New Capabilities
6. Create `interview-slides` skill (Marp-based)
7. Add evals for `setup-resume` and `generate-resume`
8. Add edge case evals for `job-recon`

### Phase 4: Documentation
9. Update `plugin.json`, `CLAUDE.md`, READMEs
