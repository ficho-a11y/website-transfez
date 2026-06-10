# Hero Section — Transfez Website Landing Page (Design Spec)

**Date:** 2026-06-10
**Status:** Draft (pending user review)
**Owner:** Ficho (ficho@transfez.com)
**Phase:** Initial section build — Homepage / Hero (first of section-by-section homepage delivery)

---

## 1. Overview

Refactor halaman utama (Homepage) Transfez Website ke Next.js + Tailwind. Spec ini mengcover **Section Hero saja** + shell halaman minimal (Navbar dari Figma, Footer placeholder) sebagai pondasi untuk section-section berikutnya yang akan dispec terpisah.

Hero menampilkan value proposition Transfez (kirim uang internasional cepat ke 70+ negara) + Calculator Card sebagai visual demo (tanpa logic functional di fase ini).

**Sumber design:** Figma `PmUWlss8YXxQbeWPUVD2zn`
- Hero section: node `1260:4599`
- Navbar: node `1260:9226`

---

## 2. Goals & Non-Goals

### Goals
- Render Hero pixel-accurate dengan design Figma untuk desktop (1440px viewport)
- Responsive mobile-first dengan stacking behavior sesuai docs `05-responsive-breakpoints.md`
- Component decomposition mengikuti SOLID principles (docs `09-solid-coding-guide.md`)
- Foundation Next.js + Tailwind + Typefez font ready untuk section berikutnya tanpa rework
- Navbar live (sesuai Figma) + Footer placeholder

### Non-Goals (out of scope di fase ini)
- ❌ Calculator card functional (no real exchange rate calculation, no API integration)
- ❌ Currency dropdown interactivity (chevron tampil tapi tidak open menu)
- ❌ Language switcher functional (toggle UI ada, klik tidak switch locale)
- ❌ i18n setup (`next-intl`, multi-locale routing) — English only untuk MVP
- ❌ Animasi (Framer Motion / GSAP / Three.js)
- ❌ Navbar dropdown menus untuk "International Transfer" & "Others" (chevron visual saja)
- ❌ Footer final design (placeholder dipakai sampai design Figma tersedia)
- ❌ Other homepage sections (Features, Pricing, Testimonials, FAQ, CTA) — akan dispec terpisah

---

## 3. Decisions Log (dari brainstorming session)

| # | Topic | Decision | Rationale |
|---|---|---|---|
| 1 | Design source | Figma MCP (per-section, link saat dibutuhkan) | Paling akurat untuk tokens & layout |
| 2 | First section | Homepage > Hero | Natural entry point |
| 3 | Calculator card | Static visual (dummy values dari Figma) | Cepat ship, no backend dependency |
| 4 | Project location | `/Users/jacktfz/Downloads/website-transfez/landing-page/` | Sebelah folder docs, rapi |
| 5 | Scope | Hero + Navbar (Figma) + Footer placeholder + full Next.js setup | Realistic context, foundation ready |
| 6 | Mobile design | Tidak ada Figma mobile → improvisasi mengikuti docs 05 | Best practice stacking |
| 7 | Bahasa | English only | Sesuai design Figma, simple MVP |
| 8 | Animasi | None | Fokus shipping, performance first |
| 9 | Component approach | SOLID-strict decomposition (Approach 2) | Wajib per docs 09, future-reusable |
| 10 | Font | Typefez asli (OTF) dari `~/Library/Fonts/TypeFezStd-*.otf` | User sudah punya font asli installed |
| 11 | Logo | Combined SVG dari Figma export | Akurat, satu file, cacheable |
| 12 | Language toggle | Visual only, ID active | Konsisten dengan English-only MVP |
| 13 | Dropdown chevron | Visual only, no menu | Belum ada design dropdown |

---

## 4. Tech Stack

| Category | Choice | Version | Notes |
|---|---|---|---|
| Framework | Next.js | 14+ (App Router) | TypeScript strict mode |
| Styling | Tailwind CSS | 3.4.x | + PostCSS + Autoprefixer |
| Font | Typefez (local) | TypeFezStd .otf | 5 weights: 400/500/600/700/800 |
| Icons | lucide-react | latest | Tree-shakeable per icon |
| Utilities | clsx + tailwind-merge | latest | `cn()` helper |
| Animation | — | — | Skipped untuk fase ini |
| Hosting | Netlify | — | Auto-detect Next.js |

---

## 5. Folder Structure

