---
name: interview-slides
description: >
  Generate Marp-format interview presentation slides for self-introduction.
  Trigger when user says "interview slides", "self-introduction slides",
  "presentation for interview", or "prepare slides for my interview".
---

# Interview Slides Generator

Generate a concise self-introduction presentation in Marp (Markdown Presentation) format, tailored for a specific job interview. The slides help the user present their background, skills, and fit for the role in a structured, professional way.

## Input

The user provides:

- **Target company/position** (required): Which interview these slides are for
- **Time limit** (optional): How long the self-introduction should be (default: 3 minutes)
- **Language** (optional): Presentation language (default: same as user's communication language)
- **Style** (optional): Formal, casual, technical, or storytelling

If a JD or recon report is available (from previous `/generate-resume` or `/job-recon` runs), use them to tailor the content.

## Process

### Step 1: Gather Context

1. **Read materials**: Load the user's `material/` files (same as `/generate-resume`)
2. **Check for existing JD**: Look in `docs/resumes/` for a matching `jd.md`
3. **Check for existing recon**: Look in `docs/research/` for a matching company report
4. **Read resume config**: Load `.claude/resume-config.json` if available

If no JD or recon is available, ask the user for basic context about the role.

### Step 2: Plan Slide Structure

Based on the time limit, determine the number of slides:

| Time Limit | Slides | Guidance |
|------------|--------|----------|
| 1 minute | 3-4 | Name + hook, 1 key experience, why this role |
| 3 minutes | 5-7 | Name + hook, 2-3 experiences, skills, why this role, closing |
| 5 minutes | 7-10 | Name + hook, 3-4 experiences with depth, skills, projects, why this role, questions |

Present the proposed structure to the user:

```
Proposed slide structure (3 minutes):

  1. Title — Name, current role, one-line hook
  2. Background — Education + career arc (30 sec)
  3. Key Experience #1 — Most relevant role/project (45 sec)
  4. Key Experience #2 — Second most relevant (30 sec)
  5. Technical Skills — Highlights matching the JD (20 sec)
  6. Why {Company} — What excites you about this opportunity (30 sec)
  7. Thank You — Contact info + transition to Q&A

Shall I proceed, or would you like to adjust?
```

### Step 3: Generate Marp Slides

Generate a `.md` file in Marp format with the following structure:

```markdown
---
marp: true
theme: default
paginate: true
---

# {Name}

## {Current Title} | {One-line Hook}

{Contact: email | GitHub | LinkedIn}

---

## Background

- {Education highlights}
- {Career arc in 2-3 bullets}

<!-- Speaker notes: Spend ~30 seconds here. Establish credibility quickly. -->

---

## {Key Experience Title}

**{Role} @ {Company}** | {Period}

- {Achievement 1 with metric}
- {Achievement 2 with metric}
- {Relevance to target role}

<!-- Speaker notes: This is your strongest match to the JD. Emphasize {keyword}. -->

---

...continue for remaining slides...
```

**Slide generation principles:**

- **One idea per slide** — don't overload slides with text
- **Bullet points, not paragraphs** — 3-5 bullets max per slide
- **Metrics over descriptions** — "Reduced latency by 60%" not "Worked on performance improvements"
- **Speaker notes** — include timing guidance and key talking points in HTML comments
- **Tailored to the JD** — emphasize experiences that match the role's requirements
- **Tell a story** — the slides should have a narrative arc (where you've been → what you've done → why here)
- **End with intent** — the last content slide should answer "why this company/role?"

### Step 4: Add Styling (Optional)

If the user wants custom styling, add a Marp theme section. Offer simple options:

```
Would you like to customize the slide style?

  1. Default (clean, professional)
  2. Minimal (lots of white space, modern)
  3. Dark (dark background, light text)
  4. Custom (provide your own Marp theme or CSS)
```

For custom themes, add inline CSS in the frontmatter:

```markdown
---
marp: true
theme: default
style: |
  section {
    font-family: 'Helvetica Neue', sans-serif;
  }
  h1 {
    color: #2563eb;
  }
---
```

### Step 5: Save & Export Instructions

Save the generated file:

```
docs/slides/{company-position}-slides.md
```

Present the user with export options:

```
Slides saved to: docs/slides/{company-position}-slides.md

To preview and export:

  Option 1: Marp CLI (recommended)
    npx @marp-team/marp-cli docs/slides/{file}.md --pdf
    npx @marp-team/marp-cli docs/slides/{file}.md --pptx
    npx @marp-team/marp-cli docs/slides/{file}.md --html

  Option 2: VS Code
    Install the "Marp for VS Code" extension, then open the .md file

  Option 3: Edit directly
    The file is plain Markdown — edit it in any text editor

Tip: Practice your delivery! Aim for {time_limit} with natural pacing.
```

### Step 6: Summary

```
Interview slides generated!

  File:     docs/slides/{company-position}-slides.md
  Slides:   {N} slides (~{time_limit} delivery)
  Language: {language}
  Tailored: {what was emphasized based on JD/recon}

Next steps:
  - Review and edit the slides to match your personal style
  - Export to PDF/PPTX using Marp CLI or VS Code
  - Practice! Time yourself to stay within {time_limit}
```

## Output Structure

```
docs/slides/
└── {company-position}-slides.md    (Marp-format markdown)
```

## Prerequisites

- Material files filled in (`material/`)
- For PDF/PPTX export: Marp CLI (`npx @marp-team/marp-cli`) or VS Code with Marp extension
- No LaTeX required — Marp uses Markdown only
