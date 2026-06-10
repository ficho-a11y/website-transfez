# Brand Colors & Typography — Transfez Reference

> Di-extract dari [transfez.com](https://transfez.com) (Next.js + Tailwind CSS v3.4 + shadcn/ui)

## Brand Colors

### Primary (Pink DA)

| Name | Hex | RGB | Tailwind Token | Usage |
|------|-----|-----|----------------|-------|
| Pink DA | `#DA649F` | `218, 100, 159` | `pinkDA` | CTA buttons, brand accent, links, highlights |
| Pink DA Light | `#FDD7F5` | `253, 215, 245` | — | Gradient start, light badges |
| Pink DA Dark | `#B65384` | `182, 83, 132` | — | Hover states |

**Brand gradient:** `linear-gradient(-213.5deg, #FDD7F5 3.88%, #DA649F 74.38%)`

### Blue (Secondary)

| Name | Hex | RGB | Tailwind Token | Usage |
|------|-----|-----|----------------|-------|
| Dark Navy | `#102C57` | `16, 44, 87` | `blue10` | Headings, dark sections |
| Blue Medium | `#2C4A77` | `44, 74, 119` | `blue2C` | Sub-headings, secondary text |

### Semantic

| Name | Hex | Tailwind Token | Usage |
|------|-----|----------------|-------|
| Green | `#06AC4E` | `green06` | Success, positive CTAs |
| Coral | `#FF7B5A` | — | Accent/warning |
| Gold | `#FFD27C` | — | Highlight/premium |
| Amber | `#FCB036` | — | Warning, attention |
| Purple | `#5A489C` | — | Accent/decorative |
| Teal | `#2EBCD0` | — | Accent/decorative |
| Lime | `#94EF48` | — | Accent/decorative |

### Neutral (Text)

| Name | Hex | Tailwind Token | Usage |
|------|-----|----------------|-------|
| Dark | `#252525` | `dark` / `black25` | Primary text |
| Grey Text | `#6C6C71` | `grey6C` | Secondary text |
| Grey Light | `#909098` | `grey90` | Placeholder, caption |
| Grey Mid | `#A6A6AC` | — | Disabled text |

### Neutral (Background & Border)

| Name | Hex | Tailwind Token | Usage |
|------|-----|----------------|-------|
| Border | `#E6E6E8` | `whiteE6` | Input/card borders |
| Border Light | `#F1F1F1` | `whiteF1` | Dividers |
| Grey BG | `#EAEAEA` | `greyEA` | Section backgrounds |
| Near White | `#F6F7F8` | — | Card backgrounds |
| Off White | `#F4F4F4` / `#F8F8F8` | — | Alternate section bg |
| White | `#FFFFFF` | — | Page background |

---

## Typography

### Font Files

Upload font Transfez secara manual ke folder:

```
public/
└── fonts/
    ├── TypefezRegular.woff2
    ├── TypefezMedium.woff2
    ├── TypefezSemiBold.woff2
    ├── TypefezBold.woff2
    └── TypefezExtraBold.woff2
```

> Font files akan diupload manual oleh developer. Sesuaikan nama file dengan font yang diterima.

### Font Setup

```ts
// app/layout.tsx
import localFont from "next/font/local"

const typefez = localFont({
  src: [
    { path: "../../public/fonts/TypefezRegular.woff2", weight: "400", style: "normal" },
    { path: "../../public/fonts/TypefezMedium.woff2", weight: "500", style: "normal" },
    { path: "../../public/fonts/TypefezSemiBold.woff2", weight: "600", style: "normal" },
    { path: "../../public/fonts/TypefezBold.woff2", weight: "700", style: "normal" },
    { path: "../../public/fonts/TypefezExtraBold.woff2", weight: "800", style: "normal" },
  ],
  variable: "--font-typefez",
  display: "swap",
  fallback: ["Arial", "sans-serif"],
})

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="id" className={typefez.variable}>
      <body>{children}</body>
    </html>
  )
}
```

### Font Weights (Used)

| Weight | Name | CSS Variable | Usage |
|--------|------|-------------|-------|
| 400 | Regular | `font-normal` | Body text |
| 500 | Medium | `font-medium` | Labels, badges |
| 600 | SemiBold | `font-semibold` | Sub-headings, card titles |
| 700 | Bold | `font-bold` | Section headings, hero |
| 800 | ExtraBold | `font-extrabold` | Hero headline, big numbers |

### Tailwind Config Snippet

```ts
// tailwind.config.ts
import type { Config } from "tailwindcss"

const config: Config = {
  theme: {
    extend: {
      colors: {
        pinkDA: "rgb(218 100 159)",
        green06: "rgb(6 172 78)",
        greyEA: "rgb(234 234 234)",
        grey6C: "rgb(108 108 113)",
        grey90: "rgb(144 144 152)",
        dark: "rgb(34 34 34)",
        blue10: "rgb(16 44 87)",
        blue2C: "rgb(44 74 119)",
        black25: "rgb(37 37 37)",
        whiteE6: "rgb(230 230 232)",
        whiteF1: "rgb(241 241 241)",
      },
      fontFamily: {
        sans: ["var(--font-typefez)", "Arial", "sans-serif"],
      },
    },
  },
}
export default config
```

### CSS Custom Properties (shadcn/ui style)

Transfez menggunakan shadcn/ui semantic tokens via HSL variables:

```css
:root {
  --background: /* put your value */;
  --foreground: /* put your value */;
  --primary: /* pinkDA */;
  --primary-foreground: /* white */;
  --secondary: /* blue10 */;
  --secondary-foreground: /* white */;
  --border: /* whiteE6 */;
  --input: /* whiteE6 */;
  --ring: /* pinkDA */;
  --destructive: /* red */;
}
```

---

## Border Radius

| Element | Radius |
|---------|--------|
| Cards | `16px` (`rounded-2xl`) |
| Inputs | `8px` (`rounded-lg`) |
| Progress bar | `6px` (`rounded-md`) |
| Badges/pills | `rounded-full` |
| Circle elements | `rounded-full` |

## Shadows

| Usage | Value |
|-------|-------|
| Card shadow | `0 8px 16px -4px rgba(22, 34, 51, 0.08)` |
| Input border default | `#E6E6E8` |
| Input border hover | `#BBBBC0` |
