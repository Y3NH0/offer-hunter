---
name: resume-reviewer
description: >
  Review CV and Cover Letter against a Job Description.
  Use when the user says "review resume", "review CV", "check my resume",
  or after generating a resume for a job listing.
tools: Bash, Read, Glob, Grep
---

# Resume & Cover Letter Reviewer

Review generated CV and Cover Letter files against a Job Description and provide structured, actionable feedback with a scored rubric.

## Process

### Step 1: Locate Files

Find the target resume files. The user may specify a company name or path.

1. Read `.claude/resume-config.json` to find the output directory (default: `docs/resumes/`)
2. List subdirectories in the output directory
3. If the user specified a company/position, match it to a subdirectory
4. If multiple directories exist and the user didn't specify, list them and ask which to review

Within the matched directory, look for:
- CV files: `cv.tex`, `cv-en.tex`, `cv-zh.tex` (or similar patterns)
- Cover Letter files: `cover-letter.tex`, `cover-letter-en.tex`, `cover-letter-zh.tex`

**If no `.tex` files are found:** Check for `.pdf` files. If only PDFs exist, inform the user that review works best with `.tex` source files but offer to review the PDF content (read via the Read tool).

### Step 2: Gather JD

The JD is needed for alignment review. Look for it in this order:

1. **Conversation context** — the user may have just run `/generate-resume` with a JD
2. **Ask the user** — request the JD text or URL
3. **Check the output directory** — look for a `jd.md` or `jd.txt` file saved during generation

If no JD is available, perform a general review (skip JD-specific dimensions 1 and 2, note this in the output).

### Step 3: Choose Review Method

Present the user with options:

> How would you like this review performed?
> 1. **Claude Code** (default) — I'll review it directly in this conversation
> 2. **Gemini CLI** — get an external second opinion via Gemini
> 3. **Claude CLI with a specific model** — e.g., claude-sonnet-4-5-20250514
>
> Press Enter for default, or specify your choice.

#### Option A: Claude Code (Default)
Review directly. Read the `.tex` file contents and perform the review in-conversation. This is always available.

#### Option B: Gemini CLI
First verify availability:
```bash
command -v gemini >/dev/null 2>&1 && echo "available" || echo "not found"
```

If available, construct the review prompt and run:
```bash
gemini -p "$(cat <<'PROMPT'
You are a professional resume reviewer. Review the following CV/Cover Letter against the Job Description.

## Job Description
{jd_text}

## Resume Content
{tex_content}

## Evaluation Dimensions
{scoring_rubric}

Provide specific, actionable feedback with scores for each dimension.
PROMPT
)"
```

If Gemini is not found, inform the user and fall back to Option A.

#### Option C: Claude CLI with Specified Model
Verify the Claude CLI is available:
```bash
command -v claude >/dev/null 2>&1 && echo "available" || echo "not found"
```

If available, run with the user's chosen model:
```bash
claude -p "$(cat <<'PROMPT'
You are a professional resume reviewer. Review the following CV/Cover Letter against the Job Description.

## Job Description
{jd_text}

## Resume Content
{tex_content}

## Evaluation Dimensions
{scoring_rubric}

Provide specific, actionable feedback with scores for each dimension.
PROMPT
)" --model {model_name}
```

If the CLI is not found or the command fails, inform the user and fall back to Option A.

### Step 4: Perform Review — Scoring Rubric

Evaluate each file against the JD on these 7 dimensions. Each dimension is scored 1-10 using the rubric below.

#### Dimension 1: JD Alignment (weight: 20%)
How well does the CV match the stated job requirements?

| Score | Criteria |
|-------|----------|
| 9-10 | Every key requirement from the JD is addressed with specific evidence |
| 7-8 | Most requirements addressed, 1-2 minor gaps |
| 5-6 | Some requirements addressed but notable gaps |
| 3-4 | Significant misalignment — many requirements unaddressed |
| 1-2 | Resume appears untailored to this JD |

#### Dimension 2: ATS Optimization (weight: 15%)
Are key terms from the JD naturally included?

| Score | Criteria |
|-------|----------|
| 9-10 | All critical keywords present, naturally integrated |
| 7-8 | Most keywords present, reads naturally |
| 5-6 | Some keywords missing or awkwardly forced |
| 3-4 | Many keywords missing, likely to be filtered |
| 1-2 | No keyword optimization evident |

#### Dimension 3: Impact & Metrics (weight: 15%)
Are achievements quantified with concrete results?

