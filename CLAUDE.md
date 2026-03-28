# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Offer Hunter is a Claude Code plugin that helps job seekers generate tailored CVs and Cover Letters (LaTeX -> PDF), review resumes with AI, and research job targets. It is template-agnostic — users bring their own LaTeX template.

## Language

Respond in the same language the user is communicating in.

## Plugin Structure

```
.
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── skills/
│   ├── setup-resume/        # Initialize workspace + configure template
│   ├── generate-resume/     # Generate tailored CV & Cover Letter
│   └── job-recon/           # Deep research on job targets
├── agents/
│   └── resume-reviewer.md   # AI-powered resume review
├── scaffolding/
│   └── material/            # .example files for user setup
├── LICENSE
├── README.md
└── README_zhtw.md
```

## Key Conventions

- This is a **Claude Code plugin**, not a standalone repo for end users
- Users install this plugin, then run `/setup-resume` in their own project to initialize
- `material/` lives in the user's project (not in this plugin repo)
- Template configuration is stored in the user's `.claude/resume-config.json`
- No specific LaTeX template is bundled — users provide their own

## Workflow

1. User installs the plugin
2. `/setup-resume` — configures template source and material directory
3. `/generate-resume` — generates tailored resume for a job listing
4. `@resume-reviewer` — gets AI feedback on the generated resume
5. `/job-recon` — researches a company/team/person before an interview
