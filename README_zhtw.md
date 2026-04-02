# Offer Hunter

> 別再到處投一樣的履歷了。投*對的*那份。

一個 [Claude Code](https://claude.ai/code) 外掛，把職缺描述變成量身打造的履歷。帶上你自己的 LaTeX 模板，指向一個職缺，就能得到真正對口的 CV + Cover Letter。

## 安裝

```bash
/plugin marketplace add Y3NH0/offer-hunter
/plugin install offer-hunter@Y3NH0-offer-hunter
```

**前置需求：**[Claude Code](https://claude.ai/code) CLI、LaTeX 編譯器（XeLaTeX/pdfLaTeX/LuaLaTeX）、一份 LaTeX 履歷模板（[Overleaf](https://www.overleaf.com/latex/templates/tagged/cv)、GitHub 或你自己的）。

## 完整流程

```
/setup-resume          設定模板 + 填寫素材（一次性）
        ↓
/generate-resume       貼上 JD → 產出客製化 CV + Cover Letter
        ↓
@resume-reviewer       7 個維度評分，送出前抓漏
        ↓
/job-recon             調查公司、團隊、面試官
        ↓
/interview-slides      生成面試自介簡報
```

## 功能

### `/setup-resume` — 一次性設定

引導你連接 LaTeX 模板（本地、Overleaf 或 GitHub）並填寫素材 — 互動式問答、自助編輯、或從既有履歷匯入。

### `/generate-resume` — 客製化履歷生成

```
/generate-resume "Stripe Senior ML Engineer"
```

1. 從 JD 提取需求、關鍵字、技術棧
2. 對應你的經歷 — 決定強調什麼、刪減什麼
3. 呈現策略等你核准
4. 依照你模板的指令產出 `.tex` 檔案
5. 編譯成 PDF

旗標：`--skip-strategy` `--cv-only` `--cover-letter-only`

### `@resume-reviewer` — AI 評分

用加權評分表從 7 個維度打分：

| 維度 | 權重 |
|------|------|
| JD 對齊度 | 20% |
| ATS 最佳化 | 15% |
| 影響力與指標 | 15% |
| 相關性 | 15% |
| 精簡度 | 10% |
| Cover Letter 契合度 | 15% |
| 語言與文法 | 10% |

每項改善建議都附具體重寫範例。可選 Claude Code（預設）、Gemini CLI 或任何 Claude 模型。

### `/job-recon` — 面試情報

```
/job-recon "調查 Datadog 的 Platform 團隊"
```

產出結構化報告：關鍵人物背景、團隊技術棧、開源產出、近期動態、來自 Glassdoor/Blind/Reddit/PTT/Dcard 的真實面試經驗、以及具體準備建議。

### `/interview-slides` — 面試簡報

```
/interview-slides "Google SWE 面試，3 分鐘"
```

生成 Marp 自介簡報 — 敘事弧線、講者備註、時間指引。支援 1/3/5 分鐘預設。可用 Marp CLI 或 VS Code 匯出 PDF/PPTX。

## 運作方式

```
material/                 你的內容（寫一次）
├── personal-info.md
├── experience.md
├── education.md
├── skills.md
└── projects/*.md

your-template/            你的 LaTeX 模板（設定一次）
├── cv.tex
└── cover-letter.tex

docs/resumes/             輸出（每個申請一個資料夾）
└── stripe-sr-ml-eng/
    ├── cv.tex → cv.pdf
    └── cover-letter.tex → cover-letter.pdf

docs/research/            偵察報告
docs/slides/              面試簡報
```

## 授權

MIT
