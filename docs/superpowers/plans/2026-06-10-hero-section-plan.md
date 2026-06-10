# Hero Section Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the Transfez Homepage Hero section with sticky Navbar (from Figma) and placeholder Footer, on a fresh Next.js + Tailwind + Typefez foundation.

**Architecture:** Next.js 14 App Router with TypeScript strict mode. Component decomposition follows SOLID (1 file = 1 job). All copy & dummy data lives in `lib/constants.ts` (OCP). Calculator card is static visual (no API/state). Navbar matches Figma; Footer is placeholder.

**Tech Stack:** Next.js 14 · TypeScript · Tailwind CSS v3.4 · lucide-react · Typefez OTF (local font) · clsx + tailwind-merge

**Spec:** [`docs/superpowers/specs/2026-06-10-hero-section-design.md`](../specs/2026-06-10-hero-section-design.md)

**Visual reference:** [`assets/figma-exports/hero-section.png`](../../../assets/figma-exports/hero-section.png), [`assets/figma-exports/navbar.png`](../../../assets/figma-exports/navbar.png)

**Testing approach:** TypeScript strict mode + `npm run build` as the primary correctness gate. Unit test only for `Button` (the one component with branching logic via variants). Static presentation components verified via manual visual QA against Figma. Final Lighthouse + 3-breakpoint responsive QA before completion.

**Project root:** `/Users/jacktfz/Downloads/website-transfez/landing-page/`
**Repo root:** `/Users/jacktfz/Downloads/website-transfez/` (already git-initialized, tracking `origin/main`)

---

## Phase 0 — Pre-flight

### Task 1: Verify environment

**Files:** none

- [ ] **Step 1: Check Node version**

Run: `node -v`
Expected: `v18.17.0` or higher (Next.js 14 requirement). If lower, run `nvm install 20 && nvm use 20`.

- [ ] **Step 2: Check npm version**

Run: `npm -v`
Expected: `9.x` or higher.

- [ ] **Step 3: Check git status from repo root**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git status
git log --oneline -1
```
Expected: `On branch main`, working tree clean, last commit is `chore: initial commit — docs & Hero section spec`.

- [ ] **Step 4: Verify Figma asset files present**

```bash
ls assets/figma-exports/
```
Expected output:
```
hero-section.png
navbar.png
transfez-logo-swoosh.svg
transfez-logo-text.svg
```

---

## Phase 1 — Project Foundation

### Task 2: Scaffold Next.js project

**Files:**
- Create: `landing-page/` (entire Next.js scaffold)

- [ ] **Step 1: Scaffold with create-next-app (Next 14, no Tailwind — we install v3.4 manually next)**

Run from repo root:
```bash
cd /Users/jacktfz/Downloads/website-transfez
npx create-next-app@14 landing-page \
  --typescript \
  --eslint \
  --app \
  --src-dir \
  --import-alias "@/*" \
  --no-tailwind \
  --use-npm
```

When prompted "Would you like to use Turbopack for next dev?", answer **No** (Netlify build compatibility is more stable with webpack).

Expected: `landing-page/` folder created with `src/app/`, `package.json`, `tsconfig.json`, `next.config.js`, `eslint.config.*`.

- [ ] **Step 2: Verify scaffold runs**

```bash
cd landing-page
npm run dev
```
Open `http://localhost:3000` → should see default Next.js welcome page. Stop server with `Ctrl+C`.

- [ ] **Step 3: Verify TypeScript strict mode**

Read `landing-page/tsconfig.json` and confirm `"strict": true` exists in `compilerOptions`. If not, add it.

- [ ] **Step 4: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page
git commit -m "chore: scaffold Next.js 14 project (TypeScript + App Router + src dir)"
```

---

### Task 3: Install Tailwind CSS v3.4 + plugins

**Files:**
- Create: `landing-page/tailwind.config.ts`
- Create: `landing-page/postcss.config.js`
- Modify: `landing-page/package.json` (auto via npm install)

- [ ] **Step 1: Install Tailwind v3.4 + PostCSS + Autoprefixer**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm install -D tailwindcss@3.4 postcss autoprefixer
```

- [ ] **Step 2: Verify installed version**

```bash
npm ls tailwindcss
```
Expected: `tailwindcss@3.4.x` (NOT 4.x).

- [ ] **Step 3: Initialize Tailwind config files**

```bash
npx tailwindcss init -p
```
Creates `tailwind.config.js` and `postcss.config.js`.

- [ ] **Step 4: Rename to TypeScript config**

```bash
mv tailwind.config.js tailwind.config.ts
```

- [ ] **Step 5: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/package.json landing-page/package-lock.json landing-page/tailwind.config.ts landing-page/postcss.config.js
git commit -m "chore: install Tailwind CSS v3.4 + PostCSS"
```

---

### Task 4: Install remaining dependencies

**Files:**
- Modify: `landing-page/package.json` (auto)

- [ ] **Step 1: Install runtime deps**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm install lucide-react clsx tailwind-merge
```

- [ ] **Step 2: Verify installs**

```bash
npm ls lucide-react clsx tailwind-merge
```
Expected: all three listed with versions.

- [ ] **Step 3: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/package.json landing-page/package-lock.json
git commit -m "chore: add lucide-react, clsx, tailwind-merge"
```

---

### Task 5: Configure Tailwind tokens (colors, font, custom sizes)

**Files:**
- Modify: `landing-page/tailwind.config.ts`

- [ ] **Step 1: Overwrite tailwind.config.ts with brand tokens**

Replace entire file content with:

```ts
import type { Config } from 'tailwindcss'

const config: Config = {
  content: [
    './src/app/**/*.{js,ts,jsx,tsx,mdx}',
    './src/components/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      colors: {
        pinkDA:      'rgb(218 100 159)',
        pinkLight:   'rgb(253 215 245)',
        pinkDark:    'rgb(182 83 132)',
        blue10:      'rgb(16 44 87)',
        blue2C:      'rgb(44 74 119)',
        green06:     'rgb(6 172 78)',
        greenSubtle: 'rgb(218 245 224)',
        dark:        'rgb(37 37 37)',
        grey6C:      'rgb(108 108 113)',
        grey90:      'rgb(144 144 152)',
        whiteE6:     'rgb(230 230 232)',
        whiteF1:     'rgb(241 241 241)',
      },
      fontFamily: {
        sans: ['var(--font-typefez)', 'Arial', 'sans-serif'],
      },
      fontSize: {
        '6xl': ['56px', { lineHeight: '1.25', fontWeight: '700' }],
      },
      boxShadow: {
        card: '0 8px 16px -4px rgba(22, 34, 51, 0.08)',
      },
      backgroundImage: {
        'brand-gradient': 'linear-gradient(-213.5deg, #FDD7F5 3.88%, #DA649F 74.38%)',
      },
    },
  },
  plugins: [],
}

