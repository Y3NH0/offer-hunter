# Website Architecture Plan - thisisyenho.com

## Personal Portfolio Website for Yen-Ho Chen (AI Agent Engineer)

**Document Version:** 1.0
**Date:** 2026-02-13
**Status:** Planning Phase

---

## Table of Contents

1. [Overview](#1-overview)
2. [Tech Stack](#2-recommended-tech-stack)
3. [Site Structure & Routing](#3-site-structure--routing)
4. [Component Architecture](#4-component-architecture)
5. [Data Flow](#5-data-flow-markdown-to-pages)
6. [Internationalization (i18n)](#6-internationalization-i18n)
7. [Design System](#7-design-system)
8. [SEO & Performance](#8-seo--performance)
9. [Deployment Strategy](#9-deployment-strategy)
10. [File Structure](#10-estimated-file-structure)
11. [Design Inspiration](#11-design-inspiration--reference-sites)
12. [Phased Roadmap](#12-phased-roadmap)
13. [Content Strategy](#13-content-strategy)

---

## 1. Overview

### Goals
- Present Yen-Ho Chen's professional profile as an AI Agent Engineer
- Showcase projects, work experience, and technical skills
- Support bilingual content (Chinese / English) with seamless switching
- Provide a modern, performant, and accessible web presence
- Use existing Markdown resume files as the primary content source
- Enable future extensibility (blog, demos, interactive AI showcases)

### Target Audience
- Recruiters and hiring managers in the AI/ML space
- Potential collaborators and open-source contributors
- Technical community members

---

## 2. Recommended Tech Stack

### Core Framework
| Technology | Version | Purpose |
|---|---|---|
| **Next.js** | 15.x (App Router) | React framework with SSG/SSR, file-based routing |
| **React** | 19.x | UI library |
| **TypeScript** | 5.x | Type safety throughout the project |

### Styling
| Technology | Purpose |
|---|---|
| **Tailwind CSS v4** | Utility-first CSS framework |
| **tailwind-merge** | Merge Tailwind classes without conflicts |
| **clsx** | Conditional className utility |
| **next-themes** | Dark/light mode theme management |

### Internationalization
| Technology | Purpose |
|---|---|
| **next-intl** | i18n for Next.js App Router (recommended over next-i18next for App Router) |

**Why next-intl over alternatives:**
- First-class App Router support with Server Components
- Type-safe message access
- Built-in routing with locale prefixes (`/zh/about`, `/en/about`)
- Supports both Server and Client Components
- Active maintenance and growing community
- Much simpler setup than next-i18next for App Router

### Content Processing
| Technology | Purpose |
|---|---|
| **gray-matter** | Parse YAML frontmatter from Markdown files |
| **unified / remark / rehype** | Markdown-to-HTML pipeline with plugin support |
| **rehype-pretty-code** (or **shiki**) | Syntax highlighting for code blocks |
| **remark-gfm** | GitHub Flavored Markdown support |

**Alternative considered:** MDX via `@next/mdx` or `contentlayer2`. However, since the content is structured resume data (not blog posts with interactive components), a simpler Markdown parsing pipeline is sufficient and more flexible. For the future blog, MDX can be introduced later.

### Animation & Interaction
| Technology | Purpose |
|---|---|
| **framer-motion** | Page transitions and scroll animations |
| **tailwindcss-animate** | Simple CSS-based animations |

### Icons
| Technology | Purpose |
|---|---|
| **lucide-react** | Clean, consistent icon library |
| **react-icons** | Additional icons (for brand logos like GitHub, LinkedIn) |

### SEO & Analytics
| Technology | Purpose |
|---|---|
| Next.js built-in `metadata` API | Meta tags, Open Graph, Twitter cards |
| **next-sitemap** | Auto-generate sitemap.xml and robots.txt |
| **Google Analytics 4** (optional) | Visitor analytics |

### Development Tools
| Technology | Purpose |
|---|---|
| **ESLint** | Code linting (Next.js default config) |
| **Prettier** | Code formatting |
| **prettier-plugin-tailwindcss** | Auto-sort Tailwind classes |

---

## 3. Site Structure & Routing

### URL Structure (with i18n)

The site uses locale-prefixed routing. Chinese is the default locale, English is the alternate.

```
/                       -> Redirects to /zh (default locale)
/zh                     -> Home page (Chinese)
/en                     -> Home page (English)
/zh/about               -> About page (Chinese)
/en/about               -> About page (English)
/zh/experience          -> Work experience (Chinese)
/en/experience          -> Work experience (English)
/zh/projects            -> Projects showcase (Chinese)
/en/projects            -> Projects showcase (English)
/zh/projects/[slug]     -> Individual project detail (Chinese)
/en/projects/[slug]     -> Individual project detail (English)
/zh/skills              -> Skills & tech stack (Chinese)
/en/skills              -> Skills & tech stack (English)
/zh/contact             -> Contact page (Chinese)
/en/contact             -> Contact page (English)

# Future (Phase 2+)
/zh/blog                -> Blog listing
/en/blog                -> Blog listing
/zh/blog/[slug]         -> Blog post
/en/blog/[slug]         -> Blog post
```

### Page Descriptions

| Page | Content | Data Source |
|---|---|---|
| **Home (/)** | Hero section with name, title, brief intro, CTA buttons, animated tech keywords | i18n JSON + resume data |
| **About** | Extended bio, education, achievements, personal traits, photo | resume markdown + i18n JSON |
| **Experience** | Timeline of work experience with expandable details | resume markdown |
| **Projects** | Card grid of projects with tags, links, and highlights | resume markdown + project detail files |
| **Skills** | Categorized skill visualization (grouped by domain) | resume markdown |
| **Contact** | Contact form (or mailto link), social links, resume download | i18n JSON |

---

## 4. Component Architecture

### Layout Components

```
RootLayout
 +-- ThemeProvider (next-themes)
 +-- NextIntlClientProvider
     +-- LocaleLayout
         +-- Header / Navigation
         |    +-- Logo
         |    +-- NavLinks
         |    +-- LocaleSwitcher (zh/en toggle)
         |    +-- ThemeToggle (dark/light)
         |    +-- MobileMenuButton
         +-- Main Content (page)
         +-- Footer
              +-- SocialLinks
              +-- Copyright
```

### Shared / UI Components

| Component | Description |
|---|---|
| `Button` | Reusable button with variants (primary, secondary, ghost, outline) |
| `Card` | Project cards, experience cards |
| `Badge` | Skill tags, technology badges |
| `SectionHeading` | Consistent section titles with optional subtitle |
| `Timeline` | Vertical timeline for experience/education |
| `TimelineItem` | Individual timeline entry |
| `SkillBar` / `SkillCategory` | Skill visualization by category |
| `ProjectCard` | Project showcase card with image, tags, links |
| `AnimatedSection` | Wrapper for scroll-triggered animations |
| `Container` | Max-width content wrapper |
| `Separator` | Visual divider |

### Page-Specific Components

| Page | Components |
|---|---|
| **Home** | `HeroSection`, `FeaturedProjects`, `SkillHighlights`, `CTASection` |
| **About** | `BioSection`, `EducationTimeline`, `AchievementList`, `PersonalTraits` |
| **Experience** | `ExperienceTimeline`, `ExperienceDetail`, `TechStackList` |
| **Projects** | `ProjectGrid`, `ProjectCard`, `ProjectFilter` |
| **Skills** | `SkillCategory`, `SkillBadgeGroup`, `ToolsList` |
| **Contact** | `ContactForm` (or `ContactInfo`), `SocialLinkGrid`, `ResumeDownload` |

---

## 5. Data Flow (Markdown to Pages)

### Architecture Overview

```
resume-zh.md / resume-en.md
         |
         v
   [gray-matter]  ---- extracts frontmatter + body
         |
         v
   [Custom Parser]  ---- splits markdown by ## headings
         |                into structured sections
         v
   [TypeScript Types] ---- strongly typed data objects
         |
         v
   [Data Access Layer] ---- functions like getExperience(locale),
         |                   getProjects(locale), getSkills(locale)
         v
   [Server Components] ---- fetch data at build time (SSG)
         |
         v
   [UI Components] ---- render the data with proper styling
```

### Detailed Data Pipeline

#### Step 1: Content Files
The existing `resume-zh.md` and `resume-en.md` serve as the primary data source. Additionally, structured JSON/YAML files will supplement the markdown for data that is easier to manage in structured format.

```
content/
  resume/
    zh.md          (copy/symlink of resume-zh.md)
    en.md          (resume-en.md, filled with real content)
  projects/
    zh/
      ai-mentor.md
      livestream-agent.md
      news-clustering.md
      bytebert.md
    en/
      ai-mentor.md
      livestream-agent.md
      news-clustering.md
      bytebert.md
  i18n/
    zh.json        (UI strings: nav labels, button text, etc.)
    en.json        (UI strings in English)
```

#### Step 2: Parsing Library (`lib/content.ts`)

```typescript
// Conceptual API (not actual implementation)

// Parse the full resume markdown into typed sections
function getResumeData(locale: 'zh' | 'en'): ResumeData

// Get specific sections
function getExperiences(locale: 'zh' | 'en'): Experience[]
function getProjects(locale: 'zh' | 'en'): Project[]
function getSkills(locale: 'zh' | 'en'): SkillCategory[]
function getEducation(locale: 'zh' | 'en'): Education[]
function getAchievements(locale: 'zh' | 'en'): Achievement[]

// Get individual project detail
function getProjectBySlug(slug: string, locale: 'zh' | 'en'): ProjectDetail
```

#### Step 3: TypeScript Types (`types/resume.ts`)

Key interfaces derived from the resume structure:

```typescript
interface Experience {
  company: string
  title: string
  period: string
  location: string
  description: string
  projects: ExperienceProject[]
  techStack: string[]
}

interface Project {
  slug: string
  title: string
  period: string
  organization: string
  description: string
  techStack: string[]
  features: Feature[]
  results: string[]
  links?: { github?: string; demo?: string; paper?: string }
}

interface SkillCategory {
  name: string
  subcategories: {
    name: string
    items: string[]
  }[]
}

interface Education {
  school: string
  degree: string
  period: string
  details: string[]
}
```

#### Step 4: Build-Time Rendering

All pages use Next.js `generateStaticParams()` to pre-render at build time. No client-side data fetching is needed for resume content. This ensures:
- Fast page loads (static HTML)
- Good SEO (content in HTML source)
- Zero runtime API calls

---

## 6. Internationalization (i18n)

### next-intl Setup with App Router

#### Routing Strategy

```
src/app/
  [locale]/
    layout.tsx       <-- NextIntlClientProvider wraps all pages
    page.tsx         <-- Home
    about/
      page.tsx
    experience/
      page.tsx
    ...
```

#### Message Files

UI strings (navigation, buttons, labels, meta descriptions) are stored in JSON:

```
messages/
  zh.json
  en.json
```

Example `zh.json`:
```json
{
  "nav": {
    "home": "首頁",
    "about": "關於我",
    "experience": "工作經歷",
    "projects": "專案作品",
    "skills": "技能",
    "contact": "聯繫方式"
  },
  "home": {
    "greeting": "你好，我是",
    "title": "AI Agent Engineer",
    "cta_projects": "查看專案",
    "cta_contact": "聯繫我"
  },
  "meta": {
    "title": "陳彥合 - AI Agent Engineer",
    "description": "AI Agent 工程師，專精於 Multi-Agent System、RAG、LLM 應用開發"
  }
}
```

#### Locale Detection & Switching

- Default locale: `zh` (Chinese)
- Supported locales: `['zh', 'en']`
- Middleware handles locale detection and redirect
- A `LocaleSwitcher` component in the header allows manual switching
- Locale preference is stored in a cookie for persistence

#### Content Locale Mapping

- UI strings -> `messages/zh.json` / `messages/en.json` (via next-intl)
- Resume content -> `content/resume/zh.md` / `content/resume/en.md` (via custom parser)
- Project details -> `content/projects/zh/*.md` / `content/projects/en/*.md`

---

## 7. Design System

### Visual Identity

- **Primary Font:** Inter (English) / Noto Sans TC (Chinese) -- loaded via `next/font`
- **Monospace Font:** JetBrains Mono (for code/tech elements)
- **Color Palette:**

| Token | Light Mode | Dark Mode |
|---|---|---|
| Background | `#ffffff` | `#0a0a0a` |
| Foreground | `#171717` | `#ededed` |
| Primary | `#2563eb` (blue-600) | `#3b82f6` (blue-500) |
| Secondary | `#64748b` (slate-500) | `#94a3b8` (slate-400) |
| Accent | `#8b5cf6` (violet-500) | `#a78bfa` (violet-400) |
| Muted | `#f1f5f9` (slate-100) | `#1e293b` (slate-800) |
| Border | `#e2e8f0` (slate-200) | `#334155` (slate-700) |

### Design Principles

1. **Clean & Minimal:** Generous whitespace, clear hierarchy, no visual clutter
2. **Content-First:** The resume content is the star; design serves it
3. **Subtle Animations:** Scroll-triggered fade-ins, smooth page transitions, hover states
4. **Responsive:** Mobile-first design, seamless experience across all viewports
5. **Accessible:** WCAG 2.1 AA compliance, proper heading hierarchy, keyboard navigation
6. **Dark Mode Default:** Many tech/AI audiences prefer dark mode; support both with a toggle

### Layout Guidelines

- **Max content width:** 1200px (container)
- **Narrow content width:** 768px (for text-heavy pages like About)
- **Grid system:** CSS Grid / Tailwind grid, 12-column
- **Spacing scale:** Follow Tailwind default (4px base unit)
- **Breakpoints:** Tailwind defaults (sm: 640px, md: 768px, lg: 1024px, xl: 1280px)

### Key Visual Elements

- **Hero section:** Full-viewport height, name in large type, animated title text, subtle gradient or particle background
- **Experience timeline:** Vertical timeline with cards, expandable sections
- **Project cards:** Image/screenshot + title + tech tags + brief description, hover effect
- **Skill visualization:** Grouped badges by category, possibly with subtle icons
- **Accent elements:** Thin colored borders, gradient underlines, subtle glow effects in dark mode

---

## 8. SEO & Performance

### SEO Strategy

#### Meta Tags (via Next.js Metadata API)
- Dynamic `title` and `description` per page and locale
- Open Graph images (auto-generated or custom per page)
- Twitter Card meta tags
- Canonical URLs with `hreflang` alternates for i18n
- JSON-LD structured data (Person, WebSite, BreadcrumbList schemas)

#### Example Metadata Configuration
```typescript
// Conceptual
export function generateMetadata({ params: { locale } }): Metadata {
  return {
    title: {
      default: 'Yen-Ho Chen | AI Agent Engineer',
      template: '%s | Yen-Ho Chen'
    },
    description: '...',
    alternates: {
      canonical: `https://thisisyenho.com/${locale}`,
      languages: {
        'zh-TW': '/zh',
        'en': '/en',
      }
    },
    openGraph: {
      type: 'website',
      locale: locale === 'zh' ? 'zh_TW' : 'en_US',
      // ...
    }
  }
}
```

#### Sitemap & Robots
- Auto-generated via `next-sitemap` in post-build step
- All locale variants included in sitemap
- Proper `robots.txt` configuration

### Performance Targets

| Metric | Target |
|---|---|
| Lighthouse Performance | 95+ |
| Lighthouse Accessibility | 100 |
| Lighthouse SEO | 100 |
| First Contentful Paint (FCP) | < 1.0s |
| Largest Contentful Paint (LCP) | < 2.0s |
| Cumulative Layout Shift (CLS) | < 0.05 |
| Total JS Bundle | < 100KB (first load) |

### Performance Strategies

1. **Static Generation (SSG):** All pages pre-rendered at build time
2. **Image Optimization:** Use `next/image` with AVIF/WebP formats
3. **Font Optimization:** Use `next/font` for zero-layout-shift font loading
4. **Minimal Client JS:** Leverage Server Components; only interactive components are Client Components
5. **Code Splitting:** Automatic via Next.js App Router
6. **Lazy Loading:** Defer below-fold content and animations
7. **No heavy frameworks:** Avoid large UI libraries; Tailwind CSS is utility-only (zero runtime)

---

## 9. Deployment Strategy

### Platform: Vercel (Free Tier)

**Why Vercel:**
- Native Next.js support (built by the same team)
- Automatic deployments from Git
- Edge network for fast global delivery
- Preview deployments for every branch/PR
- Free tier is generous for personal sites
- Built-in analytics (Web Vitals)

### Deployment Pipeline

```
GitHub Repository (thisisyenho)
    |
    v  (push to main)
Vercel Auto-Deploy
    |
    +-- Build: next build (SSG)
    +-- Output: Static HTML + Edge Functions (if needed)
    +-- Deploy to CDN
    |
    v
https://thisisyenho.com (custom domain)
```

### Domain Configuration

- **Primary domain:** `thisisyenho.com` (or similar, to be purchased)
- **Alternative:** `yenho.dev`, `yenhochen.com`
- DNS configured via Vercel or external registrar (Cloudflare, Namecheap)
- HTTPS automatically provisioned by Vercel

### Environment Variables

Minimal env vars needed (no backend/database):
- `NEXT_PUBLIC_SITE_URL` - for canonical URLs and OG images
- `NEXT_PUBLIC_GA_ID` - Google Analytics (optional)

### CI/CD Considerations

- **Linting check** on every push (ESLint + TypeScript type check)
- **Build verification** before deployment
- **Preview deployments** for branches (automatic on Vercel)
- Optional: **Lighthouse CI** via GitHub Actions for performance monitoring

---

## 10. Estimated File Structure

```
thisisyenho/
+-- content/                         # Content source files
|   +-- resume/
|   |   +-- zh.md                    # Chinese resume (primary data source)
|   |   +-- en.md                    # English resume
|   +-- projects/                    # Detailed project pages (optional)
|   |   +-- zh/
|   |   |   +-- ai-mentor.md
|   |   |   +-- livestream-agent.md
|   |   |   +-- news-clustering.md
|   |   |   +-- bytebert.md
|   |   +-- en/
|   |       +-- ai-mentor.md
|   |       +-- livestream-agent.md
|   |       +-- news-clustering.md
|   |       +-- bytebert.md
|   +-- images/                      # Project screenshots, profile photo
|       +-- profile.jpg
|       +-- projects/
|           +-- ai-mentor-hero.png
|           +-- ...
+-- messages/                        # i18n UI strings (next-intl)
|   +-- zh.json
|   +-- en.json
+-- public/                          # Static assets
|   +-- favicon.ico
|   +-- og-image.png                 # Default Open Graph image
|   +-- resume-yenho-chen.pdf        # Downloadable PDF resume
|   +-- robots.txt                   # (auto-generated by next-sitemap)
|   +-- sitemap.xml                  # (auto-generated by next-sitemap)
+-- src/
|   +-- app/
|   |   +-- [locale]/                # Locale-based routing (next-intl)
|   |   |   +-- layout.tsx           # LocaleLayout with providers
|   |   |   +-- page.tsx             # Home page
|   |   |   +-- about/
|   |   |   |   +-- page.tsx
|   |   |   +-- experience/
|   |   |   |   +-- page.tsx
|   |   |   +-- projects/
|   |   |   |   +-- page.tsx
|   |   |   |   +-- [slug]/
|   |   |   |       +-- page.tsx
|   |   |   +-- skills/
|   |   |   |   +-- page.tsx
|   |   |   +-- contact/
|   |   |       +-- page.tsx
|   |   +-- layout.tsx               # Root layout (html, body, fonts)
|   |   +-- globals.css              # Global styles + Tailwind directives
|   |   +-- not-found.tsx            # Custom 404 page
|   +-- components/
|   |   +-- ui/                      # Reusable UI primitives
|   |   |   +-- button.tsx
|   |   |   +-- card.tsx
|   |   |   +-- badge.tsx
|   |   |   +-- separator.tsx
|   |   |   +-- container.tsx
|   |   |   +-- section-heading.tsx
|   |   +-- layout/                  # Layout components
|   |   |   +-- header.tsx
|   |   |   +-- footer.tsx
|   |   |   +-- nav-links.tsx
|   |   |   +-- mobile-menu.tsx
|   |   |   +-- locale-switcher.tsx
|   |   |   +-- theme-toggle.tsx
|   |   +-- home/                    # Home page sections
|   |   |   +-- hero-section.tsx
|   |   |   +-- featured-projects.tsx
|   |   |   +-- skill-highlights.tsx
|   |   |   +-- cta-section.tsx
|   |   +-- about/
|   |   |   +-- bio-section.tsx
|   |   |   +-- education-timeline.tsx
|   |   |   +-- achievement-list.tsx
|   |   +-- experience/
|   |   |   +-- experience-timeline.tsx
|   |   |   +-- experience-card.tsx
|   |   +-- projects/
|   |   |   +-- project-grid.tsx
|   |   |   +-- project-card.tsx
|   |   +-- skills/
|   |   |   +-- skill-category.tsx
|   |   |   +-- skill-badge.tsx
|   |   +-- contact/
|   |   |   +-- contact-info.tsx
|   |   |   +-- social-links.tsx
|   |   +-- shared/                  # Cross-cutting components
|   |       +-- animated-section.tsx
|   |       +-- markdown-renderer.tsx
|   +-- lib/                         # Utility functions
|   |   +-- content.ts              # Markdown parsing & data access
|   |   +-- markdown.ts             # Markdown-to-HTML pipeline
|   |   +-- utils.ts                # General utilities (cn(), etc.)
|   +-- types/                       # TypeScript type definitions
|   |   +-- resume.ts               # Resume data types
|   |   +-- project.ts              # Project data types
|   +-- config/                      # Site configuration
|   |   +-- site.ts                 # Site metadata, social links
|   |   +-- navigation.ts          # Navigation structure
|   +-- i18n/                        # next-intl configuration
|       +-- routing.ts              # Locale routing config
|       +-- request.ts             # Server-side i18n request config
+-- .eslintrc.json
+-- .prettierrc
+-- next.config.ts                   # Next.js config (with next-intl plugin)
+-- next-sitemap.config.js           # Sitemap generation config
+-- tailwind.config.ts               # Tailwind configuration
+-- tsconfig.json
+-- package.json
+-- resume-zh.md                     # Original resume files (kept at root)
+-- resume-en.md
+-- CLAUDE.md
+-- README.md
+-- website-plan.md                  # This file
```

---

## 11. Design Inspiration & Reference Sites

The following portfolio sites represent strong examples of what this project should aspire to in terms of design, UX, and content presentation.

### 1. Brittany Chiang - brittanychiang.com

- **Why:** One of the most widely referenced developer portfolio sites. Clean single-page layout with subtle animations, sticky navigation, spotlight cursor effect, and excellent use of dark mode.
- **Takeaway:** Minimalist dark theme, clear section hierarchy, tasteful hover states. Good model for the overall aesthetic.

### 2. Lee Robinson - leerob.io

- **Why:** VP of Developer Experience at Vercel. Built with Next.js, uses MDX for blog content, excellent performance scores. Very clean, content-focused design.
- **Takeaway:** Demonstrates best practices for a Next.js personal site. Blog integration, clean typography, fast loading. Good reference for content-driven architecture.

### 3. Lilian Weng - lilianweng.github.io

- **Why:** AI researcher (OpenAI). Focused on in-depth technical content with excellent readability. Clean blog layout with strong typography.
- **Takeaway:** Good example of how an AI professional presents technical depth. Markdown-based content pipeline. Strong reference for future blog integration.

### 4. Shu Ding - shud.in

- **Why:** Developer at Vercel, creator of SWR and Nextra. Extremely minimal, fast, elegant personal site. Beautiful typography and spacing.
- **Takeaway:** Shows how far minimalism can go. Excellent performance. Good reference for a "less is more" approach.

### 5. Delba de Oliveira - delba.dev

- **Why:** Developer advocate at Vercel. Visually striking portfolio with creative animations, gradient effects, and interactive elements. Built with Next.js.
- **Takeaway:** Demonstrates how subtle animations and visual flair can elevate a portfolio without being distracting. Good reference for the hero section and page transitions.

### Additional Design References

- **Tailwind UI / Spotlight template** (tailwindui.com) - Premium portfolio template that showcases Tailwind's capability for developer portfolios
- **shadcn/ui** (ui.shadcn.com) - Not a portfolio but an excellent component reference for building consistent UI primitives with Tailwind

---

## 12. Phased Roadmap

### Phase 1: MVP (Target: 2-3 weeks)

The minimum viable portfolio that can go live.

**Scope:**
- [x] Complete website architecture plan (this document)
- [ ] Set up Next.js 15 project with TypeScript, Tailwind CSS, next-intl
- [ ] Fill in English resume content (`resume-en.md`)
- [ ] Implement Markdown parsing pipeline (`lib/content.ts`)
- [ ] Build core layout: Header, Footer, Navigation, LocaleSwitcher, ThemeToggle
- [ ] Build Home page with Hero section
- [ ] Build About page (bio, education, achievements)
- [ ] Build Experience page (timeline)
- [ ] Build Projects page (card grid)
- [ ] Build Skills page (categorized badges)
- [ ] Build Contact page (social links, email, resume download)
- [ ] Implement dark/light mode
- [ ] Implement zh/en locale switching
- [ ] Basic SEO setup (metadata, Open Graph)
- [ ] Deploy to Vercel
- [ ] Configure custom domain

**Deliverables:**
- Fully functional bilingual portfolio site
- 6 pages (Home, About, Experience, Projects, Skills, Contact)
- Dark/light mode
- Mobile responsive
- Deployed and accessible at custom domain

### Phase 2: Polish & Enhance (2-3 weeks after MVP)

- [ ] Add scroll animations (framer-motion)
- [ ] Add page transition animations
- [ ] Create individual project detail pages (`/projects/[slug]`)
- [ ] Add project screenshots and visual assets
- [ ] Generate Open Graph images (dynamic or static)
- [ ] Add JSON-LD structured data
- [ ] Set up next-sitemap for auto-generated sitemap
- [ ] Lighthouse optimization (target 95+ across all categories)
- [ ] Add resume PDF download functionality
- [ ] Improve mobile navigation (hamburger menu with animation)
- [ ] Add subtle interactive elements (cursor effects, etc.)

### Phase 3: Blog & Content (Future)

- [ ] Set up MDX pipeline for blog posts
- [ ] Build blog listing page with tags/categories
- [ ] Build blog post page with table of contents
- [ ] Add RSS feed
- [ ] Add reading time estimate
- [ ] Add social sharing buttons
- [ ] Content: Write first technical blog posts (AI Agent development, RAG, etc.)

### Phase 4: Advanced Features (Future)

- [ ] Interactive AI demos (e.g., chatbot powered by personal knowledge)
- [ ] Project gallery with filtering and search
- [ ] Guestbook or comments section
- [ ] Newsletter signup
- [ ] GitHub contribution graph or live stats
- [ ] Spotify / Now Playing widget
- [ ] View count / analytics dashboard
- [ ] CMS integration (if content management becomes cumbersome)

---

## 13. Content Strategy

### Resume Content Mapping

The following table maps resume sections to website pages:

| Resume Section | Website Page | Notes |
|---|---|---|
| Basic Info | Home (hero) + Contact | Name, title, links |
| Work Experience | Experience page | Full timeline with details |
| Education | About page | Education timeline subsection |
| Skills | Skills page + Home (highlights) | Categorized skill badges |
| Projects | Projects page | Card grid + detail pages |
| Achievements | About page | Achievement list subsection |
| Languages | About page | Brief mention |
| Soft Skills | About page | Personal traits section |
| Extracurriculars | About page | Brief mention if space allows |

### Content Priority for English Version

The English `resume-en.md` currently contains placeholder content. Before building the website, the English version should be completed with real content matching the Chinese version. This is a prerequisite for Phase 1.

### Content Enhancement Opportunities

Beyond the resume, the website can include:
- A professional headshot / avatar
- Project screenshots or demo GIFs
- A brief "currently working on" or "what I'm learning" section
- Links to relevant publications or talks
- Testimonials from colleagues (optional)

---

## Appendix: Key Decisions & Rationale

### Why Next.js App Router (not Pages Router)?
- App Router is the recommended architecture for new Next.js projects as of v13+
- Server Components reduce client-side JavaScript
- Better data fetching patterns (async components)
- Improved metadata API
- Parallel routes and intercepting routes for future needs
- next-intl has excellent App Router support

### Why next-intl (not next-i18next)?
- next-i18next was designed for Pages Router; its App Router support is secondary
- next-intl was built from the ground up for App Router and Server Components
- Simpler configuration, fewer moving parts
- Type-safe message access with TypeScript
- Built-in middleware for locale routing
- Active development with frequent releases

### Why Tailwind CSS (not CSS Modules or styled-components)?
- Zero runtime overhead (pure CSS at build time)
- Excellent developer experience with VS Code IntelliSense
- Consistent design tokens via configuration
- Dark mode support built-in (class strategy)
- Widely adopted in the Next.js ecosystem
- Pairs well with component-based architecture

### Why Markdown parsing (not a headless CMS)?
- Resume content is relatively static; no need for CMS complexity
- Markdown files are version-controlled alongside code
- No external service dependency or API calls
- Simple build-time processing
- Easy to edit with any text editor
- Future migration to CMS is straightforward if needed

### Why SSG (not SSR or ISR)?
- Portfolio content changes infrequently
- Static pages are the fastest to serve
- No per-request server cost
- Perfect for Vercel's CDN distribution
- Rebuilds triggered only when content changes (push to main)

---

*This plan was prepared on 2026-02-13. Technologies and version numbers should be verified against the latest stable releases at the time of implementation.*
