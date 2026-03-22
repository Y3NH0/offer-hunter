# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal resume & website repository (履歷＆個人網站) for 陳彥合 (Yen-Ho Chen). Currently focused on organizing resume materials and generating polished CVs. A personal website (Next.js) is planned for the future.

## Language

The user communicates in Traditional Chinese (繁體中文). Respond in Traditional Chinese unless the context requires English.

## Repository Structure

```
thisisyenho/
├── material/              # 履歷原始素材（純內容）
│   ├── projects/          # 各專案經歷（中英文）
│   ├── photos/            # 照片、頭像素材
│   ├── experience.md      # 工作經歷
│   ├── education.md       # 學歷
│   ├── skills.md          # 技術技能
│   ├── soft-skills.md     # 軟實力（含 raw notes）
│   ├── competitions.md    # 競賽經歷
│   └── extracurricular.md # 課外活動
├── docs/                  # 文件資料
│   ├── resumes/           # 履歷成品（md, tex, pdf）
│   ├── reviews/           # 績效評核
│   ├── papers/            # 論文
│   └── research/          # 求職研究、規劃筆記
└── README.md
```

## Key Conventions

- `material/` is the single source of truth for resume content. When generating or updating resumes, always read from here.
- Material files contain both Chinese (zh) and English (en) versions. Keep both in sync when editing.
- `material/projects/` files may contain personal notes (e.g. NOTE blocks) — preserve these as-is, they are intentional.
- Resume outputs (LaTeX, PDF, markdown) go in `docs/resumes/`.

## LaTeX CV

- Source: `docs/resumes/cv.tex`
- Compile with: `xelatex cv.tex` (requires XeLaTeX for CJK support)
