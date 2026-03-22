---
name: generate-resume
description: Generate tailored CV and Cover Letter (.tex + .pdf) in Chinese and English for a specific job listing. Trigger when user says "幫我生成履歷", "投這個職缺", "generate resume for", or provides a job listing title/URL/JD text.
---

# Generate Tailored Resume & Cover Letter

Generate a customized CV and Cover Letter in both Chinese and English, tailored to a specific job description.

## Input

The user provides ONE of the following as the JD source:
1. **Notion job title** — search "職缺整理" database and fetch content
2. **Notion page ID or URL** — directly fetch
3. **Raw JD text** — use as-is

Optional flag: `--skip-strategy` to bypass the strategy approval step.

If the input is ambiguous, ask the user to clarify.

## Process

### Step 1: Acquire JD

- **If Notion title**: Use `mcp__plugin_Notion_notion__notion-search` with the title, then `mcp__plugin_Notion_notion__notion-fetch` to get the full page content.
- **If Notion page ID/URL**: Use `mcp__plugin_Notion_notion__notion-fetch` directly.
- **If raw text**: Use the provided text as-is.

Extract from the JD:
- Company name
- Position title
- Key requirements and qualifications
- Tech stack mentioned
- Any specific keywords or themes

### Step 2: Read Materials

Read ALL of the following from the project's `material/` directory:
- `material/experience.md`
- `material/education.md`
- `material/skills.md`
- `material/soft-skills.md`
- `material/competitions.md`
- `material/extracurricular.md`
- All files in `material/projects/`

### Step 3: Strategy Phase

**Skip this step if user passed `--skip-strategy`.**

Analyze the JD against the materials and present a strategy:

1. **Relevance mapping**: Which experiences/projects are most relevant and why
2. **Include list**: What to put in the CV, in priority order
3. **Emphasis**: Which bullet points to highlight or rephrase to match JD keywords
4. **Omit/de-emphasize**: What to leave out or minimize
5. **ATS keywords**: Key terms from the JD that should appear in the CV
6. **Cover Letter angle**: The narrative arc — what story to tell, what to emphasize

Present this as a clear, scannable summary and **wait for the user to approve or request changes** before proceeding.

### Step 4: Generate .tex Files

Based on the approved strategy (or auto-determined if skipped):

1. **Determine output folder name**: lowercase kebab-case from company + position (e.g., `appier-ai-engineer`)
2. **Create output directory**: `docs/resumes/<company-position>/`
3. **Read the LaTeX template** from the repo's template directory
4. **Generate 4 .tex files**:
   - `cv-zh.tex` — Chinese CV
   - `cv-en.tex` — English CV
   - `cover-letter-zh.tex` — Chinese Cover Letter
   - `cover-letter-en.tex` — English Cover Letter

Key principles for generation:
- Use the `(zh)` sections from material for Chinese, `(en)` sections for English
- Tailor content to the JD — don't dump everything
- Preserve the user's voice and phrasing from materials, only adjust for relevance and emphasis
- Cover Letter should complement the CV, not repeat it — tell a story about fit
- Ensure ATS-relevant keywords from the JD appear naturally

### Step 5: Compile PDFs

Run `xelatex` for each .tex file:

```bash
cd docs/resumes/<company-position>/
xelatex cv-zh.tex
xelatex cv-en.tex
xelatex cover-letter-zh.tex
xelatex cover-letter-en.tex
```

If compilation fails, diagnose and fix the .tex source, then retry.

### Step 6: Summary

Present the user with:
- Output folder path
- List of generated files
- A brief note on what was tailored (key choices made)

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

## Prerequisites

- LaTeX template must exist in the repo
- XeLaTeX + CJK fonts must be installed
- Notion MCP server must be connected (only needed for Notion-based JD fetching)