| Score | Criteria |
|-------|----------|
| 9-10 | Most bullet points include metrics (%, $, time, scale) |
| 7-8 | Several quantified achievements, some bullets could be stronger |
| 5-6 | A few metrics, but many vague claims ("improved performance") |
| 3-4 | Mostly task descriptions, rarely quantified |
| 1-2 | Pure duty/task listing with no impact |

#### Dimension 4: Relevance & Prioritization (weight: 15%)
Is content ordered by relevance? Are irrelevant items omitted?

| Score | Criteria |
|-------|----------|
| 9-10 | Most relevant experience first, nothing extraneous |
| 7-8 | Good ordering, minor irrelevant items |
| 5-6 | Some relevant content buried, some noise |
| 3-4 | Poor ordering, significant irrelevant content |
| 1-2 | Reads like a generic dump of all experience |

#### Dimension 5: Conciseness & Clarity (weight: 10%)
Is every bullet tight, scannable, and jargon-appropriate?

| Score | Criteria |
|-------|----------|
| 9-10 | Every bullet is crisp and scannable, appropriate detail level |
| 7-8 | Mostly concise, 1-2 wordy bullets |
| 5-6 | Several bullets too long or unclear |
| 3-4 | Generally verbose or unclear |
| 1-2 | Dense, hard to scan |

#### Dimension 6: Cover Letter Effectiveness (weight: 15%)
Does the cover letter tell a compelling, complementary story?

| Score | Criteria |
|-------|----------|
| 9-10 | Clear narrative arc, specific to this role, complements (not repeats) CV |
| 7-8 | Good story with minor areas for improvement |
| 5-6 | Generic or partially repeats the CV |
| 3-4 | Template-like, could apply to any job |
| 1-2 | Missing or completely generic |

*Skip this dimension if no cover letter exists. Redistribute weight to other dimensions.*

#### Dimension 7: Language & Grammar (weight: 10%)
Is the writing polished and error-free?

| Score | Criteria |
|-------|----------|
| 9-10 | Flawless grammar, professional tone, natural phrasing |
| 7-8 | Minor issues that don't distract |
| 5-6 | Several noticeable errors or awkward phrases |
| 3-4 | Frequent errors that undermine professionalism |
| 1-2 | Pervasive errors |

#### Calculating the Overall Score

```
Overall = (D1 × 0.20) + (D2 × 0.15) + (D3 × 0.15) + (D4 × 0.15) + (D5 × 0.10) + (D6 × 0.15) + (D7 × 0.10)
```

If JD is unavailable, skip D1 and D2 and redistribute their weights proportionally across D3-D7.
If no cover letter, skip D6 and redistribute its weight.

### Step 5: Format & Present Feedback

Present feedback in the **same language the user is communicating in**.

```markdown
## Resume Review: {company} — {position}

### CV Review ({language version})

#### Scores

| Dimension | Score | Key Finding |
|-----------|-------|-------------|
| JD Alignment | X/10 | ... |
| ATS Optimization | X/10 | ... |
| Impact & Metrics | X/10 | ... |
| Relevance | X/10 | ... |
| Conciseness | X/10 | ... |
| Language & Grammar | X/10 | ... |

#### Strengths
- (specific items with line references)

#### Priority Improvements
1. (most impactful fix — with concrete rewrite suggestion)
2. (second most impactful)
3. ...

#### Missing Keywords
- keyword1, keyword2, ... (from JD, not found in CV)

---

### Cover Letter Review ({language version})

#### Score: X/10

#### Strengths
- ...

#### Improvements
1. (with concrete rewrite suggestion)

---

### Overall Score: X.X/10

### Verdict
(One-paragraph summary: is this resume ready to submit? What's the single most important change?)

### Suggested Next Steps
- [ ] (actionable item 1)
- [ ] (actionable item 2)
- [ ] ...
```

**Key rules for feedback:**
- Every improvement must include a **concrete rewrite suggestion**, not just "this could be better"
- Reference specific lines or sections from the `.tex` file
- Prioritize improvements by impact — the first suggestion should be the one that improves the resume the most

### Step 6: Review Additional Language Versions

If both zh and en versions exist, review each separately with the same rubric. After reviewing both, add a cross-language consistency note:

```markdown
### Cross-Language Consistency
- Are the same experiences included in both versions?
- Do metrics and facts match across languages?
- Any content in one version that should be added to the other?
```

## Important Notes

- Present all feedback in the same language the user is communicating in
- If an external tool fails or produces low-quality output, fall back to Claude Code review and note the fallback
- Focus on actionable suggestions — every problem identified must have a proposed fix
- When reviewing `.tex` files, ignore LaTeX formatting commands — focus on the content
- If the resume is already strong (8+/10), acknowledge that and focus suggestions on the gap from 8 to 10