```
landing-page/
├── public/
│   ├── fonts/
│   │   ├── TypeFezStd-Regular.otf
│   │   ├── TypeFezStd-Medium.otf
│   │   ├── TypeFezStd-SemiBold.otf
│   │   ├── TypeFezStd-Bold.otf
│   │   └── TypeFezStd-ExtraBold.otf
│   ├── flags/
│   │   ├── id.svg              # Indonesia (Hero send + Navbar CTA)
│   │   └── th.svg              # Thailand (Hero receive)
│   ├── transfez-logo.svg       # Combined logo (text + swoosh)
│   └── favicon.ico
├── src/
│   ├── app/
│   │   ├── layout.tsx          # font setup, metadata, html lang="en"
│   │   ├── page.tsx            # <Navbar/> <HeroSection/> <Footer/>
│   │   └── globals.css         # Tailwind directives + CSS vars
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Navbar.tsx
│   │   │   ├── Footer.tsx
│   │   │   ├── Container.tsx
│   │   │   └── navbar/
│   │   │       ├── NavLink.tsx
│   │   │       ├── LanguageToggle.tsx
│   │   │       ├── NavCta.tsx
│   │   │       ├── TransfezLogo.tsx
│   │   │       └── MobileMenu.tsx
│   │   ├── sections/
│   │   │   ├── HeroSection.tsx
│   │   │   └── hero/
│   │   │       ├── HeroContent.tsx
│   │   │       ├── CalculatorCard.tsx
│   │   │       ├── CurrencyInputRow.tsx
│   │   │       ├── FeeDetailRow.tsx
│   │   │       └── RealtimeBadge.tsx
│   │   └── ui/
│   │       ├── Button.tsx
│   │       └── CountryFlag.tsx
│   ├── lib/
│   │   ├── constants.ts
│   │   └── utils.ts            # cn() helper
│   └── types/
│       └── index.ts
├── tailwind.config.ts
├── next.config.js
├── postcss.config.js
├── tsconfig.json
├── package.json
└── .gitignore
```

---

## 6. Design Tokens (Tailwind Config)

Diambil dari docs `10-brand-colors-typography.md` + Figma variable export.

### Colors
```ts
colors: {
  pinkDA:      'rgb(218 100 159)',  // primary CTA, "70+ countries" highlight, focus ring
  pinkLight:   'rgb(253 215 245)',  // gradient blob start
  pinkDark:    'rgb(182 83 132)',   // hover state primary CTA
  blue10:      'rgb(16 44 87)',     // headline dark navy, Navbar CTA bg, Footer bg
  blue2C:      'rgb(44 74 119)',
  green06:     'rgb(6 172 78)',     // "Live rate" dot, success
  greenSubtle: 'rgb(218 245 224)',  // success subtle (badge)
  dark:        'rgb(37 37 37)',     // primary text
  grey6C:      'rgb(108 108 113)',  // secondary text
  grey90:      'rgb(144 144 152)',  // sub-heading, placeholder
  whiteE6:     'rgb(230 230 232)',  // input/card border
  whiteF1:     'rgb(241 241 241)',  // fee section bg, divider
}
```

### Typography
```ts
fontFamily: {
  sans: ['var(--font-typefez)', 'Arial', 'sans-serif'],
}
```

Type scale (Hero-relevant). Custom Tailwind config akan override `text-6xl` ke 56px untuk match Figma exact:

| Class | Size | Weight | Line-height | Usage |
|---|---|---|---|---|
| `text-6xl` (lg+) | **56px** (override) | 700 | 1.25 | Hero H1 desktop |
| `text-5xl` (md) | 48px | 700 | 1.25 | Hero H1 tablet |
| `text-4xl` (mobile) | 36px | 700 | 1.25 | Hero H1 mobile |
| `text-2xl` | 24px | 400 | 1.6 | Sub-heading (lg+) |
| `text-xl` | 20px | 400 | 1.6 | Sub-heading (mobile-tablet) |
| `text-base` | 16px | 600 | 1.0 | Button labels, calculator title |
| `text-sm` | 14px | 400 | 1.0 | Labels ("You send", "Recipient gets") |

### Spacing
Tailwind default scale dipakai. Custom semantic spacing dari Figma:
- `spacing-sm` = 4px → `gap-1` / `p-1`
- `spacing-md` = 8px → `gap-2` / `p-2`
- `spacing-2md` = 12px → `gap-3` / `p-3`
- `spacing-lg` = 16px → `gap-4` / `p-4`
- `spacing-2lg` = 20px → `gap-5` / `p-5`
- `spacing-xl` = 24px → `gap-6` / `p-6`

### Border Radius
- Card: `rounded-2xl` (16px)
- Input/Button md: `rounded-lg` (8px)
- Button lg / Pill: `rounded-full`

