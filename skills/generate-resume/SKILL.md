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

Optional flags:
- `--skip-strategy` — bypass the strategy approval step (useful when re-generating with minor tweaks)
- `--cv-only` — generate only the CV, skip cover letter
- `--cover-letter-only` — generate only the cover letter, skip CV

If the input is ambiguous, ask the user to clarify.

## Process

### Step 0: Check Configuration

Read `.claude/resume-config.json` from the project root.

- **If missing**: Tell the user to run `/setup-resume` first, then stop.
- **If exists**: Load the template path, compiler, and output directory settings. Verify the template path still exists on disk — if it doesn't, inform the user and stop.

### Step 1: Acquire & Analyze JD

**Fetching:**
- **If URL**: Use WebFetch to retrieve the page content. If WebFetch fails (timeout, 403, redirect), inform the user and ask them to paste the JD text directly.
- **If raw text**: Use the provided text as-is.

**Extraction — pull these fields from the JD:**

| Field | What to Extract | Priority |
|-------|----------------|----------|
| Company name | Official company name | Required |
| Position title | Exact title from listing | Required |
| Hard requirements | "Must have" qualifications, years of experience, degrees | Required |
| Preferred qualifications | "Nice to have" items | Required |
| Tech stack | Languages, frameworks, tools, platforms mentioned | Required |
| Responsibilities | Key job duties | Required |
| ATS keywords | Repeated terms, domain jargon, specific tools | Required |
| Team/department | Which team or org the role belongs to | If available |
| Company values/culture | Signals about what the company values | If available |
| Salary range | Compensation information | If available |

**Save the JD** as `jd.md` in the output directory for future reference (used by `@resume-reviewer`).

### Step 2: Read Materials

Read ALL material files from the configured `material_dir` (default: `material/`):

**Required files** (warn if missing):
- `personal-info.md` — contact info, name
- `experience.md` — work history
- `education.md` — degrees, coursework

**Optional files** (skip silently if missing):
- `skills.md` — technical skills and categories
- `soft-skills.md` — interpersonal and leadership skills
- `competitions.md` — awards, hackathons, competitions
- `extracurricular.md` — volunteering, community involvement
- All `.md` files in `projects/` subdirectory

**If ALL required files are missing**: Stop and tell the user to fill in their materials first (`material/*.md`). Suggest running `/setup-resume` if the directory doesn't exist.

**If required files exist but are empty or still contain only example placeholders**: Warn the user that materials appear incomplete and ask if they want to proceed anyway.

### Step 3: Strategy Phase

**Skip this step if user passed `--skip-strategy`.**

Analyze the JD against the user's materials and present a strategy:

#### 3a: Relevance Analysis

For each experience/project in the user's materials, assign a relevance score:
- **High**: Directly matches a JD requirement (same tech, same domain, same responsibility)
- **Medium**: Transferable skills or adjacent experience
- **Low**: Not relevant to this specific role

#### 3b: Strategy Summary

Present this summary and **wait for user approval**:

```
Strategy for: {position} @ {company}

INCLUDE (high relevance):
  - {experience 1} — matches: {JD requirement}
  - {project 1} — matches: {JD requirement}

EMPHASIZE (rephrase to match JD):
  - {bullet point} → rephrase to highlight {keyword}

DE-EMPHASIZE or OMIT:
  - {experience} — not relevant to this role

ATS KEYWORDS to weave in:
  - keyword1, keyword2, keyword3, ...

COVER LETTER ANGLE:
  - Opening hook: {angle}
  - Key narrative: {story connecting user's experience to role}
  - Closing: {call to action / enthusiasm}
```

#### 3c: User Feedback Loop

After presenting the strategy:
- If the user approves → proceed to Step 4
- If the user requests changes → update the strategy accordingly and re-present
- If the user provides additional context (e.g., "I know someone on this team", "emphasize my ML work") → incorporate and re-present

### Step 4: Learn Template & Generate .tex Files

#### 4a: Determine Output Path

1. Generate folder name: lowercase kebab-case from company + position
   - Remove special characters, replace spaces with hyphens
   - Examples: `google-swe-intern`, `stripe-payments-engineer`, `appier-ml-engineer`
2. Create output directory: `{output_dir}/{company-position}/`
3. If the directory already exists, ask the user: overwrite, create a versioned copy (append `-v2`), or cancel

#### 4b: Learn the Template

Read the LaTeX template from the configured `template.path`:

