# Offer Hunter

> Stop sending the same resume everywhere. Start sending the *right* one.

A [Claude Code](https://claude.ai/code) plugin that turns job descriptions into tailored resumes. Bring your own LaTeX template, point it at a job listing, and get a CV + Cover Letter that actually matches.

## Install

```bash
/plugin marketplace add Y3NH0/offer-hunter
/plugin install offer-hunter@Y3NH0-offer-hunter
```

**Prerequisites:** [Claude Code](https://claude.ai/code) CLI, a LaTeX compiler (XeLaTeX/pdfLaTeX/LuaLaTeX), and a LaTeX resume template ([Overleaf](https://www.overleaf.com/latex/templates/tagged/cv), GitHub, or your own).

## The Pipeline

```
/setup-resume          Configure template + fill in your materials (one-time)
        ↓
/generate-resume       Paste a JD → get a tailored CV + Cover Letter
        ↓
@resume-reviewer       Score it on 7 dimensions, catch gaps before you send
        ↓
/job-recon             Research the company, team, and interviewer
        ↓
/interview-slides      Generate self-intro slides for the interview
```

## Features

### `/setup-resume` — One-Time Setup

Walks you through connecting your LaTeX template (local, Overleaf, or GitHub) and filling in your materials with guided collection — interactive Q&A, self-guided editing, or import from an existing resume.

### `/generate-resume` — Tailored Resume Generation

```
/generate-resume "Senior ML Engineer at Stripe"
```

1. Extracts requirements, keywords, and tech stack from the JD
2. Maps your experience to the job — picks what to emphasize, what to cut
3. Presents the strategy for your approval
4. Generates `.tex` files following your template's exact commands
5. Compiles to PDF

Flags: `--skip-strategy` `--cv-only` `--cover-letter-only`

### `@resume-reviewer` — AI-Powered Scoring

Scores your resume on 7 weighted dimensions with a concrete rubric:

| Dimension | Weight |
|-----------|--------|
| JD Alignment | 20% |
| ATS Optimization | 15% |
| Impact & Metrics | 15% |
| Relevance | 15% |
| Conciseness | 10% |
| Cover Letter Fit | 15% |
| Language & Grammar | 10% |

Every improvement comes with a concrete rewrite suggestion. Choose your reviewer: Claude Code (default), Gemini CLI, or any Claude model.

### `/job-recon` — Interview Intelligence

```
/job-recon "Look into the Platform team at Datadog"
```

Produces a structured report: key people's backgrounds, team tech stack, open source output, recent news, real interview experiences from Glassdoor/Blind/Reddit/PTT/Dcard, and specific preparation advice.

### `/interview-slides` — Presentation Slides

```
/interview-slides "Google SWE interview, 3 minutes"
```

Generates a Marp slide deck for self-introductions — narrative arc, speaker notes, timing guidance. Supports 1/3/5 minute presets. Export to PDF/PPTX via Marp CLI or VS Code.

## How It Works

```
material/                 Your content (write once)
├── personal-info.md
├── experience.md
├── education.md
├── skills.md
└── projects/*.md

your-template/            Your LaTeX template (configured once)
├── cv.tex
└── cover-letter.tex

docs/resumes/             Output (one folder per application)
└── stripe-sr-ml-eng/
    ├── cv.tex → cv.pdf
    └── cover-letter.tex → cover-letter.pdf

docs/research/            Recon reports
docs/slides/              Interview slides
```

## License

MIT
