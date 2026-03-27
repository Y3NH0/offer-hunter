---
name: setup-resume
description: >
  Initialize resume workspace: set up material directory and configure LaTeX template.
  Trigger when user says "setup resume", "初始化履歷", "設定模板",
  or when /generate-resume detects missing configuration.
---

# Setup Resume Workspace

Initialize the user's project for resume generation. This skill sets up the material directory structure and configures the LaTeX template source.

## Process

### Step 1: Check Existing Setup

Check if `.claude/resume-config.json` already exists in the user's project root.

- If it exists, read it and ask the user if they want to reconfigure or keep the current setup.
- If it doesn't exist, proceed with fresh setup.

### Step 2: Set Up Material Directory

Check if `material/` directory exists in the project root.

- **If missing**: Create `material/` and copy all `.example` files from this plugin's `scaffolding/material/` directory:
  ```
  material/personal-info.md.example → material/personal-info.md.example
  material/experience.md.example
  material/education.md.example
  material/skills.md.example
  material/projects/project-template.md.example
  ```
  Also create the actual `.md` files by copying from `.example` so the user can start editing.

- **If exists**: Skip, inform the user that material directory already exists.

### Step 3: Configure LaTeX Template

Ask the user where their LaTeX template is. Present three options:

#### Option A: Local Path
The user already has a LaTeX template directory on their machine.
- Ask for the absolute or relative path
- Verify the path exists and contains `.tex` files

#### Option B: Overleaf
The user wants to pull a template from Overleaf.
- Ask for the Overleaf project URL (e.g., `https://www.overleaf.com/project/abc123`)
- Convert to git URL: `https://git.overleaf.com/abc123`
- Clone into `templates/` directory:
  ```bash
  git clone https://git.overleaf.com/<project-id> templates/<template-name>
  ```
- Note: Overleaf git access may require authentication. Guide the user if needed.

#### Option C: GitHub / Git Repository
The user wants to clone a template from GitHub or another git host.
- Ask for the repository URL
- Clone into `templates/` directory:
  ```bash
  git clone <repo-url> templates/<template-name>
  ```

### Step 4: Analyze Template Structure

Once the template path is confirmed, scan the template directory to understand its structure:

1. **Find all `.tex` files** — identify the main document file(s) (CV, cover letter)
2. **Find `.cls` files** — identify custom class files
3. **Find `.sty` files** — identify style packages
4. **Find section files** — look for `sections/`, `parts/`, or similar subdirectories
5. **Check for preamble** — look for shared preamble files
6. **Detect compilation method** — check for `Makefile`, `latexmkrc`, or infer from file content (pdflatex vs xelatex vs lualatex)

Present the detected structure to the user for confirmation:

```
Detected template structure:
  Main CV file:      cv.tex
  Cover letter:      cover-letter.tex
  Class file:        awesome-cv.cls
  Preamble:          preamble-common.tex
  Sections:          sections/*.tex
  Compiler:          xelatex
  Support files:     awesome-cv.cls, preamble-common.tex
```

### Step 5: Save Configuration

Write the configuration to `.claude/resume-config.json` in the user's project root:

```json
{
  "template": {
    "path": "templates/my-template",
    "cv_file": "cv.tex",
    "cover_letter_file": "cover-letter.tex",
    "class_files": ["awesome-cv.cls"],
    "preamble_files": ["preamble-common.tex"],
    "section_dir": "sections",
    "compiler": "xelatex"
  },
  "output_dir": "docs/resumes",
  "material_dir": "material"
}
```

### Step 6: Set Up Output Directory

Create `docs/resumes/` if it doesn't exist.

### Step 7: Update .gitignore

Check the user's `.gitignore` and suggest adding entries for:
- `material/` personal content files (not `.example` files)
- `docs/resumes/*/` generated output
- `.claude/resume-config.json` (contains local paths)
- LaTeX build artifacts

Reference `scaffolding/.gitignore.example` for the recommended entries.

### Step 8: Summary

Present the completed setup:

```
Resume workspace setup complete!

  Material:   material/ (edit the .md files with your content)
  Template:   templates/my-template/
  Output:     docs/resumes/
  Config:     .claude/resume-config.json

Next steps:
  1. Edit files in material/ with your resume content
  2. Run /generate-resume with a job description to create your first resume
```

## Prerequisites

- Git (for cloning templates from Overleaf/GitHub)
