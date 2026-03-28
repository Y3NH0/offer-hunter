---
name: generate-resume
description: >
  Generate tailored CV and Cover Letter (.tex + .pdf) for a specific job listing.
  Trigger when user says "generate resume", "create CV", "apply to this job",
  or provides a job listing title/URL/JD text.
---

# Generate Tailored Resume & Cover Letter

Generate a customized CV and Cover Letter tailored to a specific job description, using the user's configured LaTeX template.

## Input

The user provides ONE of the following as the JD source:
1. **URL** — fetch the page content via WebFetch and extract the JD
2. **Raw JD text** — use as-is

Optional flag: `--skip-strategy` to bypass the strategy approval step.

If the input is ambiguous, ask the user to clarify.

## Process

### Step 0: Check Configuration

Read `.claude/resume-config.json` from the project root.

- **If missing**: Tell the user to run `/setup-resume` first, then stop.
- **If exists**: Load the template path, compiler, and output directory settings.

### Step 1: Acquire JD

- **If URL**: Use WebFetch to retrieve the page content, then extract the JD text.
- **If raw text**: Use the provided text as-is.

Extract from the JD:
- Company name
- Position title
- Key requirements and qualifications
- Tech stack mentioned
- Any specific keywords or themes

### Step 2: Read Materials

Read ALL material files from the configured `material_dir` (default: `material/`):
- `personal-info.md`
- `experience.md`
- `education.md`
- `skills.md`
- `soft-skills.md` (if exists)
- `competitions.md` (if exists)
- `extracurricular.md` (if exists)
- All `.md` files in `projects/` subdirectory

Skip any files that don't exist — not all are required.

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

### Step 4: Learn Template & Generate .tex Files

Based on the approved strategy (or auto-determined if skipped):

1. **Determine output folder name**: lowercase kebab-case from company + position (e.g., `appier-ai-engineer`)
2. **Create output directory**: `<output_dir>/<company-position>/`
3. **Read the LaTeX template** from the configured `template.path`:
   - Read ALL `.tex`, `.cls`, `.sty` files to understand the template's structure, commands, and conventions
   - Pay special attention to: document class, custom commands (e.g., `\cventry`, `\cvsection`), section structure, and styling
   - Read section files if `template.section_dir` is configured
4. **Copy support files** into the output directory:
   - Copy all files listed in `template.class_files` and `template.preamble_files`
   - Copy any other support files the template needs (fonts, images, etc.)
5. **Generate .tex files** that follow the template's conventions:
   - Generate CV file(s) using the template's document structure and commands
   - Generate Cover Letter file(s) if the template supports it
   - If the user's materials contain bilingual content (zh/en sections), generate both language versions

Key principles for generation:
- **Follow the template**: Use the exact commands and structure defined in the template — do not invent LaTeX commands
- **Tailor content to the JD**: Don't dump everything — select and prioritize based on the strategy
- **Preserve the user's voice**: Use phrasing from materials, only adjust for relevance and emphasis
- **Cover Letter complements CV**: Tell a story about fit, don't repeat the CV
- **ATS keywords**: Ensure key terms from the JD appear naturally

### Step 5: Compile PDFs

Run the configured compiler for each .tex file:

```bash
cd <output_dir>/<company-position>/
<compiler> <cv-file>.tex
<compiler> <cover-letter-file>.tex
```

If compilation fails:
1. Read the error log
2. Diagnose and fix the .tex source
3. Retry (up to 3 attempts)

### Step 6: Summary

Present the user with:
- Output folder path
- List of generated files (with file sizes)
- A brief note on what was tailored (key choices made)

## Output Structure

```
<output_dir>/<company-position>/
├── cv-zh.tex / cv-zh.pdf        (if bilingual)
├── cv-en.tex / cv-en.pdf        (if bilingual)
├── cv.tex / cv.pdf              (if single language)
├── cover-letter-zh.tex / .pdf   (if bilingual)
├── cover-letter-en.tex / .pdf   (if bilingual)
├── cover-letter.tex / .pdf      (if single language)
└── <support files>               (.cls, preamble, etc.)
```

## Prerequisites

- Run `/setup-resume` first to configure template and materials
- LaTeX compiler installed (xelatex, pdflatex, or lualatex as configured)
- Appropriate fonts installed for the chosen template
