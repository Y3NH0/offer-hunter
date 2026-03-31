# Offer Hunter

> Stop sending the same resume everywhere. Start sending the *right* one.

Offer Hunter is a [Claude Code](https://claude.ai/code) plugin that turns job descriptions into tailored, professional resumes — automatically. Bring your own LaTeX template, point it at a job listing, and get a CV + Cover Letter that actually matches what the company is looking for.

## What It Does

```
You + Job Description + Your Template = Tailored Resume in seconds
```

**Generate** — Paste a JD or URL. Offer Hunter reads your experience, maps it to the job requirements, and generates a tailored CV + Cover Letter in LaTeX.

**Review** — Get your resume scored against the JD by AI. Catch missing keywords, weak bullet points, and alignment gaps before you hit send.

**Recon** — Research your interview target: the hiring manager's background, the team's tech stack, real interview experiences from Glassdoor/Blind/Reddit, and what to prepare for.

**Slides** — Generate Marp-format interview presentation slides for self-introductions. Tailored to the role, timed to your slot, with speaker notes.

## Quick Start

```bash
# Option 1: Install from GitHub
claude plugin install https://github.com/yenhochen/offer-hunter

# Option 2: Clone and install locally
git clone https://github.com/yenhochen/offer-hunter.git
claude plugin install ./offer-hunter

# Then in your project:
/setup-resume                    # configure your template + materials
/generate-resume                 # generate your first resume
```

### Prerequisites

- [Claude Code](https://claude.ai/code) CLI
- A LaTeX compiler (XeLaTeX, pdfLaTeX, or LuaLaTeX)
- A LaTeX resume template (yours, from [Overleaf](https://www.overleaf.com/latex/templates/tagged/cv), or GitHub)

### Setup

`/setup-resume` walks you through:

1. **Materials** — creates a `material/` directory with templates for your experience, education, skills, and projects
2. **Template** — connects your LaTeX template (local path, Overleaf clone, or GitHub repo)
3. **Config** — saves everything to `.claude/resume-config.json` so you only set up once

### Generate

```
/generate-resume "Senior ML Engineer at Stripe"
```

What happens under the hood:

1. **Reads the JD** — extracts requirements, keywords, and themes
2. **Reads your materials** — all your experiences, projects, skills
3. **Strategizes** — maps your experience to the JD, picks what to emphasize, what to cut (you approve before it generates)
4. **Generates** — creates `.tex` files following your template's exact structure and commands
5. **Compiles** — runs LaTeX to produce PDFs

You can also feed a URL or paste raw JD text. Add `--skip-strategy` to go straight to generation.

### Review

```
@resume-reviewer
```

Scores your resume on 7 dimensions: JD alignment, ATS optimization, impact metrics, relevance, conciseness, cover letter fit, and language quality. Choose your reviewer: Claude Code (default), Gemini CLI, or any Claude model.

### Recon

```
/job-recon "Look into the Platform team at Datadog"
```

Produces a structured intelligence report with: key people's backgrounds, team tech stack, open source output, recent company news, real interview experiences from community platforms, and specific preparation advice.

### Slides

```
/interview-slides "Google SWE interview, 3 minutes"
```

Generates a self-introduction slide deck in Marp format: narrative arc from your background to why this role, with speaker notes and timing guidance. Export to PDF/PPTX with Marp CLI or VS Code.

## How It Works

```
material/               # Your content (single source of truth)
├── personal-info.md    #   name, contact, links
├── experience.md       #   work history
├── education.md        #   degrees & research
├── skills.md           #   technical skills
└── projects/*.md       #   project writeups

your-template/          # Your LaTeX template
├── cv.tex              #   (configured during /setup-resume)
└── cover-letter.tex

docs/resumes/           # Output (one folder per application)
└── stripe-sr-ml-eng/
    ├── cv.tex + cv.pdf
    └── cover-letter.tex + cover-letter.pdf
```

Write your materials once. Generate a different, tailored resume for every application.

## Commands

| Command | What it does |
|---------|-------------|
| `/setup-resume` | One-time setup: template + materials (with guided collection) |
| `/generate-resume` | Generate tailored CV & Cover Letter (`--skip-strategy`, `--cv-only`, `--cover-letter-only`) |
| `/job-recon` | Deep research on a company/team/person |
| `/interview-slides` | Generate Marp presentation slides for interviews |
| `@resume-reviewer` | AI-powered resume review & scoring (7-dimension rubric) |

## License

MIT