1. Read ALL `.tex`, `.cls`, `.sty` files
2. Identify the template's conventions:
   - Document class and options
   - Custom commands (e.g., `\cventry`, `\cvsection`, `\position`, `\letterentry`)
   - Section structure and ordering
   - How personal info is set (commands vs. direct text)
   - How dates and locations are formatted
   - Font and encoding settings
3. Read section files if `template.section_dir` is configured

**Critical rule**: ONLY use commands and structures that exist in the template. Never invent LaTeX commands. If the template uses `\cventry{date}{title}{company}{location}{description}`, use exactly that signature.

#### 4c: Detect Language Versions

Determine whether to generate bilingual output:

1. Check if the template has separate language files (e.g., `cv-en.tex` and `cv-zh.tex`)
2. Check if the user's material files contain bilingual sections (headers like `### Contributions (zh)` and `### Contributions (en)`)
3. Check the `personal-info.md` for bilingual name fields (`Name (zh)` and `Name (en)`)

**Decision logic:**
- If the template has separate language files → generate both versions
- If materials have bilingual sections → generate both versions
- If neither → generate a single version in the language that matches the JD
- If the user explicitly requests a specific language → follow the user's request

#### 4d: Copy Support Files

Copy into the output directory:
1. All files listed in `template.class_files` and `template.preamble_files`
2. Any font files (`.ttf`, `.otf`) referenced by the template
3. Any image files referenced (logos, photos)
4. Any `.bst` bibliography style files if used
5. Any other files the template imports (detected via `\input{}` or `\include{}` commands)

#### 4e: Generate .tex Files

Generate the CV and Cover Letter following the approved strategy:

**CV generation principles:**
- Follow the template's exact structure and command signatures
- Tailor content to the JD — select and prioritize based on strategy
- Preserve the user's voice — use phrasing from materials, adjust only for relevance and emphasis
- Weave ATS keywords naturally — don't force them into unrelated bullets
- Quantify where possible — pull metrics from materials, don't fabricate numbers

**Cover Letter generation principles:**
- Complement the CV, don't repeat it
- Tell a story: why this company, why this role, why this person
- Open with a specific hook (not "I am writing to apply for...")
- Reference 2-3 key achievements that map to JD requirements
- Close with genuine enthusiasm and a forward-looking statement
- Match the tone to the company culture (startup vs. enterprise, etc.)

### Step 5: Compile PDFs

Run the configured compiler for each `.tex` file:

```bash
cd {output_dir}/{company-position}/
{compiler} {file}.tex
```

Some templates require multiple compilation passes (for cross-references, bibliographies). Run the compiler twice by default.

**If compilation fails:**

1. **Read the error log** (`.log` file) — identify the specific error
2. **Common fixes:**
   - Missing package → check if the template requires specific LaTeX packages, suggest installation
   - Undefined command → you used a command not in the template; fix the `.tex` source
   - Font not found → inform the user which fonts need to be installed
   - Encoding error → check for character encoding issues in the content
3. **Fix the `.tex` source** and retry
4. **Maximum 3 attempts** — if still failing after 3 fixes, present the error to the user with the `.tex` file path so they can debug manually
5. **Never silently skip compilation** — always inform the user of the outcome

### Step 6: Summary

Present the user with:

```
Resume generation complete!

  Output:   {output_dir}/{company-position}/
  Files:
    - cv-en.tex / cv-en.pdf (XX KB)
    - cv-zh.tex / cv-zh.pdf (XX KB)
    - cover-letter-en.tex / cover-letter-en.pdf (XX KB)
    - cover-letter-zh.tex / cover-letter-zh.pdf (XX KB)
    - jd.md (saved JD for reference)

  Key tailoring choices:
    - Emphasized: {what was highlighted}
    - Omitted: {what was left out and why}
    - ATS keywords included: keyword1, keyword2, ...

  Next steps:
    - Review with @resume-reviewer for detailed feedback
    - Open the PDFs to check formatting
    - Run /job-recon {company} to prepare for the interview
```

## Output Structure

```
{output_dir}/{company-position}/
├── jd.md                         (saved JD text)
├── cv.tex / cv.pdf               (if single language)
├── cv-en.tex / cv-en.pdf         (if bilingual)
├── cv-zh.tex / cv-zh.pdf         (if bilingual)
├── cover-letter.tex / .pdf       (if single language)
├── cover-letter-en.tex / .pdf    (if bilingual)
├── cover-letter-zh.tex / .pdf    (if bilingual)
└── {support files}               (.cls, preamble, fonts, etc.)
```

## Prerequisites

- Run `/setup-resume` first to configure template and materials
- LaTeX compiler installed (xelatex, pdflatex, or lualatex as configured)
- Appropriate fonts installed for the chosen template
