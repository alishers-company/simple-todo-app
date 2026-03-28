# Claude AI Build Instructions

You are generating a complete **React + TypeScript + Vite** web application prototype based on client requirements in `PLAN.md`. Your output is pushed directly to GitHub and deployed to GitHub Pages — every file you write goes live automatically with no human review step.

> **PRIORITY:** Read `PLAN.md` first. Build exactly what the client described — nothing more, nothing less.

---

## What Already Exists (Do NOT write these files)

These files are pre-configured and correct. Overwriting them will break the build or deploy:

| File | Why it must stay |
|---|---|
| `vite.config.ts` | Sets `BASE_URL` for GitHub Pages sub-path routing |
| `package.json` | All dependencies pre-installed |
| `tsconfig.json` / `tsconfig.app.json` / `tsconfig.node.json` | TypeScript config for strict mode |
| `src/vite-env.d.ts` | Vite type declarations (`import.meta.env`) |
| `public/vite.svg` | Default icon |
| `.github/workflows/deploy-pages.yml` | GitHub Actions deploy pipeline |
| `.github/copilot-instructions.md` | These instructions |
| `src/main.tsx` | BrowserRouter with `basename={import.meta.env.BASE_URL}` already configured |

**You MAY update `index.html`** — change `<title>Prototype</title>` to match the project name from PLAN.md.
**You MAY update `src/main.tsx`** — but you MUST keep the `<BrowserRouter basename={import.meta.env.BASE_URL}>` wrapper intact.

---

## What You Must Write

The files below currently contain stubs. Write all of them completely:

```
src/
├── App.tsx               ← Root component with <Routes>
├── index.css             ← Global styles (keep @import "tailwindcss" as line 1)
├── pages/                ← One file per route from PLAN.md
├── components/
│   └── ui/               ← Button, Card, Input, Badge (reuse everywhere)
├── hooks/                ← Custom React hooks
├── lib/
│   └── mock-data.ts      ← All mock/seed data in one place
└── types/                ← Shared TypeScript interfaces
```

---

## ⚠️ Critical: React Router on GitHub Pages

The app is served from `https://{owner}.github.io/{repo-name}/` — a sub-path, not root.
You **MUST** pass `basename={import.meta.env.BASE_URL}` to `<BrowserRouter>` or all routes except `/` will 404:

```tsx
// src/main.tsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import './index.css'
import App from './App.tsx'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <BrowserRouter basename={import.meta.env.BASE_URL}>
      <App />
    </BrowserRouter>
  </StrictMode>,
)
```

---

## Tech Stack (pre-installed — do NOT add packages)

- **React 19** with TypeScript strict mode
- **Vite 6** — build tool
- **Tailwind CSS 4** — `@import "tailwindcss"` already in `index.css`
- **React Router DOM v7** — for multi-page routing

---

## PLAN.md Sections → What to Build

| Section | Drives |
|---|---|
| **Overview** | App purpose and tone — influences layout, copy, color mood |
| **Pages** | One `src/pages/*.tsx` file + one `<Route>` per page listed |
| **Features** | Interactive behaviors — implement with `useState`/`useReducer`/hooks |
| **Design Preferences** | Override the default color palette below with client's brand |
| **Data & Auth** | What data the app uses and how users authenticate — use mock data unless PLAN.md says otherwise. If auth is mentioned, build a realistic login UI with mock validation |
| **Integrations** | External services — build UI that looks connected but uses mock data. Show realistic API response shapes in `src/lib/mock-data.ts` |

---

## Design System

Apply these consistently across ALL pages. Override only when PLAN.md specifies brand colors.

### Color Palette

```
Primary action:   bg-blue-600 hover:bg-blue-700 text-white
Secondary action: bg-gray-100 hover:bg-gray-200 text-gray-800
Destructive:      bg-red-600 hover:bg-red-700 text-white
Success:          bg-green-600 text-white
Page background:  bg-gray-50
Card background:  bg-white
Headings:         text-gray-900
Body text:        text-gray-600
Muted text:       text-gray-400
Borders:          border-gray-200
```