### Shadows
- Card: `shadow-[0_8px_16px_-4px_rgba(22,34,51,0.08)]` (custom utility)

---

## 7. Component Architecture

### Component Tree

```
<RootLayout>                           # font + metadata
└── <HomePage>
    ├── <Navbar>                       # sticky, white bg, 68px height
    │   ├── <Container>
    │   │   ├── (left) NavLinks
    │   │   │   ├── <NavLink label="International Transfer" hasDropdown />
    │   │   │   ├── <NavLink label="Others" hasDropdown />
    │   │   │   └── <NavLink label="Promo" />
    │   │   ├── (center) <TransfezLogo />
    │   │   └── (right) controls
    │   │       ├── <LanguageToggle active="ID" />
    │   │       └── <NavCta label="Send from Indonesia" flag={ID} />
    │   └── <MobileMenu />             # mobile only, hamburger + drawer
    │
    ├── <main>
    │   └── <HeroSection>              # split lg+, stacked mobile
    │       └── <Container>
    │           ├── <HeroContent>      # left
    │           │   ├── <h1>           # with <span text-pinkDA> highlight
    │           │   ├── <p>            # sub-heading
    │           │   └── CTAs
    │           │       ├── <Button variant="primary" />
    │           │       └── <Button variant="outline" />
    │           └── <CalculatorCard>   # right
    │               ├── header (title + live-rate-status)
    │               ├── <CurrencyInputRow label="You send" />
    │               ├── fee block (whiteF1 bg)
    │               │   ├── <FeeDetailRow icon={Minus} />
    │               │   └── <FeeDetailRow icon={X} />
    │               ├── <CurrencyInputRow label="Recipient gets" />
    │               └── <RealtimeBadge />
    │
    └── <Footer>                       # placeholder, 4-col grid, blue10 bg
```

### Props Interfaces (TypeScript)

```ts
// HeroSection — no props, reads from constants
export function HeroSection(): JSX.Element

// HeroContent
interface HeroContentProps {
  headline: { prefix: string; highlight: string }
  subheading: string
  primaryCta: { label: string; href: string }
  secondaryCta: { label: string; href: string }
}

// CalculatorCard
interface CalculatorCardProps {
  title: string
  liveRateLabel: string
  sendInput: CurrencyInput
  receiveInput: CurrencyInput
  fees: FeeDetail[]
  badge: { prefix: string; highlight: string }
}

// CurrencyInputRow
interface CurrencyInputRowProps {
  label: string
  currency: Currency
  amount: string
  showDropdown?: boolean   // default true; false hides chevron
}

// FeeDetailRow
interface FeeDetailRowProps {
  icon: LucideIcon
  label: string
  value: string
}

// RealtimeBadge
interface RealtimeBadgeProps {
  prefix: string
  highlight: string
}

// ui/Button
interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant: 'primary' | 'outline'
  size?: 'md' | 'lg'        // default 'lg' (50px height)
  asChild?: boolean         // Radix Slot for rendering as <a>
}

// ui/CountryFlag
interface CountryFlagProps {
  src: string
  alt: string
  size?: number             // default 28
}

// layout/navbar/NavLink
interface NavLinkProps {
  label: string
  href: string
  hasDropdown?: boolean
}

// layout/navbar/LanguageToggle
interface LanguageToggleProps {
  active: 'ID' | 'EN'
  options: readonly ['ID', 'EN']
}

// layout/navbar/NavCta
interface NavCtaProps {
  label: string
  flagSrc: string
  flagAlt: string
  href: string
}
```

### Shared Types (`types/index.ts`)

```ts
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

---

## 8. Data Layer (`lib/constants.ts`)

```ts
import { Minus, X } from 'lucide-react'

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
  ],
  badge: { prefix: 'Money arrives in', highlight: 'Real-time' },
} as const

