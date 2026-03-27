---
name: job-recon
description: >
  深度調查求職目標的人物、團隊、或部門，產出結構化情報 .md 檔案。
  當使用者說「幫我調查」、「研究一下這個人/團隊/主管」、「survey」、
  「recon」、「幫我查一下面試官」、「了解一下這個部門」，
  或提供一個人名+公司名要求背景調查時觸發。
  即使使用者只是隨口提到想了解某個公司的某個人或團隊，也應該觸發。
---

# Job Recon — 求職目標情報調查

針對求職相關的人物、團隊、或部門進行深度公開資訊調查，產出一份結構化的情報文件，幫助使用者在面試前充分準備。

## Input

使用者提供以下資訊的任意組合：

- **人名**（必要或可選）：目標人物，例如主管、面試官、團隊負責人
- **公司名**（必要）：目標公司
- **部門/團隊**（可選）：例如 AI Lab、Security Research
- **職缺連結或名稱**（可選）：提供更多脈絡

如果資訊不足以開始（例如只說「幫我調查」沒給任何名字），詢問使用者至少提供公司名 + 人名或部門名。

## Process

### Step 1: 規劃搜尋策略

根據輸入決定調查範圍。調查永遠從廣到深：先建立全貌，再逐項深挖。

**核心調查維度**（每次都要涵蓋）：

| 維度 | 說明 |
|------|------|
| 人物背景 | 職稱、學歷、職涯路徑、LinkedIn、公開演講 |
| 團隊架構 | 團隊定位、已知成員、上層主管、團隊規模 |
| 核心技術/產品 | 團隊負責的產品、專案、技術棧 |
| 開源/學術產出 | GitHub repos、HuggingFace models、論文、部落格文章 |
| 招聘資訊 | 相關職缺的 JD、薪資範圍、團隊描述 |
| 面試心得 | PTT/Dcard/Glassdoor/Medium/interview.tw 上的面試經驗分享 |
| 公司動態 | 近期新聞、合作夥伴、產品發布 |

### Step 2: 多輪搜尋

使用 WebSearch 進行**至少 3-4 輪**搜尋，每輪用不同角度和關鍵字組合。善用中英文雙語搜尋。

**第一輪：建立基本輪廓**
- `"人名" "公司名"` — 找到正確的人
- `"人名" "公司名" LinkedIn` — 確認職稱、學歷
- `公司名 部門名 團隊` — 團隊定位

**第二輪：深挖技術與產品**
- `公司名 核心產品/專案名 技術細節`
- `公司名 open source OR github OR huggingface`
- `人名 conference OR talk OR speaker OR 演講`

**第三輪：招聘與團隊脈絡**
- `公司名 部門名 hiring OR 職缺 OR CakeResume OR LinkedIn jobs`
- `公司名 部門名 team structure OR 團隊`
- `公司名 最近新聞 OR partnership OR announcement`

**第四輪：面試心得與社群情報**
- `公司名 面試 心得 PTT` — PTT Soft_Job / Tech_Job / Salary 等看板
- `公司名 面試 Dcard` — Dcard 工作板
- `公司名 interview experience glassdoor`
- `公司名 職缺名稱 面試分享 OR 面試經驗`
- `公司名 interview.tw OR Medium 面試`

這一輪的目的是蒐集**真實面試者的經驗談**：面試流程（幾關、形式）、考題類型、公司文化觀感、offer 談判等。這些第一手資訊對面試準備非常有價值。

**第五輪：補充與驗證**
- 針對前幾輪發現的關鍵線索深挖（例如發現了一個開源專案，就去查其 GitHub/HuggingFace 頁面）
- 用 WebFetch 取得重要頁面的完整內容（model cards、GitHub README、職缺 JD 等）

每輪搜尋盡量**平行發出多個 WebSearch**，不要一個一個串行搜尋。

### Step 3: 深度取得（WebFetch）

對以下類型的頁面使用 WebFetch 取得完整內容：

- HuggingFace model cards / org pages
- GitHub repository README
- 公司技術部落格文章
- 職缺詳細 JD 頁面

如果 WebFetch 失敗（403、redirect），嘗試替代 URL 或從搜尋結果摘要中提取資訊。

### Step 4: 彙整輸出

將所有調查結果整理成結構化 .md 檔案，存到 `docs/research/`。

## Output

### 檔案位置

`docs/research/<company>-<topic>.md`

例如：
- `docs/research/trendmicro-ai-lab-stephanie-chou.md`
- `docs/research/google-deepmind-taipei-team.md`
- `docs/research/appier-ai-engineering.md`

### 文件結構

嚴格遵循以下結構（根據實際調查到的內容調整，沒查到的章節可省略但要註明「未找到公開資訊」）：

```markdown
# {公司} {團隊/部門} 與 {人名} — 深度情報整理

> 調查日期：{YYYY-MM-DD}
> 目的：面試準備

---

## 1. {人名} — 個人背景

| 欄位 | 資訊 |
|------|------|
| 姓名 | ... |
| LinkedIn | ... |
| 現職 | ... |
| 學歷 | ... |
| 專業背景 | ... |
| 所在地 | ... |

### 公開活動
- 演講、conference 出席、文章發表等

### 管理風格推測
- 從公開資訊推斷（背景轉型路徑、團隊文化線索等）

---

## 2. 團隊概況

### 組織定位
### 已知團隊成員
### 團隊規模與招聘

---

## 3. 核心技術/產品 — 深度分析

### 概述
### 技術架構
### 開源產出（如有）
- Models / Datasets / Repos 詳細列表

---

## 4. 公司近期動態

### 重大新聞/合作
### 產品發布

---

## 5. 面試心得彙整

整理從 PTT、Dcard、Glassdoor、Medium、interview.tw 等來源蒐集到的面試經驗：

### 面試流程
- 幾關、每關形式（電話/視訊/onsite）、時程

### 考題與準備方向
- 技術題類型（coding、system design、ML theory 等）
- 行為面試題型
- take-home assignment（如有）

### 面試者觀感
- 公司文化、面試官態度、整體體驗
- offer 相關資訊（如有公開）

> 如果搜尋不到面試心得，註明「未找到公開面試分享」，不要捏造。

---

## 6. 面試準備建議

### 關鍵話題（展現你做過功課）
### 可能被問到的方向
### 可以主動提問的問題

---

## 7. 資料來源
- 所有引用的 URL，帶標題的 markdown links
```

如果調查對象是團隊而非特定個人，省略第 1 節的個人資訊表格，將重心放在團隊架構與技術分析。

## 品質標準

- 每個事實都要有來源 URL 支持，最終列在「資料來源」
- 區分「確認的事實」和「推測」，推測要明確標註
- 人名、職稱、公司名要精確，不要混淆同名不同人
- 中英文資訊都要搜尋，呈現以繁體中文為主
- 面試準備建議要具體且可操作，不要泛泛而談

## Prerequisites

- WebSearch 和 WebFetch 工具需可用