### Spacing & Layout

```
Page:    px-4 sm:px-6 lg:px-8  with  max-w-7xl mx-auto
Cards:   p-4 sm:p-6
Gaps:    space-y-6  or  gap-6
Radius:  rounded-lg (cards)  rounded-md (buttons/inputs)
```

### Typography

```
Page title:      text-2xl sm:text-3xl font-bold text-gray-900
Section heading: text-xl font-semibold text-gray-900
Body:            text-base text-gray-600
Caption/small:   text-sm text-gray-400
```

---

## Component Rules

- One component per file, named export matching filename
- Functional components with hooks only — no class components
- Every prop must have a TypeScript `interface` (e.g., `interface CardProps { ... }`)
- Max 120 lines per component — extract sub-components when larger

### Required base UI components (build in `src/components/ui/`, reuse everywhere)

| Component | Variants |
|---|---|
| `Button` | `primary`, `secondary`, `destructive`, `ghost` |
| `Card` | optional `header` and `footer` slots |
| `Input` | `label` prop, error state with red border + message |
| `Badge` | for status indicators |

---

## State Management

- Every data-driven section needs three states: **loading** → **empty** → **populated**
- Use `useState` for local state, `useReducer` for complex state
- Use React Context only when 3+ components share the same state
- All mock data in `src/lib/mock-data.ts` — use realistic names, prices, dates (no Lorem ipsum)

---

## Interaction & Polish

- All clickable elements: `cursor-pointer` + visible hover/focus states
- Buttons: `focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2`
- Transitions: `transition-colors duration-150` on interactive elements
- Form inputs: red border + error message below on validation failure
- Navigation: highlight active route in navbar

---

## Accessibility

- All images: descriptive `alt` text
- Icon-only buttons: `aria-label`
- All interactive elements: keyboard-focusable via `<button>` or `tabIndex`
- No light gray text on white backgrounds (fails contrast)

---

## Rules

### Must Follow

1. **Read PLAN.md first** — build exactly what is described
2. **TypeScript only** — no `.js` or `.jsx` files. Define interfaces for all props
3. **Tailwind CSS only** — no inline styles, no CSS modules
4. **Zero build errors** — TypeScript strict mode is on; verify mentally before finishing
5. **Semantic HTML** — `<main>`, `<nav>`, `<section>`, `<header>`, `<footer>`; `alt` on all images
6. **Mobile-first** — design for 375px first, then `sm:` `md:` `lg:` breakpoints
7. **React Router** — always use `basename={import.meta.env.BASE_URL}` (see critical note above)

### Must NOT Do

- Do NOT install npm packages — everything needed is already installed
- Do NOT add auth flows unless PLAN.md explicitly asks for them
- Do NOT create a backend or API server
- Do NOT modify `vite.config.ts`, `.github/workflows/deploy-pages.yml`, or any `tsconfig*.json`
- Do NOT remove `@import "tailwindcss"` from `index.css`
- Do NOT use `any` type — define proper types for everything
- Do NOT leave TODO comments or stub implementations — complete everything fully
- Do NOT use `https://via.placeholder.com` — use `https://placehold.co/{width}x{height}` instead

---

## Completion Checklist

Before finishing, verify ALL of these:

- [ ] `src/main.tsx` uses `<BrowserRouter basename={import.meta.env.BASE_URL}>`
- [ ] Every page in PLAN.md has a corresponding route and `src/pages/*.tsx` file
- [ ] Every feature in PLAN.md is implemented (nothing stubbed)
- [ ] Navigation works between all pages; active route is highlighted
- [ ] `index.html` `<title>` updated to the project name from PLAN.md
- [ ] App looks correct on mobile (375px) and desktop (1280px)
- [ ] No TypeScript errors — all props typed, no `any`
- [ ] All mock data is realistic (real names, plausible prices/dates)
- [ ] Loading and empty states exist for all data-driven sections
- [ ] All buttons and links have hover + focus states
- [ ] All placeholder images use `https://placehold.co/{width}x{height}` format
