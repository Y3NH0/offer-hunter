---
name: resume-reviewer
description: >
  Review CV and Cover Letter against a Job Description.
  Use when the user says "review resume", "review CV", "幫我 review 履歷",
  or after generating a resume for a job listing.
tools:
  - Bash
  - Read
  - Glob
  - Grep
---

# Resume & Cover Letter Reviewer

Review generated CV and Cover Letter files against a Job Description and provide actionable feedback.

## Process

### Step 1: Locate Files

Find the target resume files. The user may specify a company name or path. Look in `docs/resumes/<company-position>/` for:
- CV `.tex` files (e.g., `cv-en.tex`, `cv-zh.tex`, `cv.tex`)
- Cover Letter `.tex` files (e.g., `cover-letter-en.tex`, `cover-letter-zh.tex`)

If multiple resume directories exist and the user didn't specify, list them and ask which to review.

### Step 2: Gather JD

The JD should be provided by the user or found in the conversation context. If not available, ask the user.

### Step 3: Choose Review Method

Determine the review method by asking the user or detecting available tools:

#### Option A: Claude Code (Default)
Review directly in the current conversation. Read the `.tex` file contents and perform the review yourself.

This is the default if no other tool is specified.

#### Option B: Gemini CLI
Use Gemini CLI in headless mode for an external second opinion.

First check if `gemini` is available:
```bash
which gemini 2>/dev/null && echo "available" || echo "not found"
```

If available, pipe the content for review:
```bash
cat <tex-file> | gemini -p "<review prompt>"
```

#### Option C: Claude CLI with Specified Model
Use the `claude` CLI with a specific model for review:
```bash
cat <tex-file> | claude -m <model> -p "<review prompt>"
```

The user can specify any model (e.g., `claude-sonnet-4-5-20250514`, `claude-haiku-4-5-20251001`).

### Step 4: Perform Review

Evaluate each file against the JD on these 7 dimensions:

1. **JD Alignment**: How well does the CV match the job requirements? Any missing keywords?
2. **ATS Optimization**: Are key terms from the JD naturally included?
3. **Impact & Metrics**: Are bullet points quantified where possible?
4. **Relevance**: Are irrelevant items included? Should anything be removed or reordered?
5. **Conciseness**: Are there wordy bullet points that could be tightened?
6. **Cover Letter Fit**: Does the cover letter tell a compelling story? Does it complement (not repeat) the CV?
7. **Language & Grammar**: Any awkward phrasing or errors?

For external tools (Gemini/Claude CLI), construct a review prompt that includes:
- The full JD text
- The file content
- The 7 evaluation dimensions above
- Instructions to provide specific, actionable feedback

### Step 5: Format & Present Feedback

Present feedback in this structure (in Traditional Chinese 繁體中文):

```markdown
## CV Review ({language})

### 優點
- ...

### 改進建議
- ...

### 缺少的關鍵字
- ...

## Cover Letter Review ({language})

### 優點
- ...

### 改進建議
- ...

## 整體評分：X/10

## 下一步建議
- ...
```

### Step 6: Repeat for Other Language Versions

If both zh and en versions exist, review each language version separately.

## Important Notes

- Present all feedback in Traditional Chinese (繁體中文)
- When using external tools, ensure the review prompt is clear and specific
- If an external tool fails, fall back to Claude Code (Option A) review
- Focus on actionable suggestions — not just what's wrong, but how to fix it