export const FOOTER_LINKS = {
  brand: { name: 'Transfez', tagline: 'Send money fast and reliably worldwide' },
  columns: [
    { title: 'Product', links: [
      { label: 'Send money', href: '#' },
      { label: 'Rates',      href: '#' },
      { label: 'Pricing',    href: '#' },
    ]},
    { title: 'Company', links: [
      { label: 'About us', href: '#' },
      { label: 'Careers',  href: '#' },
      { label: 'Blog',     href: '#' },
    ]},
    { title: 'Support', links: [
      { label: 'Help center', href: '#' },
      { label: 'Contact',     href: '#' },
      { label: 'FAQ',         href: '#' },
    ]},
  ],
  social: [
    { name: 'Instagram', href: '#', icon: 'Instagram' },
    { name: 'Twitter',   href: '#', icon: 'Twitter' },
    { name: 'LinkedIn',  href: '#', icon: 'Linkedin' },
    { name: 'YouTube',   href: '#', icon: 'Youtube' },
  ],
  copyright: '© 2026 Transfez. All rights reserved.',
} as const
```

---

## 9. Asset Strategy

| Asset | Source | Destination | Format |
|---|---|---|---|
| Typefez fonts (5 weights) | `~/Library/Fonts/TypeFezStd-*.otf` | `public/fonts/` | OTF (copy as-is) |
| Indonesia flag | Wikimedia Commons SVG | `public/flags/id.svg` | SVG static |
| Thailand flag | Wikimedia Commons SVG | `public/flags/th.svg` | SVG static |
| Transfez logo | Figma export (combined) | `public/transfez-logo.svg` | SVG single file |
| Calculator icons | `lucide-react` | npm package | Tree-shakeable per icon |
| Social icons (Footer) | `lucide-react` | npm package | Tree-shakeable per icon |
| Hero gradient blob | Pure CSS (`radial-gradient` + `blur`) | inline component style | No file |

**Font fallback:** `Arial, sans-serif` di `next/font/local` config dengan `display: 'swap'` untuk avoid FOIT.

**Optional optimization (post-MVP):** Convert OTF → WOFF2 untuk ~70% size reduction.

---

## 10. Responsive Strategy

Mobile-first per docs `05-responsive-breakpoints.md`.

### Hero
| Breakpoint | Layout | H1 Size |
|---|---|---|
| `< md` (< 768px) | Stacked: heading → CTAs → calculator card | `text-4xl` (36px) |
| `md` (768-1023px) | Stacked dengan calculator card constrained width | `text-5xl` (48px) |
| `lg+` (≥ 1024px) | Side-by-side: content kiri, calculator kanan | `text-6xl` (56px) |

### Navbar
| Breakpoint | Behavior |
|---|---|
| `< md` | Logo (kiri) + hamburger (kanan). Left nav, lang toggle, CTA hidden. Hamburger toggles drawer dengan stacked items |
| `md+` | Full layout sesuai Figma: nav links kiri, logo center, lang+CTA kanan |

### Footer
| Breakpoint | Columns |
|---|---|
| `< md` | 1 col stacked |
| `md` | 2 col |
| `lg+` | 4 col |

### Touch targets
Min 44×44px per docs (semua button & nav link harus comply).

---

## 11. Acceptance Criteria

Hero section dianggap **selesai** jika:

- [ ] Project Next.js bisa di-`npm run dev` dan render Hero tanpa error/warning
- [ ] Typefez font load correctly di semua weight yang dipakai (verify via DevTools Network tab)
- [ ] Visual matching Figma untuk desktop (1440px) — side-by-side comparison ≥95% accurate
- [ ] Responsive correctly di 3 breakpoint (mobile 375px, tablet 768px, desktop 1440px)
- [ ] Navbar sticky on scroll, mobile hamburger functional
- [ ] CTA buttons clickable (href `#` ok), hover state visible
- [ ] No console errors / warnings
- [ ] Lighthouse mobile score > 90 (per docs `04-seo-performance.md`)
- [ ] TypeScript strict mode pass tanpa `any`
- [ ] Code structure mengikuti SOLID per docs `09` (verified via folder review)

---

## 12. Open Questions / Risks

| Risk | Mitigation |
|---|---|
| Logo SVG size (~40KB combined) terasa berat | Acceptable untuk MVP. Optimization: SVGO pass + remove unused metadata bisa cut ke ~15KB |
| OTF font lebih besar dari WOFF2 (~3x) | Acceptable untuk MVP. Post-launch optimization: convert ke WOFF2 |
| Wikimedia flag SVG kadang oversized (full national flag detail) | Saya pakai versi optimized/simplified jika ada; fallback resize via `width` prop |
| Calculator dummy data hardcoded | Sengaja — fase ini static. Future spec untuk functional version |
| Footer placeholder tidak match design final | Acceptable — semua link/text di `FOOTER_LINKS` constant, mudah update |

---

## 13. Next Steps

1. ✅ Spec ini di-review oleh user
2. Setelah approved → invoke `writing-plans` skill untuk buat implementation plan step-by-step
3. Implementation plan akan break-down jadi sub-tasks executable (init project → setup tailwind → font → flags → constants → Button → CountryFlag → HeroContent → CurrencyInputRow → FeeDetailRow → RealtimeBadge → CalculatorCard → HeroSection → TransfezLogo → NavLink → LanguageToggle → NavCta → MobileMenu → Navbar → Footer → wire ke page.tsx → manual visual QA)
