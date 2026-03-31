# Offer Hunter

> 別再到處投一樣的履歷了。投*對的*那份。

Offer Hunter 是一個 [Claude Code](https://claude.ai/code) 外掛，把職缺描述變成量身打造的專業履歷 — 全自動。帶上你自己的 LaTeX 模板，指向一個職缺，就能得到真正符合公司需求的 CV + Cover Letter。

## 它能做什麼

```
你 + 職缺描述 + 你的模板 = 幾秒內產出客製化履歷
```

**生成** — 貼上 JD 或網址。Offer Hunter 讀取你的經歷，對應職缺需求，生成客製化的 CV + Cover Letter（LaTeX 格式）。

**審閱** — 讓 AI 根據 JD 幫你的履歷打分。在送出前抓出遺漏的關鍵字、薄弱的條列項、和對齊落差。

**偵察** — 調查你的面試目標：主管的背景、團隊的技術棧、來自 Glassdoor/PTT/Dcard 的真實面試經驗、以及你該準備什麼。

**簡報** — 生成 Marp 格式的面試自我介紹簡報。根據職位量身打造，配合你的時間限制，附帶講者備註。

## 快速開始

```bash
# 方法 1：從 GitHub 安裝
claude plugin install https://github.com/yenhochen/offer-hunter

# 方法 2：clone 後本地安裝
git clone https://github.com/yenhochen/offer-hunter.git
claude plugin install ./offer-hunter

# 接著在你的專案中：
/setup-resume                    # 設定模板 + 素材
/generate-resume                 # 生成你的第一份履歷
```

### 前置需求

- [Claude Code](https://claude.ai/code) CLI
- LaTeX 編譯器（XeLaTeX、pdfLaTeX 或 LuaLaTeX）
- LaTeX 履歷模板（自己的、來自 [Overleaf](https://www.overleaf.com/latex/templates/tagged/cv) 或 GitHub）

### 初始設定

`/setup-resume` 會引導你完成：

1. **素材** — 建立 `material/` 目錄，包含經歷、學歷、技能、專案的範本檔案
2. **模板** — 連接你的 LaTeX 模板（本地路徑、Overleaf clone 或 GitHub repo）
3. **設定** — 儲存到 `.claude/resume-config.json`，只需設定一次

### 生成履歷

```
/generate-resume "Stripe Senior ML Engineer"
```

背後做了什麼：

1. **讀取 JD** — 提取需求、關鍵字、主題
2. **讀取你的素材** — 所有經歷、專案、技能
3. **制定策略** — 對應你的經歷到 JD，決定強調什麼、刪減什麼（你核准後才生成）
4. **生成** — 依照你模板的結構和指令產出 `.tex` 檔案
5. **編譯** — 跑 LaTeX 產出 PDF

你也可以餵 URL 或直接貼 JD 文字。加 `--skip-strategy` 跳過策略階段直接生成。

### 審閱履歷

```
@resume-reviewer
```

從 7 個維度評分：JD 對齊度、ATS 最佳化、影響力指標、相關性、精簡度、Cover Letter 契合度、語言品質。可選擇 Claude Code（預設）、Gemini CLI 或任何 Claude 模型。

### 求職偵察

```
/job-recon "調查 Datadog 的 Platform 團隊"
```

產出結構化情報報告：關鍵人物背景、團隊技術棧、開源產出、近期公司動態、來自社群平台的真實面試經驗、以及具體的準備建議。

### 面試簡報

```
/interview-slides "Google SWE 面試，3 分鐘"
```

生成 Marp 格式的自我介紹簡報：從你的背景到為什麼選擇這個職位的敘事弧線，附帶講者備註和時間指引。可用 Marp CLI 或 VS Code 匯出 PDF/PPTX。

## 運作方式

```
material/               # 你的內容（唯一資料來源）
├── personal-info.md    #   姓名、聯繫方式、連結
├── experience.md       #   工作經歷
├── education.md        #   學歷 & 研究
├── skills.md           #   技術技能
└── projects/*.md       #   專案描述

your-template/          # 你的 LaTeX 模板
├── cv.tex              #   （/setup-resume 時設定）
└── cover-letter.tex

docs/resumes/           # 輸出（每個申請一個資料夾）
└── stripe-sr-ml-eng/
    ├── cv.tex + cv.pdf
    └── cover-letter.tex + cover-letter.pdf
```

素材寫一次。每次申請生成不同的、量身打造的履歷。

## 指令

| 指令 | 功能 |
|------|------|
| `/setup-resume` | 一次性設定：模板 + 素材（含引導式素材收集） |
| `/generate-resume` | 生成客製化 CV & Cover Letter（`--skip-strategy`、`--cv-only`、`--cover-letter-only`） |
| `/job-recon` | 深度調查公司/團隊/人物 |
| `/interview-slides` | 生成 Marp 面試簡報 |
| `@resume-reviewer` | AI 履歷審閱 & 評分（7 維度評分表） |

## 授權

MIT
