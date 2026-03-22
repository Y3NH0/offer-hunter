# Personal Resume & Website

個人履歷與網站專案

## Project Structure

```
.
├── material/                # 履歷原始素材
│   ├── projects/            # 各專案經歷（中英文）
│   ├── photos/              # 照片素材
│   ├── experience.md        # 工作經歷
│   ├── education.md         # 學歷
│   ├── skills.md            # 技術技能
│   ├── soft-skills.md       # 軟實力
│   ├── competitions.md      # 競賽經歷
│   └── extracurricular.md   # 課外活動
├── docs/
│   ├── resumes/             # 履歷成品（tex, pdf）
│   ├── reviews/             # 績效評核
│   ├── papers/              # 論文
│   └── research/            # 求職研究、規劃筆記
├── .claude/
│   └── skills/
│       └── generate-resume/ # 客製化履歷生成 skill
├── CLAUDE.md
└── README.md
```

## Workflow

1. **素材管理** — `material/` 是履歷內容的 single source of truth，中英文並存
2. **職缺追蹤** — Notion「職缺整理」資料庫管理目標職缺
3. **履歷生成** — 使用 `/generate-resume` skill，根據 JD 客製化 CV + Cover Letter
4. **產出** — 每個職缺產出獨立資料夾於 `docs/resumes/<company-position>/`

## Tech Stack

- **LaTeX (XeLaTeX):** CV / Cover Letter 生成，基於 Awesome-CV 客製化 template
- **Notion:** 職缺管理與 JD 來源
- **Claude Code:** 自動化履歷客製化流程

## Roadmap

- [x] 整理履歷素材（material/）
- [x] 建立 generate-resume skill
- [ ] 客製化 LaTeX template（基於 Awesome-CV）
- [ ] 個人網站（Next.js）
