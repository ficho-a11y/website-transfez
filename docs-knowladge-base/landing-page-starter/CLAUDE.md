# CLAUDE.md

Dokumentasi untuk refactor Transfez Website landing page.

## Tech Stack

Next.js · Tailwind CSS v3.4 · Framer Motion / GSAP / Three.js · Netlify · Typefez (local font)

## Docs

| # | File | Isi |
|---|------|-----|
| 01 | [project-overview](./01-project-overview.md) | Tujuan, KPIs, timeline |
| 02 | [structure-directory](./02-structure-directory.md) | Folder structure, naming |
| 03 | [components-reference](./03-components-reference.md) | Daftar component |
| 04 | [seo-performance](./04-seo-performance.md) | SEO & Core Web Vitals |
| 05 | [responsive-breakpoints](./05-responsive-breakpoints.md) | Breakpoint guide |
| 06 | [animation-interactions](./06-animation-interactions.md) | Animasi & hover effects |
| 07 | [deployment-checklist](./07-deployment-checklist.md) | Pre-launch & deploy |
| 08 | [assets-resources](./08-assets-resources.md) | Asset list & free resources |
| 09 | [solid-coding-guide](./09-solid-coding-guide.md) | SOLID principles (**wajib baca**) |
| 10 | [brand-colors-typography](./10-brand-colors-typography.md) | Warna & font Transfez |

## Quick Rules

- **Sebelum ngoding:** Baca 09 (SOLID), cek 02 (structure), cek 10 (colors)
- **Saat ngoding:** 1 file = 1 job (SRP), data di `constants.ts` (OCP), props specific (ISP)
- **Sebelum commit:** Cek 04 (SEO), 05 (responsive), Lighthouse > 90
- **Sebelum launch:** Follow 07 (deployment checklist) step by step