export default config
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/tailwind.config.ts
git commit -m "feat(theme): configure Tailwind with Transfez brand tokens"
```

---

### Task 6: Setup globals.css with Tailwind directives + CSS variables

**Files:**
- Modify: `landing-page/src/app/globals.css`

- [ ] **Step 1: Overwrite globals.css**

Replace entire file content with:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --background: #ffffff;
  --foreground: #252525;
  --primary: 218 100 159;
  --primary-foreground: 255 255 255;
  --secondary: 16 44 87;
  --secondary-foreground: 255 255 255;
  --border: 230 230 232;
  --input: 230 230 232;
  --ring: 218 100 159;
  --destructive: 231 49 38;
}

html {
  scroll-behavior: smooth;
}

body {
  background: var(--background);
  color: var(--foreground);
  font-family: var(--font-typefez), Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/app/globals.css
git commit -m "feat(theme): setup globals.css with Tailwind + CSS vars"
```

---

### Task 7: Create `cn()` utility

**Files:**
- Create: `landing-page/src/lib/utils.ts`

- [ ] **Step 1: Create utils.ts**

```ts
import { clsx, type ClassValue } from 'clsx'
import { twMerge } from 'tailwind-merge'

export function cn(...inputs: ClassValue[]): string {
  return twMerge(clsx(inputs))
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/lib/utils.ts
git commit -m "feat(utils): add cn() helper for class merging"
```

---

### Task 8: Copy Typefez fonts + setup in layout.tsx

**Files:**
- Create: `landing-page/public/fonts/TypeFezStd-{Regular,Medium,SemiBold,Bold,ExtraBold}.otf` (5 files)
- Modify: `landing-page/src/app/layout.tsx`

- [ ] **Step 1: Copy 5 font weights from system to project**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
mkdir -p public/fonts
cp ~/Library/Fonts/TypeFezStd-Regular.otf   public/fonts/
cp ~/Library/Fonts/TypeFezStd-Medium.otf    public/fonts/
cp ~/Library/Fonts/TypeFezStd-SemiBold.otf  public/fonts/
cp ~/Library/Fonts/TypeFezStd-Bold.otf      public/fonts/
cp ~/Library/Fonts/TypeFezStd-ExtraBold.otf public/fonts/
ls -lh public/fonts/
```
Expected: 5 .otf files, ~54KB each.

- [ ] **Step 2: Overwrite layout.tsx**

```tsx
import type { Metadata } from 'next'
import localFont from 'next/font/local'
import './globals.css'

const typefez = localFont({
  src: [
    { path: '../../public/fonts/TypeFezStd-Regular.otf',   weight: '400', style: 'normal' },
    { path: '../../public/fonts/TypeFezStd-Medium.otf',    weight: '500', style: 'normal' },
    { path: '../../public/fonts/TypeFezStd-SemiBold.otf',  weight: '600', style: 'normal' },
    { path: '../../public/fonts/TypeFezStd-Bold.otf',      weight: '700', style: 'normal' },
    { path: '../../public/fonts/TypeFezStd-ExtraBold.otf', weight: '800', style: 'normal' },
  ],
  variable: '--font-typefez',
  display: 'swap',
  fallback: ['Arial', 'sans-serif'],
})

export const metadata: Metadata = {
  title: 'Transfez — Fast international money transfers',
  description:
    'Send money from Indonesia to anywhere in the world with low fees, real exchange rates, and arrival times measured in minutes — not days.',
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={typefez.variable}>
      <body className="font-sans">{children}</body>
    </html>
  )
}
```

- [ ] **Step 3: Run dev server, verify font loads**

```bash
npm run dev
```
Open `http://localhost:3000`. Open DevTools → Network → filter "font". Reload. Verify all 5 OTF files load with status 200.

Stop server with `Ctrl+C`.

- [ ] **Step 4: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/public/fonts/ landing-page/src/app/layout.tsx
git commit -m "feat(font): setup Typefez local font (5 weights)"
```

---

## Phase 2 — Static Assets

### Task 9: Add Indonesia and Thailand flag SVGs

**Files:**
- Create: `landing-page/public/flags/id.svg`
- Create: `landing-page/public/flags/th.svg`

- [ ] **Step 1: Create flags directory**

```bash
mkdir -p /Users/jacktfz/Downloads/website-transfez/landing-page/public/flags
```

- [ ] **Step 2: Create id.svg**

Path: `landing-page/public/flags/id.svg`

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 28 28" width="28" height="28">
  <defs>
    <clipPath id="circle-id">
      <circle cx="14" cy="14" r="14"/>
    </clipPath>
  </defs>
  <g clip-path="url(#circle-id)">
    <rect width="28" height="14" fill="#FF0000"/>
    <rect y="14" width="28" height="14" fill="#FFFFFF"/>
  </g>
</svg>
```

- [ ] **Step 3: Create th.svg**

Path: `landing-page/public/flags/th.svg`

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 28 28" width="28" height="28">
  <defs>
    <clipPath id="circle-th">
      <circle cx="14" cy="14" r="14"/>
    </clipPath>
  </defs>
  <g clip-path="url(#circle-th)">
    <rect width="28" height="4.667" fill="#A51931"/>
    <rect y="4.667" width="28" height="4.666" fill="#F4F5F8"/>
    <rect y="9.333" width="28" height="9.334" fill="#2D2A4A"/>
    <rect y="18.667" width="28" height="4.666" fill="#F4F5F8"/>
    <rect y="23.333" width="28" height="4.667" fill="#A51931"/>
  </g>
</svg>
```

- [ ] **Step 4: Verify flags render**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm run dev
```
Open `http://localhost:3000/flags/id.svg` and `http://localhost:3000/flags/th.svg` directly in browser. Both should display as circular flags.

Stop server.

- [ ] **Step 5: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/public/flags/
git commit -m "feat(assets): add Indonesia and Thailand circular flag SVGs"
```

---

### Task 10: Copy Transfez logo SVGs from repo assets

**Files:**
- Create: `landing-page/public/transfez-logo-text.svg`
- Create: `landing-page/public/transfez-logo-swoosh.svg`

- [ ] **Step 1: Copy from repo assets/figma-exports to project public/**

```bash
cd /Users/jacktfz/Downloads/website-transfez
cp assets/figma-exports/transfez-logo-text.svg   landing-page/public/transfez-logo-text.svg
cp assets/figma-exports/transfez-logo-swoosh.svg landing-page/public/transfez-logo-swoosh.svg
ls -lh landing-page/public/transfez-logo-*.svg
```
Expected: 2 SVG files, ~39KB (text) + ~1KB (swoosh).

- [ ] **Step 2: Verify logos render**

```bash
cd landing-page && npm run dev
```
Open `http://localhost:3000/transfez-logo-text.svg` and `http://localhost:3000/transfez-logo-swoosh.svg`. Both render correctly.

Stop server.

- [ ] **Step 3: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/public/transfez-logo-text.svg landing-page/public/transfez-logo-swoosh.svg
git commit -m "feat(assets): add Transfez logo SVGs (text + swoosh)"
```

---

## Phase 3 — Types & Data

### Task 11: Create shared TypeScript types

**Files:**
- Create: `landing-page/src/types/index.ts`

- [ ] **Step 1: Create types file**

```ts
import type { LucideIcon } from 'lucide-react'

export interface Currency {
  code: string
  flagSrc: string
  alt: string
}

