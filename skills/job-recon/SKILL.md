---
name: job-recon
description: >
  Deep research on job targets — people, teams, or departments — producing a structured
  intelligence report. Trigger when user says "research", "investigate", "look into",
  "recon", "survey", or provides a person/team name at a company for background research.
---

# Job Recon — Target Intelligence Research

Conduct deep open-source intelligence research on people, teams, or departments relevant to a job opportunity. Produce a structured report to help the user prepare for interviews.

## Input

The user provides any combination of:

- **Person name** (required or optional): Target individual — e.g., hiring manager, interviewer, team lead
- **Company name** (required): Target company
- **Department/Team** (optional): e.g., AI Lab, Security Research, Platform Engineering
- **Job listing URL or title** (optional): Provides additional context

If insufficient information is provided, ask the user for at least a company name + person name or department name.

## Process

### Step 1: Plan Search Strategy

Determine the scope based on input. Always go broad-to-deep: establish the big picture first, then drill down.

**Core research dimensions** (cover all that apply):

| Dimension | Description |
|-----------|-------------|
| Person background | Title, education, career path, LinkedIn, public talks |
| Team structure | Team mission, known members, reporting chain, team size |
| Core tech/products | Products, projects, and tech stack the team owns |
| Open source/academic output | GitHub repos, HuggingFace models, papers, blog posts |
| Hiring signals | Related job postings, salary ranges, team descriptions |
| Interview experiences | First-hand interview reports from community platforms |
| Company news | Recent announcements, partnerships, product launches |

### Step 2: Multi-Round Search

Use WebSearch for **5 rounds**, each with different angles and keyword combinations. Search in **both English and the target company's local language** (e.g., Chinese for Taiwan/China companies, Japanese for Japan companies, etc.).

Run **multiple WebSearch queries in parallel** within each round — do not serialize them.

**Round 1: Establish Profile**
- `"person name" "company name"` — find the right person
- `"person name" "company name" LinkedIn` — confirm title, education
- `company name department team` — team positioning

**Round 2: Deep-Dive on Tech & Products**
- `company name product/project technical details`
- `company name open source OR github OR huggingface`
- `person name conference OR talk OR speaker OR keynote`

**Round 3: Hiring & Team Context**
- `company name department hiring OR jobs OR careers`
- `company name department team structure OR engineering blog`
- `company name recent news OR partnership OR announcement`

**Round 4: Interview Experiences & Community Intel**

Search for first-hand interview reports on platforms relevant to the target region:

| Region | Platforms to search |
|--------|-------------------|
| Global | Glassdoor, Blind, Reddit, Medium, levels.fyi |
| Taiwan | PTT (Soft_Job, Tech_Job, Salary), Dcard, interview.tw, CakeResume |
| China | Maimai (脉脉), Nowcoder (牛客), Zhihu (知乎), Offershow |
| Japan | OpenWork, Glassdoor JP, 転職会議 |
| Korea | Blind KR, JobPlanet |

Search queries should combine the company name with interview-related terms in the appropriate language. The goal is to collect **real interviewees' experiences**: interview process (rounds, format), question types, company culture impressions, and offer negotiation details.

**If a platform is inaccessible** (gated content, login wall, 403): Extract what you can from search result snippets. Note in the report that full access was not available. Do not skip the platform entirely — snippets from search results often contain useful summaries.

**Round 5: Follow-Up & Verification**
- Deep-dive on key leads discovered in earlier rounds (e.g., found an open-source project → check its GitHub/HuggingFace page)
- Verify key claims by cross-referencing multiple sources
- Fill any remaining gaps from the research dimensions

### Step 3: Deep Fetch (WebFetch)

Use WebFetch to retrieve full content from high-value pages such as:

- HuggingFace model cards / org pages
- GitHub repository READMEs
- Company engineering blog posts
- Detailed job description pages
- Conference talk abstracts or slides pages

If WebFetch fails (403, redirect, timeout):
1. Try the Google Cache or web archive version: `webcache.googleusercontent.com/search?q=cache:{url}`
2. Extract information from search result snippets already collected
3. Note in the report that the full page was inaccessible

### Step 4: Compile Output

Organize all findings into a structured `.md` file and save to `docs/research/`.

**Before writing, review all collected information for:**
- **Conflicting facts** — if sources disagree (e.g., different titles for the same person), note both and indicate which source is more recent/authoritative
- **Freshness** — flag any information that appears to be more than 1 year old with a note like "(as of {year})"
- **Confidence** — clearly distinguish confirmed facts from inferences

## Output

### File Location

`docs/research/{company}-{topic}.md`

Examples:
- `docs/research/google-deepmind-taipei-team.md`
- `docs/research/stripe-payments-engineering.md`
- `docs/research/nvidia-nim-microservices.md`

### Document Structure

Follow this structure strictly. Sections with no findings should note "No public information found" rather than being omitted. Write the report in the **same language the user is communicating in**.

```markdown
# {Company} {Team/Department} & {Person} — Intelligence Report

> Research date: {YYYY-MM-DD}
> Purpose: Interview preparation

---

## 1. {Person} — Background

| Field | Info |
|-------|------|
| Name | ... |
| LinkedIn | ... |
| Current role | ... |
| Education | ... |
| Expertise | ... |
| Location | ... |

### Public Activity
- Talks, conference appearances, published articles, etc.

### Leadership Style (Inferred)
- Inferences from public info (career transitions, team culture signals, etc.)
- *Label all inferences explicitly*

---

## 2. Team Overview

### Mission & Positioning
### Known Members
### Team Size & Hiring

---

## 3. Core Tech/Products — Deep Dive

### Overview
### Technical Architecture
### Open Source Output (if any)
- Models / Datasets / Repos — detailed list

---

## 4. Recent Company News

### Major News / Partnerships
### Product Launches

---

## 5. Interview Experience Roundup

Compiled from community platforms (Glassdoor, Blind, Reddit, regional platforms, etc.):

### Interview Process
- Number of rounds, format (phone/video/onsite), timeline

### Question Types & Preparation
- Technical question types (coding, system design, domain-specific, etc.)
- Behavioral interview topics
- Take-home assignments (if any)

### Interviewee Impressions
- Company culture, interviewer demeanor, overall experience
- Offer-related info (if publicly shared)

> If no interview reports are found, note "No public interview experiences found" — do not fabricate.

---

## 6. Interview Preparation Advice

### Key Talking Points (show you've done your homework)
### Likely Questions
### Good Questions to Ask

---

## 7. Sources
- All referenced URLs as titled markdown links
- Note any sources that were inaccessible (with reason)
```

If the research target is a team rather than a specific person, omit the personal background table in section 1 and focus on team structure and technical analysis.

## Quality Standards

- Every fact must be backed by a source URL, listed in "Sources"
- Clearly distinguish confirmed facts from inferences — label inferences explicitly
- Be precise with names, titles, and company names — do not confuse people with similar names
- Search in both English and the target's local language
- Interview preparation advice should be specific and actionable, not generic
- Flag outdated information with the year/date it was current
- When sources conflict, present both versions with context on which is more recent

## Privacy Guidelines

- Only include information that is **publicly available** (published by the person themselves, on company pages, or in public forums)
- Do not include personal contact information beyond what's on official company or LinkedIn pages
- Do not include information from private social media accounts
- If you find sensitive personal information (home address, personal phone, family details), do not include it in the report

## Prerequisites

- WebSearch and WebFetch tools must be available
