---
name: setup-resume
description: >
  Initialize resume workspace: set up material directory and configure LaTeX template.
  Trigger when user says "setup resume", "initialize resume", "configure template",
  or when /generate-resume detects missing configuration.
---

# Setup Resume Workspace

Initialize the user's project for resume generation. This skill sets up the material directory, guides the user through filling in their content, and configures the LaTeX template source.

## Process

### Step 1: Check Existing Setup

Check if `.claude/resume-config.json` already exists in the user's project root.

- **If it exists**: Read it and present the current configuration. Ask the user:
  - **Reconfigure** — start over with fresh settings
  - **Update template** — re-pull the template from its source (Overleaf/GitHub) without touching materials
  - **Keep current** — exit without changes
- **If it doesn't exist**: Proceed with fresh setup.

### Step 2: Set Up Material Directory

Check if `material/` directory exists in the project root.

**If missing:** Create it and copy all scaffolding files from this plugin's `scaffolding/material/` directory:

```
material/
├── personal-info.md          (copied from .example, ready to edit)
├── personal-info.md.example  (reference — keep for format guidance)
├── experience.md
├── experience.md.example
├── education.md
├── education.md.example
├── skills.md
├── skills.md.example
├── projects/
│   ├── project-template.md.example
│   └── (user adds project files here)
```

For each `.example` file, create a corresponding `.md` file by copying its contents. The `.md` file is what the user edits; the `.example` stays as a format reference.

**If exists:** Check which files are present. If any required files are missing, offer to create them from the `.example` templates.

### Step 3: Guided Material Collection

After setting up the directory, guide the user through filling in their materials. This is the most important part — good materials lead to good resumes.

Present this to the user:

```
Your material directory is ready! Let's fill it in.

I'll guide you through each file. You can:
  a) Fill them in now interactively (I'll ask questions)
  b) Fill them in yourself later (just edit the .md files)
  c) Import from an existing resume or LinkedIn export

Which approach would you prefer?
```

#### Option A: Interactive Collection

Walk through each file in order, asking questions:

1. **personal-info.md** — "What's your full name? Email? GitHub username? LinkedIn URL?"
2. **experience.md** — "Tell me about your most recent job. What was your title? Company? When did you start?"
   - For each role, ask for 3-5 key contributions with metrics
   - Prompt: "Can you quantify that? (e.g., reduced latency by 40%, served 1M users)"