export interface CurrencyInput {
  label: string
  currency: Currency
  amount: string
  showDropdown?: boolean
}

export interface FeeDetail {
  icon: LucideIcon
  label: string
  value: string
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/types/index.ts
git commit -m "feat(types): add shared Currency, CurrencyInput, FeeDetail types"
```

---

### Task 12: Create constants for nav, hero, calculator, footer

**Files:**
- Create: `landing-page/src/lib/constants.ts`

- [ ] **Step 1: Create constants file**

```ts
import { Minus, X } from 'lucide-react'
import type { FeeDetail } from '@/types'

export const NAV_LINKS = [
  { label: 'International Transfer', hasDropdown: true,  href: '#' },
  { label: 'Others',                 hasDropdown: true,  href: '#' },
  { label: 'Promo',                  hasDropdown: false, href: '#' },
] as const

export const NAVBAR_LANGUAGE = {
  active: 'ID' as const,
  options: ['ID', 'EN'] as const,
}

export const NAVBAR_CTA = {
  label: 'Send from Indonesia',
  flagSrc: '/flags/id.svg',
  flagAlt: 'Indonesia',
  href: '#',
} as const

export const HERO_CONTENT = {
  headline: {
    prefix: 'Fast, easy international money transfers to ',
    highlight: '70+ countries',
  },
  subheading:
    'Send money from Indonesia to anywhere in the world with low fees, real exchange rates, and arrival times measured in minutes — not days.',
  primaryCta:   { label: 'Send money now',  href: '#' },
  secondaryCta: { label: 'See live rates', href: '#' },
} as const

export const HERO_CALCULATOR = {
  title: 'Send money',
  liveRateLabel: 'Live rate',
  sendInput: {
    label: 'You send',
    currency: { code: 'IDR', flagSrc: '/flags/id.svg', alt: 'Indonesia' },
    amount: '20,000,000',
  },
  receiveInput: {
    label: 'Recipient gets',
    currency: { code: 'THB', flagSrc: '/flags/th.svg', alt: 'Thailand' },
    amount: '1,440.00',
    showDropdown: false,
  },
  fees: [
    { icon: Minus, label: 'Transfez fee',   value: 'SGD 5.00' },
    { icon: X,     label: 'Exchange rate', value: '1 SGD = IDR 12,743.39' },
  ] satisfies FeeDetail[],
  badge: { prefix: 'Money arrives in', highlight: 'Real-time' },
} as const

export const FOOTER_LINKS = {
  brand: { name: 'Transfez', tagline: 'Send money fast and reliably worldwide' },
  columns: [
    {
      title: 'Product',
      links: [
        { label: 'Send money', href: '#' },
        { label: 'Rates',      href: '#' },
        { label: 'Pricing',    href: '#' },
      ],
    },
    {
      title: 'Company',
      links: [
        { label: 'About us', href: '#' },
        { label: 'Careers',  href: '#' },
        { label: 'Blog',     href: '#' },
      ],
    },
    {
      title: 'Support',
      links: [
        { label: 'Help center', href: '#' },
        { label: 'Contact',     href: '#' },
        { label: 'FAQ',         href: '#' },
      ],
    },
  ],
  social: [
    { name: 'Instagram', href: '#' },
    { name: 'Twitter',   href: '#' },
    { name: 'LinkedIn',  href: '#' },
    { name: 'YouTube',   href: '#' },
  ],
  copyright: '© 2026 Transfez. All rights reserved.',
} as const
```

- [ ] **Step 2: Verify TypeScript compiles**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npx tsc --noEmit
```
Expected: no errors.

- [ ] **Step 3: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/lib/constants.ts
git commit -m "feat(data): add nav, hero, calculator, footer constants"
```

---

## Phase 4 — UI Primitives

### Task 13: Create `<Container>` layout wrapper

**Files:**
- Create: `landing-page/src/components/layout/Container.tsx`

- [ ] **Step 1: Create Container component**

```tsx
import { cn } from '@/lib/utils'

interface ContainerProps {
  children: React.ReactNode
  className?: string
}

export function Container({ children, className }: ContainerProps) {
  return (
    <div className={cn('mx-auto w-full max-w-7xl px-4 sm:px-6 lg:px-8', className)}>
      {children}
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/layout/Container.tsx
git commit -m "feat(layout): add Container wrapper (max-w-7xl)"
```

---

### Task 14: Create `<Button>` with primary + outline variants

**Files:**
- Create: `landing-page/src/components/ui/Button.tsx`
- Create: `landing-page/src/components/ui/Button.test.tsx`
- Modify: `landing-page/package.json` (test deps)
- Modify: `landing-page/vitest.config.ts` (new)

- [ ] **Step 1: Install Vitest + React Testing Library**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm install -D vitest @vitejs/plugin-react @testing-library/react @testing-library/jest-dom jsdom
```

- [ ] **Step 2: Create vitest.config.ts**

Path: `landing-page/vitest.config.ts`

```ts
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: ['./vitest.setup.ts'],
  },
  resolve: {
    alias: { '@': path.resolve(__dirname, './src') },
  },
})
```

- [ ] **Step 3: Create vitest.setup.ts**

Path: `landing-page/vitest.setup.ts`

```ts
import '@testing-library/jest-dom/vitest'
```

- [ ] **Step 4: Add test script to package.json**

In `landing-page/package.json`, find the `"scripts"` block and add `"test": "vitest"` after the existing `"lint"` line. Example:

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint",
  "test": "vitest"
}
```

- [ ] **Step 5: Write the failing test first**

Path: `landing-page/src/components/ui/Button.test.tsx`

```tsx
import { describe, it, expect } from 'vitest'
import { render, screen } from '@testing-library/react'
import { Button } from './Button'

describe('Button', () => {
  it('renders label text', () => {
    render(<Button variant="primary">Click me</Button>)
    expect(screen.getByRole('button', { name: 'Click me' })).toBeInTheDocument()
  })

  it('applies primary variant classes', () => {
    render(<Button variant="primary">P</Button>)
    const btn = screen.getByRole('button')
    expect(btn.className).toMatch(/bg-pinkDA/)
    expect(btn.className).toMatch(/text-white/)
  })

  it('applies outline variant classes', () => {
    render(<Button variant="outline">O</Button>)
    const btn = screen.getByRole('button')
    expect(btn.className).toMatch(/border/)
    expect(btn.className).toMatch(/text-pinkDA/)
  })

  it('forwards additional props', () => {
    render(<Button variant="primary" disabled>X</Button>)
    expect(screen.getByRole('button')).toBeDisabled()
  })
})
```

- [ ] **Step 6: Run test, verify it FAILS**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm test -- --run Button.test
```
Expected: FAIL with "Failed to resolve import './Button'" or similar.

- [ ] **Step 7: Implement Button**

Path: `landing-page/src/components/ui/Button.tsx`

```tsx
import type { ButtonHTMLAttributes } from 'react'
import { cn } from '@/lib/utils'

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant: 'primary' | 'outline'
  size?: 'md' | 'lg'
}

const SIZE_CLASSES: Record<NonNullable<ButtonProps['size']>, string> = {
  md: 'h-10 px-5 text-sm',
  lg: 'h-[50px] px-6 text-base',
}

const VARIANT_CLASSES: Record<ButtonProps['variant'], string> = {
  primary:
    'bg-pinkDA text-white hover:bg-pinkDark active:bg-pinkDark focus-visible:ring-2 focus-visible:ring-pinkDA/40',
  outline:
    'border border-pinkDA text-pinkDA bg-white hover:bg-pinkDA/5 active:bg-pinkDA/10 focus-visible:ring-2 focus-visible:ring-pinkDA/40',
}

export function Button({
  variant,
  size = 'lg',
  className,
  children,
  ...props
}: ButtonProps) {
  return (
    <button
      type={props.type ?? 'button'}
      className={cn(
        'inline-flex items-center justify-center rounded-full font-semibold transition-colors',
        'disabled:opacity-50 disabled:cursor-not-allowed',
        'focus-visible:outline-none',
        SIZE_CLASSES[size],
        VARIANT_CLASSES[variant],
        className,
      )}
      {...props}
    >
      {children}
    </button>
  )
}
```

- [ ] **Step 8: Run test, verify all PASS**

```bash
npm test -- --run Button.test
```
Expected: 4 tests pass.

- [ ] **Step 9: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/ui/Button.tsx \
        landing-page/src/components/ui/Button.test.tsx \
        landing-page/vitest.config.ts \
        landing-page/vitest.setup.ts \
        landing-page/package.json \
        landing-page/package-lock.json
git commit -m "feat(ui): add Button with primary/outline variants + Vitest"
```

---

### Task 15: Create `<CountryFlag>` wrapper

**Files:**
- Create: `landing-page/src/components/ui/CountryFlag.tsx`

- [ ] **Step 1: Create CountryFlag**

```tsx
/* eslint-disable @next/next/no-img-element */
import { cn } from '@/lib/utils'

interface CountryFlagProps {
  src: string
  alt: string
  size?: number
  className?: string
}

export function CountryFlag({ src, alt, size = 28, className }: CountryFlagProps) {
  return (
    <img
      src={src}
      alt={alt}
      width={size}
      height={size}
      className={cn('inline-block flex-shrink-0 rounded-full', className)}
      loading="lazy"
      decoding="async"
    />
  )
}
```

Note: using `<img>` instead of `<Image>` because SVGs don't need next/image optimization and we want full control over circular clip rendering. eslint-disable is intentional.

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/ui/CountryFlag.tsx
git commit -m "feat(ui): add CountryFlag wrapper for SVG flags"
```

---

## Phase 5 — Hero Section (bottom-up)

### Task 16: Create `<RealtimeBadge>`

**Files:**
- Create: `landing-page/src/components/sections/hero/RealtimeBadge.tsx`

- [ ] **Step 1: Create RealtimeBadge**

```tsx
import { Sparkles } from 'lucide-react'

interface RealtimeBadgeProps {
  prefix: string
  highlight: string
}

export function RealtimeBadge({ prefix, highlight }: RealtimeBadgeProps) {
  return (
    <div className="inline-flex items-center gap-2 rounded-full bg-whiteF1 px-3 py-1.5 text-sm">
      <Sparkles className="h-4 w-4 text-pinkDA" aria-hidden="true" />
      <span className="text-grey6C">
        {prefix}{' '}
        <span className="font-semibold text-pinkDA">{highlight}</span>
      </span>
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/sections/hero/RealtimeBadge.tsx
git commit -m "feat(hero): add RealtimeBadge component"
```

---

### Task 17: Create `<FeeDetailRow>`

**Files:**
- Create: `landing-page/src/components/sections/hero/FeeDetailRow.tsx`

- [ ] **Step 1: Create FeeDetailRow**

```tsx
import type { LucideIcon } from 'lucide-react'

interface FeeDetailRowProps {
  icon: LucideIcon
  label: string
  value: string
}

export function FeeDetailRow({ icon: Icon, label, value }: FeeDetailRowProps) {
  return (
    <div className="flex items-center justify-between text-sm">
      <div className="flex items-center gap-2 text-grey6C">
        <Icon className="h-5 w-5" aria-hidden="true" />
        <span>{label}</span>
      </div>
      <span className="font-medium text-dark">{value}</span>
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/sections/hero/FeeDetailRow.tsx
git commit -m "feat(hero): add FeeDetailRow component"
```

---

### Task 18: Create `<CurrencyInputRow>`

**Files:**
- Create: `landing-page/src/components/sections/hero/CurrencyInputRow.tsx`

- [ ] **Step 1: Create CurrencyInputRow**

```tsx
import { ChevronDown } from 'lucide-react'
import { CountryFlag } from '@/components/ui/CountryFlag'
import type { Currency } from '@/types'

interface CurrencyInputRowProps {
  label: string
  currency: Currency
  amount: string
  showDropdown?: boolean
}

export function CurrencyInputRow({
  label,
  currency,
  amount,
  showDropdown = true,
}: CurrencyInputRowProps) {
  return (
    <div className="rounded-lg border border-whiteE6 bg-white p-3">
      <label className="block text-sm text-grey90">{label}</label>
      <div className="mt-2 flex items-center justify-between">
        <div className="flex items-center gap-2">
          <CountryFlag src={currency.flagSrc} alt={currency.alt} size={28} />
          <span className="text-base font-semibold text-dark">{currency.code}</span>
          {showDropdown && (
            <ChevronDown className="h-5 w-5 text-grey90" aria-hidden="true" />
          )}
        </div>
        <span className="text-lg font-semibold text-dark tabular-nums">{amount}</span>
      </div>
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/sections/hero/CurrencyInputRow.tsx
git commit -m "feat(hero): add CurrencyInputRow component"
```

---

### Task 19: Create `<CalculatorCard>`

**Files:**
- Create: `landing-page/src/components/sections/hero/CalculatorCard.tsx`

- [ ] **Step 1: Create CalculatorCard**

```tsx
import type { CurrencyInput, FeeDetail } from '@/types'
import { CurrencyInputRow } from './CurrencyInputRow'
import { FeeDetailRow } from './FeeDetailRow'
import { RealtimeBadge } from './RealtimeBadge'

interface CalculatorCardProps {
  title: string
  liveRateLabel: string
  sendInput: CurrencyInput
  receiveInput: CurrencyInput
  fees: readonly FeeDetail[]
  badge: { prefix: string; highlight: string }
}

export function CalculatorCard({
  title,
  liveRateLabel,
  sendInput,
  receiveInput,
  fees,
  badge,
}: CalculatorCardProps) {
  return (
    <div className="relative w-full max-w-[488px]">
      <div className="relative z-10 rounded-2xl border border-whiteE6 bg-white p-6 shadow-card">
        <div className="flex items-center justify-between">
          <h3 className="text-base font-semibold text-dark">{title}</h3>
          <div className="flex items-center gap-1.5 text-sm text-green06">
            <span className="inline-block h-1.5 w-1.5 rounded-full bg-green06" aria-hidden="true" />
            <span>{liveRateLabel}</span>
          </div>
        </div>

        <div className="mt-5">
          <CurrencyInputRow
            label={sendInput.label}
            currency={sendInput.currency}
            amount={sendInput.amount}
            showDropdown={sendInput.showDropdown}
          />
        </div>

        <div className="mt-3 space-y-2 rounded-lg bg-whiteF1 px-4 py-3">
          {fees.map((fee) => (
            <FeeDetailRow
              key={fee.label}
              icon={fee.icon}
              label={fee.label}
              value={fee.value}
            />
          ))}
        </div>

        <div className="mt-3">
          <CurrencyInputRow
            label={receiveInput.label}
            currency={receiveInput.currency}
            amount={receiveInput.amount}
            showDropdown={receiveInput.showDropdown}
          />
        </div>

        <div className="mt-4 flex justify-center">
          <RealtimeBadge prefix={badge.prefix} highlight={badge.highlight} />
        </div>
      </div>
    </div>
  )
}
```

- [ ] **Step 2: Verify TypeScript compiles**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npx tsc --noEmit
```
Expected: no errors.

- [ ] **Step 3: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/sections/hero/CalculatorCard.tsx
git commit -m "feat(hero): add CalculatorCard composer"
```

---

### Task 20: Create `<HeroContent>` (left side)

**Files:**
- Create: `landing-page/src/components/sections/hero/HeroContent.tsx`

- [ ] **Step 1: Create HeroContent**

```tsx
import Link from 'next/link'
import { Button } from '@/components/ui/Button'

interface HeroContentProps {
  headline: { prefix: string; highlight: string }
  subheading: string
  primaryCta: { label: string; href: string }
  secondaryCta: { label: string; href: string }
}

export function HeroContent({
  headline,
  subheading,
  primaryCta,
  secondaryCta,
}: HeroContentProps) {
  return (
    <div className="w-full max-w-[640px]">
      <h1 className="text-4xl font-bold leading-[1.25] text-blue10 md:text-5xl lg:text-6xl">
        {headline.prefix}
        <span className="text-pinkDA">{headline.highlight}</span>
      </h1>
      <p className="mt-6 text-base text-grey6C md:text-lg lg:text-xl">
        {subheading}
      </p>
      <div className="mt-8 flex flex-wrap items-center gap-3">
        <Link href={primaryCta.href}>
          <Button variant="primary" size="lg">{primaryCta.label}</Button>
        </Link>
        <Link href={secondaryCta.href}>
          <Button variant="outline" size="lg">{secondaryCta.label}</Button>
        </Link>
      </div>
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/sections/hero/HeroContent.tsx
git commit -m "feat(hero): add HeroContent (headline + CTAs)"
```

---

### Task 21: Create `<HeroSection>` (composes left + right + gradient blob)

**Files:**
- Create: `landing-page/src/components/sections/HeroSection.tsx`

- [ ] **Step 1: Create HeroSection**

```tsx
import { Container } from '@/components/layout/Container'
import { HERO_CONTENT, HERO_CALCULATOR } from '@/lib/constants'
import { HeroContent } from './hero/HeroContent'
import { CalculatorCard } from './hero/CalculatorCard'

export function HeroSection() {
  return (
    <section className="relative overflow-hidden bg-white py-16 lg:py-24">
      <div
        aria-hidden="true"
        className="pointer-events-none absolute -right-32 top-0 h-[600px] w-[600px] rounded-full bg-gradient-to-br from-pinkLight to-pinkDA opacity-40 blur-3xl lg:opacity-50"
      />

      <Container className="relative">
        <div className="flex flex-col items-start gap-12 lg:flex-row lg:items-center lg:justify-between lg:gap-8">
          <HeroContent
            headline={HERO_CONTENT.headline}
            subheading={HERO_CONTENT.subheading}
            primaryCta={HERO_CONTENT.primaryCta}
            secondaryCta={HERO_CONTENT.secondaryCta}
          />
          <CalculatorCard
            title={HERO_CALCULATOR.title}
            liveRateLabel={HERO_CALCULATOR.liveRateLabel}
            sendInput={HERO_CALCULATOR.sendInput}
            receiveInput={HERO_CALCULATOR.receiveInput}
            fees={HERO_CALCULATOR.fees}
            badge={HERO_CALCULATOR.badge}
          />
        </div>
      </Container>
    </section>
  )
}
```

- [ ] **Step 2: Wire Hero into page.tsx for visual checkpoint**

Path: `landing-page/src/app/page.tsx` (overwrite default)

```tsx
import { HeroSection } from '@/components/sections/HeroSection'

export default function HomePage() {
  return (
    <main>
      <HeroSection />
    </main>
  )
}
```

- [ ] **Step 3: Run dev server, visual check**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm run dev
```

Open `http://localhost:3000`. Verify:
- Headline reads "Fast, easy international money transfers to **70+ countries**" (pink on "70+ countries")
- Sub-heading visible below
- Two buttons: pink "Send money now", outline "See live rates"
- Calculator card on right with IDR flag, "You send" / "Recipient gets", fee rows, "Money arrives in **Real-time**" badge
- Gradient pink blob behind card (subtle)
- No console errors

Compare side-by-side with `assets/figma-exports/hero-section.png`.

Stop server.

- [ ] **Step 4: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/sections/HeroSection.tsx landing-page/src/app/page.tsx
git commit -m "feat(hero): compose HeroSection and wire into page"
```

---

## Phase 6 — Navbar

### Task 22: Create `<TransfezLogo>` wrapper

**Files:**
- Create: `landing-page/src/components/layout/navbar/TransfezLogo.tsx`

- [ ] **Step 1: Create TransfezLogo**

```tsx
/* eslint-disable @next/next/no-img-element */
interface TransfezLogoProps {
  className?: string
}

export function TransfezLogo({ className }: TransfezLogoProps) {
  return (
    <div
      className={className}
      style={{ position: 'relative', width: 139.5, height: 24 }}
      aria-label="Transfez"
      role="img"
    >
      <img
        src="/transfez-logo-text.svg"
        alt=""
        style={{
          position: 'absolute',
          top: 2.21,
          left: 0,
          width: 127.72,
          height: 21.79,
        }}
      />
      <img
        src="/transfez-logo-swoosh.svg"
        alt=""
        style={{
          position: 'absolute',
          top: 0,
          left: 120.53,
          width: 18.97,
          height: 12.74,
        }}
      />
    </div>
  )
}
```

- [ ] **Step 2: Visual verify in page**

Temporarily edit `landing-page/src/app/page.tsx` to add logo above Hero:

```tsx
import { HeroSection } from '@/components/sections/HeroSection'
import { TransfezLogo } from '@/components/layout/navbar/TransfezLogo'

export default function HomePage() {
  return (
    <main>
      <div className="p-8"><TransfezLogo /></div>
      <HeroSection />
    </main>
  )
}
```

Run `npm run dev` → visit `http://localhost:3000` → logo "transfez" should render correctly with pink swoosh accent above the "z".

After verifying, REVERT `page.tsx` back to just `<main><HeroSection /></main>` (we wire it properly via Navbar later).

Stop server.

- [ ] **Step 3: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/layout/navbar/TransfezLogo.tsx landing-page/src/app/page.tsx
git commit -m "feat(navbar): add TransfezLogo wrapper (text + swoosh)"
```

---

### Task 23: Create `<NavLink>`

**Files:**
- Create: `landing-page/src/components/layout/navbar/NavLink.tsx`

- [ ] **Step 1: Create NavLink**

```tsx
import Link from 'next/link'
import { ChevronDown } from 'lucide-react'

interface NavLinkProps {
  label: string
  href: string
  hasDropdown?: boolean
}

export function NavLink({ label, href, hasDropdown = false }: NavLinkProps) {
  return (
    <Link
      href={href}
      className="inline-flex items-center gap-1 text-base text-dark hover:text-pinkDA transition-colors"
    >
      <span>{label}</span>
      {hasDropdown && (
        <ChevronDown className="h-5 w-5 text-grey90" aria-hidden="true" />
      )}
    </Link>
  )
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/layout/navbar/NavLink.tsx
git commit -m "feat(navbar): add NavLink with optional chevron"
```

---

### Task 24: Create `<LanguageToggle>`

**Files:**
- Create: `landing-page/src/components/layout/navbar/LanguageToggle.tsx`

- [ ] **Step 1: Create LanguageToggle**

```tsx
import { cn } from '@/lib/utils'

interface LanguageToggleProps {
  active: 'ID' | 'EN'
  options: readonly ['ID', 'EN']
}

export function LanguageToggle({ active, options }: LanguageToggleProps) {
  return (
    <div
      className="inline-flex items-center rounded-full bg-whiteF1 p-1"
      role="group"
      aria-label="Language"
    >
      {options.map((opt) => {
        const isActive = opt === active
        return (
          <button
            key={opt}
            type="button"
            disabled
            aria-current={isActive}
            className={cn(
              'inline-flex h-8 min-w-[36px] items-center justify-center rounded-full px-3 text-sm font-semibold transition-colors',
              isActive
                ? 'bg-white text-dark shadow-sm'
                : 'text-grey90',
            )}
          >
            {opt}
          </button>
        )
      })}
    </div>
  )
}
```

Note: buttons are `disabled` because MVP is English-only and toggle is visual only.

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/layout/navbar/LanguageToggle.tsx
git commit -m "feat(navbar): add LanguageToggle (visual only, ID active)"
```

---

### Task 25: Create `<NavCta>`

**Files:**
- Create: `landing-page/src/components/layout/navbar/NavCta.tsx`

- [ ] **Step 1: Create NavCta**

```tsx
import Link from 'next/link'
import { CountryFlag } from '@/components/ui/CountryFlag'

interface NavCtaProps {
  label: string
  flagSrc: string
  flagAlt: string
  href: string
}

export function NavCta({ label, flagSrc, flagAlt, href }: NavCtaProps) {
  return (
    <Link
      href={href}
      className="inline-flex h-11 items-center gap-2 rounded-full bg-blue10 px-4 text-sm font-semibold text-white transition-colors hover:bg-blue2C"
    >
      <span>{label}</span>
      <CountryFlag src={flagSrc} alt={flagAlt} size={20} />
    </Link>
  )
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/layout/navbar/NavCta.tsx
git commit -m "feat(navbar): add NavCta (Send from Indonesia button)"
```

---

### Task 26: Create `<MobileMenu>` (hamburger + drawer)

**Files:**
- Create: `landing-page/src/components/layout/navbar/MobileMenu.tsx`

- [ ] **Step 1: Create MobileMenu**

```tsx
'use client'

import { useState } from 'react'
import { Menu, X } from 'lucide-react'
import Link from 'next/link'
import { CountryFlag } from '@/components/ui/CountryFlag'
import { NAV_LINKS, NAVBAR_CTA } from '@/lib/constants'
import { cn } from '@/lib/utils'

export function MobileMenu() {
  const [open, setOpen] = useState(false)

  return (
    <div className="md:hidden">
      <button
        type="button"
        onClick={() => setOpen((prev) => !prev)}
        aria-label={open ? 'Close menu' : 'Open menu'}
        aria-expanded={open}
        className="inline-flex h-11 w-11 items-center justify-center rounded-full hover:bg-whiteF1"
      >
        {open ? <X className="h-6 w-6" /> : <Menu className="h-6 w-6" />}
      </button>

      <div
        className={cn(
          'absolute left-0 right-0 top-full z-40 origin-top border-t border-whiteE6 bg-white px-4 py-6 shadow-card transition-all',
          open ? 'block' : 'hidden',
        )}
      >
        <nav className="flex flex-col gap-1">
          {NAV_LINKS.map((link) => (
            <Link
              key={link.label}
              href={link.href}
              onClick={() => setOpen(false)}
              className="rounded-lg px-3 py-3 text-base text-dark hover:bg-whiteF1"
            >
              {link.label}
            </Link>
          ))}
        </nav>

        <div className="mt-4 border-t border-whiteE6 pt-4">
          <Link
            href={NAVBAR_CTA.href}
            onClick={() => setOpen(false)}
            className="inline-flex h-11 w-full items-center justify-center gap-2 rounded-full bg-blue10 px-4 text-sm font-semibold text-white"
          >
            <span>{NAVBAR_CTA.label}</span>
            <CountryFlag src={NAVBAR_CTA.flagSrc} alt={NAVBAR_CTA.flagAlt} size={20} />
          </Link>
        </div>
      </div>
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/layout/navbar/MobileMenu.tsx
git commit -m "feat(navbar): add MobileMenu (hamburger + drawer, <md only)"
```

---

### Task 27: Create `<Navbar>` (composes everything)

**Files:**
- Create: `landing-page/src/components/layout/Navbar.tsx`
- Modify: `landing-page/src/app/page.tsx`

- [ ] **Step 1: Create Navbar**

```tsx
import { Container } from './Container'
import { TransfezLogo } from './navbar/TransfezLogo'
import { NavLink } from './navbar/NavLink'
import { LanguageToggle } from './navbar/LanguageToggle'
import { NavCta } from './navbar/NavCta'
import { MobileMenu } from './navbar/MobileMenu'
import { NAV_LINKS, NAVBAR_LANGUAGE, NAVBAR_CTA } from '@/lib/constants'

export function Navbar() {
  return (
    <header className="sticky top-0 z-30 w-full border-b border-whiteE6 bg-white/95 backdrop-blur">
      <Container>
        <div className="relative flex h-[68px] items-center justify-between">
          {/* Left: nav links (md+) */}
          <nav className="hidden items-center gap-8 md:flex">
            {NAV_LINKS.map((link) => (
              <NavLink
                key={link.label}
                label={link.label}
                href={link.href}
                hasDropdown={link.hasDropdown}
              />
            ))}
          </nav>

          {/* Center: logo (md+ absolute, mobile in-flow on left) */}
          <div className="md:absolute md:left-1/2 md:top-1/2 md:-translate-x-1/2 md:-translate-y-1/2">
            <TransfezLogo />
          </div>

          {/* Right: lang toggle + CTA (md+) */}
          <div className="hidden items-center gap-3 md:flex">
            <LanguageToggle
              active={NAVBAR_LANGUAGE.active}
              options={NAVBAR_LANGUAGE.options}
            />
            <NavCta
              label={NAVBAR_CTA.label}
              flagSrc={NAVBAR_CTA.flagSrc}
              flagAlt={NAVBAR_CTA.flagAlt}
              href={NAVBAR_CTA.href}
            />
          </div>

          {/* Mobile hamburger */}
          <MobileMenu />
        </div>
      </Container>
    </header>
  )
}
```

- [ ] **Step 2: Wire Navbar into page**

Overwrite `landing-page/src/app/page.tsx`:

```tsx
import { Navbar } from '@/components/layout/Navbar'
import { HeroSection } from '@/components/sections/HeroSection'

export default function HomePage() {
  return (
    <>
      <Navbar />
      <main>
        <HeroSection />
      </main>
    </>
  )
}
```

- [ ] **Step 3: Visual checkpoint (desktop)**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm run dev
```

Open `http://localhost:3000` at desktop width (≥1024px). Compare against `assets/figma-exports/navbar.png`:
- Left: "International Transfer ▾", "Others ▾", "Promo"
- Center: Transfez logo
- Right: ID|EN pill (ID active solid white bg), dark navy "Send from Indonesia 🇮🇩" pill
- Navbar sticky on scroll

- [ ] **Step 4: Visual checkpoint (mobile)**

Resize browser to 375px width. Verify:
- Logo visible on left
- Hamburger icon on right
- Click hamburger → drawer slides down with nav links + CTA
- Click X → drawer closes

Stop server.

- [ ] **Step 5: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/layout/Navbar.tsx landing-page/src/app/page.tsx
git commit -m "feat(navbar): compose Navbar (logo center, sticky, mobile drawer)"
```

---

## Phase 7 — Footer

### Task 28: Create `<Footer>` placeholder

**Files:**
- Create: `landing-page/src/components/layout/Footer.tsx`
- Modify: `landing-page/src/app/page.tsx`

- [ ] **Step 1: Create Footer**

```tsx
import Link from 'next/link'
import { Instagram, Twitter, Linkedin, Youtube } from 'lucide-react'
import { Container } from './Container'
import { FOOTER_LINKS } from '@/lib/constants'

const SOCIAL_ICON_MAP = {
  Instagram,
  Twitter,
  LinkedIn: Linkedin,
  YouTube: Youtube,
} as const

export function Footer() {
  return (
    <footer className="bg-blue10 text-white">
      <Container>
        <div className="py-12 lg:py-16">
          <div className="grid grid-cols-1 gap-10 md:grid-cols-2 lg:grid-cols-4">
            {/* Brand column */}
            <div className="lg:col-span-1">
              <h3 className="text-xl font-bold">{FOOTER_LINKS.brand.name}</h3>
              <p className="mt-3 text-sm text-white/70">
                {FOOTER_LINKS.brand.tagline}
              </p>
            </div>

            {/* Link columns */}
            {FOOTER_LINKS.columns.map((col) => (
              <div key={col.title}>
                <h4 className="text-sm font-semibold uppercase tracking-wide text-white/90">
                  {col.title}
                </h4>
                <ul className="mt-4 space-y-3">
                  {col.links.map((link) => (
                    <li key={link.label}>
                      <Link
                        href={link.href}
                        className="text-sm text-white/70 transition-colors hover:text-white"
                      >
                        {link.label}
                      </Link>
                    </li>
                  ))}
                </ul>
              </div>
            ))}
          </div>

          {/* Bottom row */}
          <div className="mt-12 flex flex-col items-start justify-between gap-4 border-t border-white/10 pt-6 sm:flex-row sm:items-center">
            <p className="text-sm text-white/70">{FOOTER_LINKS.copyright}</p>
            <div className="flex items-center gap-4">
              {FOOTER_LINKS.social.map((s) => {
                const Icon = SOCIAL_ICON_MAP[s.name as keyof typeof SOCIAL_ICON_MAP]
                return (
                  <Link
                    key={s.name}
                    href={s.href}
                    aria-label={s.name}
                    className="text-white/70 transition-colors hover:text-white"
                  >
                    <Icon className="h-5 w-5" />
                  </Link>
                )
              })}
            </div>
          </div>
        </div>
      </Container>
    </footer>
  )
}
```

- [ ] **Step 2: Wire Footer into page**

Overwrite `landing-page/src/app/page.tsx`:

```tsx
import { Navbar } from '@/components/layout/Navbar'
import { Footer } from '@/components/layout/Footer'
import { HeroSection } from '@/components/sections/HeroSection'

export default function HomePage() {
  return (
    <>
      <Navbar />
      <main>
        <HeroSection />
      </main>
      <Footer />
    </>
  )
}
```

- [ ] **Step 3: Visual check**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm run dev
```

Verify at `http://localhost:3000`:
- Footer appears below Hero with dark navy background
- 4 columns at lg+ (Brand, Product, Company, Support)
- 2 columns at md
- 1 column stacked at mobile
- Copyright + 4 social icons at bottom

Stop server.

- [ ] **Step 4: Commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git add landing-page/src/components/layout/Footer.tsx landing-page/src/app/page.tsx
git commit -m "feat(footer): add placeholder Footer (4-col grid, social icons)"
```

---

## Phase 8 — Final Verification

### Task 29: Run build, fix any errors

**Files:** none (read-only verification)

- [ ] **Step 1: Run production build**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm run build
```

Expected: build completes without errors. Look for:
- ✅ "Compiled successfully"
- ✅ All routes generated
- ❌ ANY TypeScript errors → fix in source files
- ❌ ANY warnings about missing keys, unused imports, etc → fix

- [ ] **Step 2: Run lint**

```bash
npm run lint
```
Expected: no errors. Fix any reported issues.

- [ ] **Step 3: Run tests**

```bash
npm test -- --run
```
Expected: all Button tests pass.

- [ ] **Step 4: If any fixes were made, commit them**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git status
# if changes:
git add landing-page/
git commit -m "fix: resolve build/lint/test issues"
```

---

### Task 30: Visual QA — Desktop (Figma comparison)

**Files:** none (manual)

- [ ] **Step 1: Run dev server**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm run dev
```

- [ ] **Step 2: Open browser at 1440px width**

Open `http://localhost:3000`. Set browser window to exactly 1440px wide (use DevTools device mode: "Responsive" → 1440 × 920).

- [ ] **Step 3: Compare against Figma references**

Open these 2 files side-by-side with browser:
- `assets/figma-exports/hero-section.png`
- `assets/figma-exports/navbar.png`

Checklist (mark each):
- [ ] Headline reads exactly "Fast, easy international money transfers to 70+ countries" with "70+ countries" in pink (`#DA649F`)
- [ ] Sub-heading matches exactly
- [ ] Primary CTA "Send money now" — pink filled pill, hover darkens
- [ ] Secondary CTA "See live rates" — outline pink pill
- [ ] Calculator card on right with white bg + soft shadow
- [ ] Card header: "Send money" + green dot + "Live rate"
- [ ] "You send" row: IDR flag (red/white circle) + IDR + chevron + "20,000,000"
- [ ] Fee block: "Transfez fee SGD 5.00" + "Exchange rate 1 SGD = IDR 12,743.39" on grey-white bg
- [ ] "Recipient gets" row: THB flag + THB + NO chevron + "1,440.00"
- [ ] Bottom badge: "Money arrives in **Real-time**" with sparkle icon
- [ ] Pink gradient blob visible behind card (subtle)
- [ ] Navbar logo "transfez" rendered correctly (lowercase + pink swoosh on z)
- [ ] Navbar: 3 left links, logo center, ID|EN pill + dark navy CTA on right
- [ ] Typefez font applied (not Arial fallback)

Stop server.

- [ ] **Step 4: Document any deviations**

If anything looks off, note it. Likely candidates:
- Spacing differences (gap-x, py)
- Color tone mismatches (rare since tokens are exact)
- Font weight wrong on certain text

Fix any deviations in the relevant component file, commit with message like:
```
fix(hero): adjust calculator card padding to match Figma
```

---

### Task 31: Responsive QA — Mobile + Tablet

**Files:** none (manual)

- [ ] **Step 1: Run dev server**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm run dev
```

- [ ] **Step 2: Test mobile viewport (375 × 812 — iPhone X)**

In DevTools device mode → "iPhone 12 Pro" or set custom 375×812.

Checklist:
- [ ] Hero stacks vertically: headline → CTAs → calculator
- [ ] H1 uses `text-4xl` (36px), not larger
- [ ] CTAs wrap if needed, full-width tap targets
- [ ] Calculator card centered, max-w applied (not bleeding)
- [ ] Navbar shows logo + hamburger only (no nav links / lang / CTA visible)
- [ ] Click hamburger → drawer opens with all nav links + CTA stacked
- [ ] Click X → drawer closes
- [ ] Footer stacks to 1 column

- [ ] **Step 3: Test tablet viewport (768 × 1024 — iPad)**

Checklist:
- [ ] Hero still stacked (per `md` breakpoint spec)
- [ ] H1 uses `text-5xl` (48px)
- [ ] Navbar shows full layout (links left, logo center, lang+CTA right)
- [ ] Hamburger NOT visible (replaced by full nav)
- [ ] Footer 2 columns

- [ ] **Step 4: Test desktop (1280 × 800 — laptop)**

Checklist:
- [ ] Hero side-by-side (content left, calculator right)
- [ ] H1 uses `text-6xl` (56px override)
- [ ] Footer 4 columns

- [ ] **Step 5: Sticky navbar behavior**

Scroll down on any breakpoint → navbar should stay at top with `bg-white/95 backdrop-blur` effect.

Stop server.

- [ ] **Step 6: Commit any responsive fixes**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git status
# if changes:
git add landing-page/
git commit -m "fix: responsive adjustments after mobile/tablet QA"
```

---

### Task 32: Lighthouse audit

**Files:** none (manual)

- [ ] **Step 1: Build + start production server**

```bash
cd /Users/jacktfz/Downloads/website-transfez/landing-page
npm run build
npm start
```

- [ ] **Step 2: Run Lighthouse in DevTools**

Open `http://localhost:3000` in Chrome. DevTools → Lighthouse tab → Mobile + Performance + Accessibility + Best Practices + SEO → "Analyze page load".

- [ ] **Step 3: Verify scores**

Target (per docs `04-seo-performance.md`):
- Performance: ≥ 90
- Accessibility: ≥ 90
- Best Practices: ≥ 90
- SEO: ≥ 90

Common fixes if Performance < 90:
- Font: convert OTF → WOFF2 (use online tool or `npx ttf2woff2`)
- Images: if any > 100KB, optimize
- Unused CSS: Tailwind purge should handle automatically

Common fixes if Accessibility < 90:
- Missing alt text on `<img>` → ensure all have meaningful or empty `alt=""` for decorative
- Insufficient color contrast → check grey text combinations
- Missing labels on buttons → add `aria-label` where text is icon-only

Stop server.

- [ ] **Step 4: If scores meet bar, final commit**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git log --oneline | head -10
echo "Hero section MVP complete."
```

If any fixes were needed:
```bash
git add landing-page/
git commit -m "perf: optimize for Lighthouse 90+ scores"
```

---

### Task 33: Push all commits to GitHub

**Files:** none (git operation)

- [ ] **Step 1: Push main branch**

```bash
cd /Users/jacktfz/Downloads/website-transfez
git push origin main
```

- [ ] **Step 2: Verify on GitHub**

Open https://github.com/ficho-a11y/website-transfez and verify:
- All commits visible
- `landing-page/` folder present with all files
- No sensitive files exposed (`.env.local`, etc.)

- [ ] **Step 3: Mark plan complete**

The Hero section MVP is now live in GitHub. Next steps (separate plans):
- New section spec (Features, Pricing, Testimonials, FAQ, CTA)
- Functional calculator (post-MVP)
- i18n setup (post-MVP)
- Final Footer design (when Figma ready)
- Final Navbar dropdowns (when Figma ready)

---

## Appendix — Quick reference

### Color tokens (from spec)
```
pinkDA      #DA649F   primary
pinkLight   #FDD7F5
pinkDark    #B65384
blue10      #102C57   navy
blue2C      #2C4A77
green06     #06AC4E
dark        #252525
grey6C      #6C6C71
grey90      #909098
whiteE6     #E6E6E8
whiteF1     #F1F1F1
```

### File map (alphabetical within layer)

| File | Responsibility |
|---|---|
| `app/layout.tsx` | Font setup, metadata, html lang |
| `app/page.tsx` | Compose Navbar + HeroSection + Footer |
| `app/globals.css` | Tailwind directives + CSS vars |
| `components/layout/Container.tsx` | max-w-7xl wrapper |
| `components/layout/Navbar.tsx` | Sticky header, composes navbar/* |
| `components/layout/Footer.tsx` | Placeholder footer (4-col) |
| `components/layout/navbar/LanguageToggle.tsx` | ID/EN pill (visual) |
| `components/layout/navbar/MobileMenu.tsx` | Hamburger + drawer |
| `components/layout/navbar/NavCta.tsx` | "Send from Indonesia" button |
| `components/layout/navbar/NavLink.tsx` | Text + optional chevron |
| `components/layout/navbar/TransfezLogo.tsx` | Logo wrapper |
| `components/sections/HeroSection.tsx` | Compose left + right + blob |
| `components/sections/hero/CalculatorCard.tsx` | Compose 3 sub-rows |
| `components/sections/hero/CurrencyInputRow.tsx` | Flag + currency + amount |
| `components/sections/hero/FeeDetailRow.tsx` | Icon + label + value |
| `components/sections/hero/HeroContent.tsx` | Headline + sub + CTAs |
| `components/sections/hero/RealtimeBadge.tsx` | Pill with sparkles icon |
| `components/ui/Button.tsx` | primary/outline variants |
| `components/ui/CountryFlag.tsx` | SVG flag wrapper |
| `lib/constants.ts` | All copy & dummy data |
| `lib/utils.ts` | cn() helper |
| `types/index.ts` | Shared TypeScript types |
