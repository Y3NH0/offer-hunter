# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A resume generation toolkit that creates tailored CVs and Cover Letters (LaTeX → PDF) based on job descriptions. Supports bilingual output (Chinese/English) with Notion integration for job tracking.

## Language

Respond in Traditional Chinese (繁體中文) unless the context requires English.

## Repository Structure

```
.
├── material/              # Resume source materials (your content)
│   ├── projects/          # Project experiences (zh + en)
│   ├── photos/            # Headshot photos
│   ├── experience.md      # Work experience
│   ├── education.md       # Education
│   ├── skills.md          # Technical skills
│   └── ...                # See .example files for format
├── templates/             # LaTeX templates
│   └── awesome-cv/        # Custom Awesome-CV template
│       ├── awesome-cv.cls # Modified class file
│       ├── preamble-common.tex
│       ├── cv.tex         # CV template
│       ├── cover-letter.tex
│       └── sections/      # Section templates
├── docs/
│   └── resumes/           # Generated output (per job application)
├── .claude/
│   ├── skills/
│   │   └── generate-resume/  # Resume generation skill
│   └── agents/
│       └── resume-reviewer.md  # Gemini-powered review agent
└── README.md
```

## Key Conventions

- `material/` is the single source of truth for resume content
- Material files contain both Chinese (zh) and English (en) versions
- Resume outputs go in `docs/resumes/<company-position>/`
- Each output folder contains its own `awesome-cv.cls` copy for self-contained compilation

## LaTeX CV

- Template: `templates/awesome-cv/` (custom fork of Awesome-CV by posquit0)
- Compile with: `xelatex` (requires XeLaTeX + CJK fonts, e.g. PingFang TC or Noto Sans TC)
- Generated per-job outputs go to `docs/resumes/<company-position>/`

## Notion Integration (Optional)

- Set up a Notion database to track job listings
- The `generate-resume` skill can fetch JD content from Notion via MCP
- Configure your Notion page ID in the skill or provide JD text directly

## Workflow

1. Fill in `material/` with your resume content (see `.example` files)
2. Use `/generate-resume` with a job listing to create tailored CV + Cover Letter
3. Use `@resume-reviewer` to get AI-powered feedback on the output
4. Compile and iterate
