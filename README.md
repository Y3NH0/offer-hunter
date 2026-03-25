# Resume Toolkit

Generate tailored, professional CVs and Cover Letters from job descriptions — powered by Claude Code + LaTeX.

## Features

- **JD-tailored generation**: Automatically customize your resume for each job application
- **Bilingual support**: Chinese (繁體中文) and English output
- **Professional LaTeX template**: Custom Awesome-CV with banner header, accent colors, and CJK support
- **Notion integration**: Track job listings and fetch JD content directly
- **AI-powered review**: Get feedback on your resume using Gemini CLI
- **Strategy phase**: Analyze JD vs your experience before generating — or skip and go direct

## Quick Start

### Prerequisites

- [Claude Code](https://claude.ai/code) CLI
- XeLaTeX (via MacTeX or TeX Live)
- CJK fonts (PingFang TC on macOS, or Noto Sans TC)
- (Optional) Gemini CLI for resume review
- (Optional) Notion MCP plugin for job tracking

### Setup

1. **Clone and set up materials**
   ```bash
   git clone <this-repo>
   cd resume-toolkit
   ```

2. **Copy example files and fill in your content**
   ```bash
   cp material/personal-info.md.example material/personal-info.md
   cp material/experience.md.example material/experience.md
   cp material/education.md.example material/education.md
   cp material/skills.md.example material/skills.md
   # Edit each file with your actual content
   ```

3. **Install XeLaTeX** (if not already)
   ```bash
   # macOS
   brew install --cask mactex-no-gui
   ```

4. **Test the template**
   ```bash
   cd templates/awesome-cv
   xelatex cv.tex
   ```

### Generate a Resume

Use the Claude Code skill:
```
/generate-resume "AI Engineer at Company X"
```

Or provide JD text directly:
```
/generate-resume --skip-strategy
> Paste your JD here...
```

### Review a Resume

After generating, get AI feedback:
```
@resume-reviewer
```

## Project Structure

```
material/           # Your resume content (single source of truth)
├── *.md.example    # Template files — copy and fill in
templates/          # LaTeX templates
├── awesome-cv/     # Custom Awesome-CV with banner header
.claude/
├── skills/         # generate-resume skill
├── agents/         # resume-reviewer agent (Gemini-powered)
docs/resumes/       # Generated output per job application
```

## Template Features

- Full-width accent color banner header with photo
- Tag-style position label
- Two-line entry format (Organization + Position)
- Bold subtitles on bullet points
- Section-first-char accent coloring
- `\cvproject` command for simplified project entries
- `\cvskill` for left-aligned skill categories
- PingFang TC / Noto Sans TC CJK support

## Customization

### Accent Color
Edit `templates/awesome-cv/preamble-common.tex`:
```latex
\definecolor{awesome}{HTML}{1A7A7A}  % Change to your preferred color
```

### Font
Edit `templates/awesome-cv/awesome-cv.cls` font section to use your preferred fonts.

### Section Order
Edit the `\input{sections/...}` order in `cv.tex`.

## License

- LaTeX template based on [Awesome-CV](https://github.com/posquit0/Awesome-CV) by posquit0 (LPPL v1.3c)
- Toolkit code: MIT
