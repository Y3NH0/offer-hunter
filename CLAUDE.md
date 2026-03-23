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
├── templates/             # LaTeX 模板
│   └── awesome-cv/        # 基於 Awesome-CV 的自訂模板
│       ├── awesome-cv.cls
│       ├── preamble-common.tex
│       ├── cv.tex
│       ├── cover-letter.tex
│       └── sections/
├── docs/                  # 文件資料
│   ├── resumes/           # 履歷成品（每個職缺一個資料夾）
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

- Template: `templates/awesome-cv/` (custom fork of Awesome-CV by posquit0)
- Compile with: `xelatex` (requires XeLaTeX + CJK fonts, e.g. PingFang TC)
- Generated per-job outputs go to `docs/resumes/<company-position>/`
- Each output folder contains its own `awesome-cv.cls` copy for self-contained compilation

## Notion Integration

- **2026 JOB 主頁面** (page ID: `26fa0e4d-dfa1-8052-b7b2-f859f0d17872`)
  - 內含資料庫「職缺整理」— 記錄預計投遞的職缺
- **求職 CV/Cover Letter 生成流程：**
  1. 從 Notion「職缺整理」fetch 目標職缺內容（JD、公司資訊等）
  2. 讀取 `material/` 內的履歷素材
  3. 根據職缺需求挑選 & 調整相關經歷，生成客製化的 CV 和求職信
  4. 產出放到 `docs/resumes/`
