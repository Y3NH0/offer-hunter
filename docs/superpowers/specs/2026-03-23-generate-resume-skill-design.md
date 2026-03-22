# generate-resume Skill Design

## Overview

A Claude Code skill that generates tailored CV and Cover Letter (.tex + .pdf) in both Chinese and English, given resume materials, a job description, and a LaTeX template.

## Input

1. **JD source** (one of):
   - Notion job listing title (skill searches + fetches from "職缺整理" database)
   - Notion page ID or URL
   - Raw JD text pasted by user
2. **Optional flag**: `--skip-strategy` to bypass strategy approval and generate directly

## Process

### Step 1: Acquire JD
- If given a title: use `notion-search` to find the job listing, then `notion-fetch` to get full content
- If given a page ID/URL: directly `notion-fetch`
- If given raw text: use as-is

### Step 2: Read Materials
- Read all `.md` files under `material/` (experience, education, skills, soft-skills, competitions, extracurricular)
- Read all project files under `material/projects/`

### Step 3: Strategy Phase (skippable with `--skip-strategy`)
- Analyze JD requirements vs available materials
- Present a strategy recommendation:
  - Which experiences/projects to include and their priority order
  - Which bullet points to emphasize or rephrase
  - What to omit or de-emphasize
  - Key skills/keywords to highlight for ATS matching
  - Cover letter angle/narrative
- Wait for user approval or modifications

### Step 4: Generation Phase
- Based on the approved strategy (or auto-determined if skipped):
  - Generate `cv-zh.tex` and `cv-en.tex` from the LaTeX template
  - Generate `cover-letter-zh.tex` and `cover-letter-en.tex`
- Compile all 4 .tex files with `xelatex` to produce PDFs

### Step 5: Output
- Save all files to `docs/resumes/<company-position>/`

## Output Structure

```
docs/resumes/<company-position>/
├── cv-zh.tex
├── cv-zh.pdf
├── cv-en.tex
├── cv-en.pdf
├── cover-letter-zh.tex
├── cover-letter-zh.pdf
├── cover-letter-en.tex
└── cover-letter-en.pdf
```

Folder naming: lowercase, kebab-case derived from company + position (e.g., `appier-ai-engineer`).

## Prerequisites

- LaTeX template (user's custom Awesome-CV based template) exists in the repo
- XeLaTeX + CJK fonts installed on the system
- Notion MCP server connected (for Notion-based JD fetching)

## Key Conventions

- `material/` is the single source of truth for content — always read from here
- Material files contain both `(zh)` and `(en)` sections — use the appropriate language version
- Preserve the user's voice and style from the material files
- Tailor content to the JD, don't just dump everything
- Cover Letter should complement the CV, not repeat it
