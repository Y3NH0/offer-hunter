# Offer Hunter

Claude Code 求職外掛：自動生成客製化履歷、AI 審稿、面試目標深度調查。

## 功能特色

- **JD 客製化生成**：自動根據每個職缺調整你的 CV 和求職信
- **模板無關**：自帶 LaTeX 模板 — 從 Overleaf、GitHub 或本地檔案皆可
- **雙語支援**：中文和英文輸出（若素材包含雙語版本）
- **AI 審稿**：透過 Claude Code、Gemini CLI 或指定模型取得履歷回饋
- **求職偵察**：面試前深度調查公司、團隊、面試官
- **策略分析階段**：生成前先分析 JD 與個人經歷的匹配度，也可跳過直接生成

## 快速開始

### 前置需求

- [Claude Code](https://claude.ai/code) CLI
- LaTeX 編譯器（XeLaTeX、pdfLaTeX 或 LuaLaTeX）
- LaTeX 履歷模板（自己的、來自 Overleaf 或 GitHub）

### 安裝

```bash
# 在 Claude Code 中安裝外掛
/plugin install offer-hunter
```

### 初始設定

在你的專案中執行 setup skill：

```
/setup-resume
```

這會：
1. 建立 `material/` 目錄，包含履歷素材的範例檔案
2. 設定你的 LaTeX 模板來源（本地路徑、Overleaf 或 GitHub）
3. 建立輸出目錄

### 生成履歷

```
/generate-resume "AI Engineer at Company X"
```

或提供 JD 網址：
```
/generate-resume https://example.com/job-posting
```

或直接貼上 JD 文字：
```
/generate-resume --skip-strategy
> 在此貼上你的 JD...
```

### 審閱履歷

生成後，取得 AI 回饋：
```
@resume-reviewer
```

### 調查求職目標

面試前，調查公司/團隊/面試官：
```
/job-recon "調查 Company X 的 AI 團隊"
```

## 運作方式

```
material/           # 你的履歷素材（唯一資料來源）
├── personal-info.md
├── experience.md
├── education.md
├── skills.md
└── projects/*.md

your-template/      # 你的 LaTeX 模板（setup 時設定）
├── cv.tex
├── cover-letter.tex
└── ...

docs/resumes/       # 各職缺生成的履歷輸出
└── company-position/
    ├── cv.tex + cv.pdf
    └── cover-letter.tex + cover-letter.pdf
```

## 外掛技能

| 技能 | 指令 | 說明 |
|------|------|------|
| 初始設定 | `/setup-resume` | 初始化工作區並設定模板 |
| 生成履歷 | `/generate-resume` | 建立客製化 CV 和求職信 |
| 求職偵察 | `/job-recon` | 調查公司、團隊和相關人物 |
| 履歷審閱 | `@resume-reviewer` | AI 驅動的履歷回饋 |

## 授權

MIT
