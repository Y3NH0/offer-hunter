# Offer Hunter

A Claude Code plugin for job seekers: generate tailored resumes, get AI-powered reviews, and research your interview targets.

## Features

- **JD-tailored generation**: Automatically customize your CV and Cover Letter for each job application
- **Template-agnostic**: Bring your own LaTeX template — from Overleaf, GitHub, or local files
- **Bilingual support**: Chinese and English output (if your materials include both)
- **AI-powered review**: Get resume feedback via Claude Code, Gemini CLI, or a specified model
- **Job recon**: Deep research on companies, teams, and interviewers before your interview
- **Strategy phase**: Analyze JD vs your experience before generating — or skip and go direct

## Quick Start

### Prerequisites

- [Claude Code](https://claude.ai/code) CLI
- A LaTeX compiler (XeLaTeX, pdfLaTeX, or LuaLaTeX)
- A LaTeX resume template (your own, from Overleaf, or from GitHub)

### Install

```bash
# In Claude Code, install the plugin
/plugin install offer-hunter
```

### Setup

Run the setup skill in your project:

```
/setup-resume
```

This will:
1. Create a `material/` directory with example files for your resume content
2. Configure your LaTeX template (local path, Overleaf, or GitHub)
3. Set up output directories

### Generate a Resume

```
/generate-resume "AI Engineer at Company X"
```

Or provide a JD URL:
```
/generate-resume https://example.com/job-posting
```

Or paste JD text directly:
```
/generate-resume --skip-strategy
> Paste your JD here...
```

### Review a Resume

After generating, get AI feedback:
```
@resume-reviewer
```

### Research a Job Target

Before an interview, research the company/team/interviewer:
```
/job-recon "Research the AI team at Company X"
```

## How It Works

```
material/           # Your resume content (single source of truth)
├── personal-info.md
├── experience.md
├── education.md
├── skills.md
└── projects/*.md

your-template/      # Your LaTeX template (configured during setup)
├── cv.tex
├── cover-letter.tex
└── ...

docs/resumes/       # Generated output (per job application)
└── company-position/
    ├── cv.tex + cv.pdf
    └── cover-letter.tex + cover-letter.pdf
```

## Plugin Skills

| Skill | Command | Description |
|-------|---------|-------------|
| Setup | `/setup-resume` | Initialize workspace and configure template |
| Generate | `/generate-resume` | Create tailored CV & Cover Letter |
| Recon | `/job-recon` | Research companies, teams, and people |
| Review | `@resume-reviewer` | AI-powered resume feedback |

## License

MIT
