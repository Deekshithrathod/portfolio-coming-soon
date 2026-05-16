# Portfolio — Brainstorm & Spec

## Vision

A personal portfolio + content platform. Minimal, functional, no fluff. Covers projects, technical writing, opinionated rants, and a public ideas board. Available on web and mobile (iOS + Android).

---

## Content Sections

### 1. Projects

- **List view** — cards showing name, short description, status, tech stack tags
- **Detail page** — full writeup, why it was built, live demo (or placeholder skeleton if unavailable), tech stack, progress tracker
- **Progress tracking** — pulled live from GitHub issues on the linked repo (open vs. closed issue counts, maybe milestone progress)
- **Status tags** — `active` / `shipped` / `archived`
- **Categories** — by domain or tech stack (TBD when there are enough projects to warrant it)

### 2. Blog

- Long-form, detailed, technical writing
- Standard list + individual post pages
- Tags / categories for filtering
- Voting (see Voting section)

### 3. Rants — *"Did they even test this?"*

- Short-form posts — longer than a tweet, shorter than a blog
- Opinionated, informal tone
- Always includes: what the bug/issue is, why it's bad, possible fix/suggestion
- Voting (see Voting section)
- Organized as a series — all rants live under the "Did they even test this?" umbrella
- No user submissions

### 4. Ideas

- A public board of potential project/feature ideas
- Structured fields per idea:
  - `title`
  - `description`
  - `status` — `exploring` / `building` / `abandoned` / `shipped`
  - `tags`
- Voting (see Voting section) — community helps surface good ideas

### 5. About *(light, optional)*

- Short bio, what this site is, links (GitHub, etc.)

---

## Voting

- Available on: **projects**, **blog posts**, **rants**, **ideas**
- Anonymous — no login required
- Tracked per device via a generated anonymous device ID (stored locally)
- One vote per entity per device
- Vote count is public

---

## Tech Stack

### Monorepo Structure (Turborepo)

```
/
├── apps/
│   ├── web/          # Next.js
│   └── mobile/       # Expo (React Native — iOS & Android)
├── packages/
│   ├── tokens/       # Shared design tokens (colors, spacing, typography)
│   ├── types/        # Shared TypeScript types
│   └── notion/       # Shared Notion API client & data fetchers
```

### Web — `apps/web`

- **Framework** — Next.js (App Router)
- **Styling** — TBD (Tailwind CSS is the obvious pick for minimal/utility-first)
- **Hosting** — Vercel

### Mobile — `apps/mobile`

- **Framework** — Expo (React Native)
- **Styling** — NativeWind (Tailwind syntax for React Native, keeps it consistent with web)
- **Platforms** — iOS + Android
- **Purpose** — Same content as web, native experience, no exclusive features

### Content — MDX files in the repo

- **Blogs, Rants, Projects, Ideas** — written as `.mdx` files, living in the repo
- Git history is the changelog — no separate CMS or admin panel
- Frontmatter handles structured metadata (title, date, tags, status, GitHub repo URL, etc.)
- Rendered via a MDX pipeline in Next.js (e.g. `next-mdx-remote` or `@next/mdx`)
- Mobile app fetches pre-processed content from the Next.js API or a shared data layer

### Backend — Convex

Convex handles only the interactive/transactional data — nothing static:

- **Votes** — stored in Convex, one vote per entity per device
- **Device IDs** — anonymous ID generated on first visit, stored in `localStorage` (web) / `AsyncStorage` (mobile), registered in Convex
- **GitHub issue sync** — a Convex scheduled action polls the GitHub Issues API for linked repos and caches open/closed counts; avoids hitting GitHub rate limits on every page load

---

## Design Direction

- **Minimal** — no animations, no gradients, no fancy things until core is solid
- **Functional first** — get it working, then layer on polish
- **Consistency** — shared design tokens (`packages/tokens`) ensure web and mobile feel like the same product without sharing UI component code (React and React Native can't share JSX)

---

## Open Questions / Deferred Decisions

| # | Question | Notes |
|---|----------|-------|
| 1 | Tailwind on web — confirm? | Fits the minimal direction, pairs well with NativeWind on mobile |
| 2 | GitHub Progress — issues only, or also milestones + PRs? | Start with issues, extend later |
| 3 | Mobile navigation pattern — tab bar or drawer? | TBD |
| 4 | SEO / OG images for blog/rant posts? | Worth doing for web, defer until content exists |
| 5 | Mobile content strategy — fetch from Next.js API or duplicate MDX pipeline in Expo? | Fetching from a Next.js API route is simpler |

---

## Out of Scope (for now)

- User accounts / authentication
- Comments
- User-submitted content of any kind
- Push notifications
- Search (can add later)
- Dark mode (functionality first)