3. **education.md** — "Where did you study? What degree? Any notable coursework, thesis, or honors?"
4. **skills.md** — "What are your main technical skills? Let's organize them by category."
5. **projects/** — "Do you have any notable side projects, open source work, or research? Let's create a file for each."

Write answers directly into the `.md` files as the user provides them.

#### Option B: Self-Guided

Tell the user which files to edit and what to focus on:

```
Edit these files in material/:

  1. personal-info.md — your contact info and online profiles
  2. experience.md   — work history (most recent first, 3-5 bullets per role)
  3. education.md    — degrees, coursework, thesis, honors
  4. skills.md       — technical skills organized by category
  5. projects/       — one .md file per notable project

Tip: Use action verbs and include metrics where possible.
     Each .example file shows the expected format.

Run /generate-resume when you're ready!
```

#### Option C: Import from Existing Source

If the user has an existing resume (PDF, DOCX, or LaTeX) or LinkedIn data:

1. Ask for the file path
2. Read the file content (use the Read tool for PDF/text, or ask user to paste for other formats)
3. Parse the content and populate the material `.md` files
4. Present the extracted content for the user to review and correct

### Step 4: Configure LaTeX Template

Ask the user where their LaTeX template is:

```
Where is your LaTeX resume template?

  a) Local path — I already have a template directory
  b) Overleaf — clone from an Overleaf project
  c) GitHub — clone from a git repository
```

#### Option A: Local Path
- Ask for the absolute or relative path
- **Validate**: Check the path exists AND contains at least one `.tex` file
- If validation fails, show what was found and ask the user to correct the path

#### Option B: Overleaf
- Ask for the Overleaf project URL (e.g., `https://www.overleaf.com/project/abc123`)
- Convert to git URL: `https://git.overleaf.com/abc123`
- Clone into `templates/` directory:
  ```bash
  git clone https://git.overleaf.com/{project-id} templates/{template-name}
  ```
- **If authentication fails**: Guide the user:
  ```
  Overleaf git access requires authentication.

  Option 1: Use a git token
    - Go to Overleaf → Account Settings → Git Integration
    - Generate a token
    - Clone with: git clone https://{email}:{token}@git.overleaf.com/{id} templates/{name}

  Option 2: Download as zip
    - In Overleaf, click Menu → Download → Source
    - Unzip to templates/{name}/
    - Then re-run /setup-resume and choose "Local path"
  ```

#### Option C: GitHub / Git Repository
- Ask for the repository URL
- Clone into `templates/` directory:
  ```bash
  git clone {repo-url} templates/{template-name}
  ```
- If the clone fails (auth required, repo not found), show the error and ask the user to fix it

### Step 5: Analyze Template Structure

Once the template path is confirmed, scan the template directory:

1. **Find all `.tex` files** — identify main document files

2. **Identify CV vs. Cover Letter files** — heuristics:
   - Filename contains `cv`, `resume`, `curriculum` → CV file
   - Filename contains `cover`, `letter`, `motivation` → Cover Letter file
   - If ambiguous, ask the user which file is which

3. **Find `.cls` and `.sty` files** — custom class and style files

4. **Find section/component files** — look in subdirectories like `sections/`, `parts/`, `components/`

5. **Detect compilation method** — check in this order:
   - `Makefile` or `latexmkrc` → use the command specified there
   - `.tex` file contains `\usepackage{fontspec}` or `\usepackage{xeCJK}` → `xelatex`
   - `.tex` file contains `\usepackage[utf8]{inputenc}` → `pdflatex`
   - `.cls` file specifies a compiler → use that
   - Default → `xelatex` (best Unicode/font support)

6. **Detect font requirements** — scan for `\setmainfont`, `\setsansfont`, `\newfontfamily` and list required fonts

Present the detected structure for confirmation:

```
Detected template structure:
  CV file:          cv.tex
  Cover letter:     cover-letter.tex
  Class file:       awesome-cv.cls
  Preamble:         preamble-common.tex
  Sections:         sections/*.tex
  Compiler:         xelatex
  Fonts required:   Roboto, Source Sans Pro
  Support files:    awesome-cv.cls, preamble-common.tex

Does this look correct? (yes / correct something)
```

### Step 6: Save Configuration

Write the configuration to `.claude/resume-config.json`:

```json
{
  "template": {
    "path": "templates/my-template",
    "source": "overleaf",
    "source_url": "https://www.overleaf.com/project/abc123",
    "cv_file": "cv.tex",
    "cover_letter_file": "cover-letter.tex",
    "class_files": ["awesome-cv.cls"],
    "preamble_files": ["preamble-common.tex"],
    "section_dir": "sections",
    "compiler": "xelatex",
    "required_fonts": ["Roboto", "Source Sans Pro"]
  },
  "output_dir": "docs/resumes",
  "material_dir": "material"
}
```

The `source` and `source_url` fields enable template updates later.

### Step 7: Set Up Output Directory

Create `docs/resumes/` if it doesn't exist.

### Step 8: Update .gitignore

Check the user's `.gitignore` and suggest adding entries for:
- Personal content files in `material/` (not `.example` files)
- Generated output in `docs/resumes/*/`
- Research files in `docs/research/`
- Config file `.claude/resume-config.json`
- LaTeX build artifacts

Reference `scaffolding/.gitignore.example` for the recommended entries. Show the user what will be added and ask for confirmation before modifying `.gitignore`.

### Step 9: Summary

```
Resume workspace setup complete!

  Material:   material/ ({N} files ready to edit)
  Template:   {template_path}/ (compiler: {compiler})
  Output:     docs/resumes/
  Config:     .claude/resume-config.json

Next steps:
  1. Edit files in material/ with your resume content
     (or continue the guided collection if you chose that option)
  2. Run /generate-resume with a job description to create your first resume
  3. Run @resume-reviewer to get feedback on the result
```

## Prerequisites

- Git (for cloning templates from Overleaf/GitHub)
