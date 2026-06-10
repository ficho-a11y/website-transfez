# SOLID Principles - Coding Guide

## Apa itu SOLID?

SOLID adalah 5 prinsip design pattern yang bikin code:
- Mudah dibaca & maintain
- Scalable & extensible
- Less bugs & easier debugging

---

## S - Single Responsibility Principle (SRP)

> Satu file/component = satu tanggung jawab

### SALAH
```tsx
// components/LandingPage.tsx — SEMUA ada di satu file
export default function LandingPage() {
  return (
    <div>
      {/* Navbar */}
      <nav>...</nav>

      {/* Hero */}
      <section>
        <h1>Welcome</h1>
        <p>Subtitle</p>
        <button>CTA</button>
      </section>

      {/* Features */}
      <section>
        <div>Feature 1</div>
        <div>Feature 2</div>
      </section>

      {/* Pricing */}
      <section>
        <div>Basic</div>
        <div>Pro</div>
      </section>

      {/* Footer */}
      <footer>...</footer>
    </div>
  )
}
```

### BENAR
```tsx
// app/page.tsx — hanya menyusun layout
import Navbar from "@/components/layout/Navbar"
import HeroSection from "@/components/sections/HeroSection"
import FeaturesSection from "@/components/sections/FeaturesSection"
import PricingSection from "@/components/sections/PricingSection"
import Footer from "@/components/layout/Footer"

export default function LandingPage() {
  return (
    <main>
      <Navbar />
      <HeroSection />
      <FeaturesSection />
      <PricingSection />
      <Footer />
    </main>
  )
}
```

```tsx
// components/sections/HeroSection.tsx — TANGGUNG JAWAB SATU: hero saja
import Button from "@/components/ui/Button"

interface HeroSectionProps {
  headline: string
  subheadline: string
  ctaLabel: string
  ctaHref: string
  secondaryLabel?: string
  secondaryHref?: string
}

export default function HeroSection({
  headline,
  subheadline,
  ctaLabel,
  ctaHref,
  secondaryLabel,
  secondaryHref,
}: HeroSectionProps) {
  return (
    <section className="py-20 md:py-28">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <h1 className="text-4xl md:text-6xl font-bold tracking-tight">
          {headline}
        </h1>
        <p className="mt-6 text-lg text-gray-600 max-w-2xl mx-auto">
          {subheadline}
        </p>
        <div className="mt-10 flex gap-4 justify-center">
          <Button href={ctaHref}>{ctaLabel}</Button>
          {secondaryLabel && (
            <Button variant="outline" href={secondaryHref}>
              {secondaryLabel}
            </Button>
          )}
        </div>
      </div>
    </section>
  )
}
```

### Checklist SRP
- [ ] Satu component = satu section / satu UI element
- [ ] Business logic terpisah dari UI
- [ ] Constants & config di file terpisah

---

## O - Open/Closed Principle (OCP)

> Component terbuka untuk extension, tertutup untuk modification

### SALAH
```tsx
// Ubah component SETIAP KALI ada pricing tier baru
function PricingCard({ tier }: { tier: "basic" | "pro" }) {
  if (tier === "basic") {
    return (
      <div>
        <h3>Basic</h3>
        <p>Rp 99.000</p>
        <ul>
          <li>Feature A</li>
          <li>Feature B</li>
        </ul>
      </div>
    )
  }
  if (tier === "pro") {
    return (
      <div>
        <h3>Pro</h3>
        <p>Rp 199.000</p>
        <ul>
          <li>Feature A</li>
          <li>Feature B</li>
          <li>Feature C</li>
        </ul>
      </div>
    )
  }
  return null
}
```

### BENAR
```tsx
// types/index.ts — Definisi data structure
export interface PricingTier {
  name: string
  price: number
  period: string
  features: string[]
  highlighted?: boolean
  ctaLabel: string
  ctaHref: string
}
```

```tsx
// lib/constants.ts — Data driven, tinggal tambah object baru
import { PricingTier } from "@/types"

export const PRICING_TIERS: PricingTier[] = [
  {
    name: "Basic",
    price: 99000,
    period: "/bulan",
    features: [
      "Feature A",
      "Feature B",
    ],
    ctaLabel: "Mulai Gratis",
    ctaHref: "/signup?plan=basic",
  },
  {
    name: "Pro",
    price: 199000,
    period: "/bulan",
    features: [
      "Feature A",
      "Feature B",
      "Feature C",
    ],
    highlighted: true,
    ctaLabel: "Mulai Sekarang",
    ctaHref: "/signup?plan=pro",
  },
  // Tinggal tambah tier baru di sini, TANPA ubah component
]
```

```tsx
// components/sections/PricingSection.tsx — Tidak perlu diubah saat ada tier baru
import { PricingTier } from "@/types"
import PricingCard from "@/components/ui/PricingCard"

interface PricingSectionProps {
  tiers: PricingTier[]
}

export default function PricingSection({ tiers }: PricingSectionProps) {
  return (
    <section className="py-20">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
          {tiers.map((tier) => (
            <PricingCard key={tier.name} tier={tier} />
          ))}
        </div>
      </div>
    </section>
  )
}
```

```tsx
// components/ui/PricingCard.tsx — Generic, menerima data via props
import { PricingTier } from "@/types"
import Button from "./Button"

interface PricingCardProps {
  tier: PricingTier
}

export default function PricingCard({ tier }: PricingCardProps) {
  return (
    <div
      className={`p-6 rounded-xl border ${
        tier.highlighted
          ? "border-blue-500 ring-2 ring-blue-500"
          : "border-gray-200"
      }`}
    >
      {tier.highlighted && (
        <span className="inline-block px-3 py-1 text-xs font-medium bg-blue-100 text-blue-700 rounded-full mb-4">
          Recommended
        </span>
      )}
      <h3 className="text-xl font-semibold">{tier.name}</h3>
      <p className="mt-4 text-3xl font-bold">
        Rp {tier.price.toLocaleString("id-ID")}
        <span className="text-base font-normal text-gray-500">{tier.period}</span>
      </p>
      <ul className="mt-6 space-y-3">
        {tier.features.map((feature) => (
          <li key={feature} className="flex items-center gap-2">
            <svg className="w-5 h-5 text-green-500 shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M5 13l4 4L19 7" />
            </svg>
            {feature}
          </li>
        ))}
      </ul>
      <Button
        variant={tier.highlighted ? "primary" : "outline"}
        className="w-full mt-8"
        href={tier.ctaHref}
      >
        {tier.ctaLabel}
      </Button>
    </div>
  )
}
```

### Checklist OCP
- [ ] Data driven — content dari constants/config, bukan hardcoded
- [ ] Component menerima data via props, tidak hardcode content
- [ ] Tambah fitur baru = tambah data/config, bukan ubah component

---

## L - Liskov Substitution Principle (LSP)

> Child component harus bisa menggantikan parent tanpa break

### SALAH
```tsx
// Button biasa
function Button({ children, onClick }: { children: React.ReactNode; onClick: () => void }) {
  return <button onClick={onClick}>{children}</button>
}

// "Button" tapi behavior berbeda — kadang link, kadang button
// Consumer harus cek tipe dulu
function ActionButton({ href, onClick, children }: any) {
  if (href) {
    return <a href={href}>{children}</a>  // BUKAN button!
  }
  return <button onClick={onClick}>{children}</button>
}
```

### BENAR
```tsx
// components/ui/Button.tsx — Konsisten, bisa dipakai sebagai <button> atau <a>
type ButtonBaseProps = {
  children: React.ReactNode
  variant?: "primary" | "secondary" | "outline" | "ghost"
  size?: "sm" | "md" | "lg"
  className?: string
}

type ButtonAsButton = ButtonBaseProps & {
  as?: "button"
  onClick?: () => void
  type?: "button" | "submit" | "reset"
  href?: never
}

type ButtonAsLink = ButtonBaseProps & {
  as: "a"
  href: string
  onClick?: never
  type?: never
}

type ButtonProps = ButtonAsButton | ButtonAsLink

const variantStyles: Record<string, string> = {
  primary: "bg-blue-600 text-white hover:bg-blue-700",
  secondary: "bg-gray-600 text-white hover:bg-gray-700",
  outline: "border border-gray-300 text-gray-700 hover:bg-gray-50",
  ghost: "text-gray-700 hover:bg-gray-100",
}

const sizeStyles: Record<string, string> = {
  sm: "px-3 py-1.5 text-sm",
  md: "px-5 py-2.5 text-base",
  lg: "px-7 py-3.5 text-lg",
}

export default function Button({
  children,
  variant = "primary",
  size = "md",
  className = "",
  ...props
}: ButtonProps) {
  const baseClasses = `inline-flex items-center justify-center font-medium rounded-lg transition-all duration-150 ${variantStyles[variant]} ${sizeStyles[size]} ${className}`

  if (props.as === "a") {
    return (
      <a className={baseClasses} href={props.href}>
        {children}
      </a>
    )
  }

  return (
    <button
      className={baseClasses}
      onClick={props.onClick}
      type={props.type ?? "button"}
    >
      {children}
    </button>
  )
}
```

### Checklist LSP
- [ ] Component punya API yang konsisten
- [ ] Variant/size tidak mengubah fundamental behavior
- [ ] Polymorphic components punya type safety yang ketat

---

## I - Interface Segregation Principle (ISP)

> Props yang specific, bukan satu giant props object

### SALAH
```tsx
// Semua data landing page di satu props
interface LandingPageData {
  heroHeadline: string
  heroSubheadline: string
  heroCtaLabel: string
  heroImage: string
  features: Feature[]
  pricingTiers: PricingTier[]
  testimonials: Testimonial[]
  faqItems: FAQ[]
  navLinks: NavLink[]
  footerLinks: FooterLink[]
  companyName: string
  logoUrl: string
  socialLinks: SocialLink[]
}

// Section terima SEMUA data padahal cuma butuh sebagian
function HeroSection({ data }: { data: LandingPageData }) {
  return <h1>{data.heroHeadline}</h1>
}
```

### BENAR
```tsx
// Setiap section punya interface PROPS-nya sendiri

// Hanya punya props yang dia butuhkan
interface HeroProps {
  headline: string
  subheadline: string
  ctaLabel: string
  ctaHref: string
  secondaryLabel?: string
  secondaryHref?: string
  imageUrl?: string
}

function HeroSection({
  headline,
  subheadline,
  ctaLabel,
  ctaHref,
}: HeroProps) {
  return (
    <section>
      <h1>{headline}</h1>
      <p>{subheadline}</p>
      <a href={ctaHref}>{ctaLabel}</a>
    </section>
  )
}

// Pricing section cuma butuh pricing data
interface PricingProps {
  tiers: PricingTier[]
}

function PricingSection({ tiers }: PricingProps) {
  return (
    <section>
      {tiers.map((tier) => (
        <PricingCard key={tier.name} tier={tier} />
      ))}
    </section>
  )
}

// FAQ section cuma butuh FAQ data
interface FAQProps {
  items: FAQ[]
}

function FAQSection({ items }: FAQProps) {
  return (
    <section>
      {items.map((item) => (
        <AccordionItem key={item.question} {...item} />
      ))}
    </section>
  )
}
```

### Checklist ISP
- [ ] Setiap component punya typed props interface sendiri
- [ ] Props hanya berisi data yang component butuhkan
- [ ] Tidak ada "god props" yang mengandung semua data
- [ ] Optional props ditandai dengan `?`

---

## D - Dependency Inversion Principle (DIP)

> Component bergantung pada abstraction (interface), bukan implementation detail

### SALAH
```tsx
// Section langsung import data — tightly coupled ke constants
import { PRICING_TIERS } from "@/lib/constants"

function PricingSection() {
  return (
    <section>
      {PRICING_TIERS.map((tier) => (
        <div key={tier.name}>{tier.name}</div>
      ))}
    </section>
  )
}
```

```tsx
// Component langsung pakai implementation detail
function SubscribeForm() {
  const res = await fetch("https://api.myapp.com/subscribe", {
    method: "POST",
    body: JSON.stringify({ email }),
  })
  // URL hardcoded, gak bisa diganti
}
```

### BENAR
```tsx
// lib/services.ts — Abstraction layer
export interface NewsletterService {
  subscribe(email: string): Promise<{ success: boolean }>
}

export interface AnalyticsService {
  trackEvent(event: string, data?: Record<string, unknown>): void
  trackPageView(path: string): void
}
```

```tsx
// lib/services/email-provider.ts — Implementation
import { NewsletterService } from "@/lib/services"

export class EmailProviderA implements NewsletterService {
  async subscribe(email: string) {
    const res = await fetch("/api/subscribe", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ email }),
    })
    return res.json()
  }
}
```

```tsx
// components/sections/CTASection.tsx — Bergantung pada abstraction
import { NewsletterService } from "@/lib/services"
import Button from "@/components/ui/Button"
import { useState } from "react"

interface CTASectionProps {
  headline: string
  subheadline: string
  newsletterService: NewsletterService
}

export default function CTASection({
  headline,
  subheadline,
  newsletterService,
}: CTASectionProps) {
  const [email, setEmail] = useState("")

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault()
    await newsletterService.subscribe(email)
  }

  return (
    <section className="py-20 bg-blue-600">
      <div className="max-w-7xl mx-auto px-4 text-center text-white">
        <h2 className="text-3xl font-bold">{headline}</h2>
        <p className="mt-4 text-blue-100">{subheadline}</p>
        <form onSubmit={handleSubmit} className="mt-8 flex gap-3 max-w-md mx-auto">
          <input
            type="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            placeholder="Email kamu"
            className="flex-1 px-4 py-2.5 rounded-lg text-gray-900"
            required
          />
          <Button type="submit" variant="secondary">
            Subscribe
          </Button>
        </form>
      </div>
    </section>
  )
}
```

```tsx
// app/page.tsx — DI container (composition root)
import { EmailProviderA } from "@/lib/services/email-provider"
import { PRICING_TIERS, HERO_DATA, FEATURES_DATA, FAQ_DATA } from "@/lib/constants"
import HeroSection from "@/components/sections/HeroSection"
import FeaturesSection from "@/components/sections/FeaturesSection"
import PricingSection from "@/components/sections/PricingSection"
import FAQSection from "@/components/sections/FAQSection"
import CTASection from "@/components/sections/CTASection"

const newsletterService = new EmailProviderA()

export default function Home() {
  return (
    <main>
      <HeroSection {...HERO_DATA} />
      <FeaturesSection features={FEATURES_DATA} />
      <PricingSection tiers={PRICING_TIERS} />
      <FAQSection items={FAQ_DATA} />
      <CTASection
        headline="Siap memulai?"
        subheadline="Daftar sekarang dan gratis 14 hari."
        newsletterService={newsletterService}
      />
    </main>
  )
}
```

### Checklist DIP
- [ ] Data datang dari props / service injection, bukan hardcoded import
- [ ] Service punya interface, component bergantung ke interface
- [ ] API calls terpisah di service layer
- [ ] Composition root (page.tsx) sebagai penghubung

---

## Full SOLID Example — File Structure

```
src/
├── app/
│   ├── layout.tsx            ← Root layout
│   ├── page.tsx              ← Composition root (wiring)
│   └── globals.css
├── components/
│   ├── layout/
│   │   ├── Navbar.tsx        ← SRP: hanya nav
│   │   └── Footer.tsx        ← SRP: hanya footer
│   ├── sections/
│   │   ├── HeroSection.tsx   ← SRP + ISP: props specific
│   │   ├── FeaturesSection.tsx
│   │   ├── PricingSection.tsx
│   │   ├── TestimonialsSection.tsx
│   │   ├── FAQSection.tsx
│   │   └── CTASection.tsx    ← DIP: terima service via props
│   └── ui/
│       ├── Button.tsx        ← LSP: konsisten button/link
│       ├── PricingCard.tsx   ← OCP: data driven
│       ├── FeatureCard.tsx
│       ├── TestimonialCard.tsx
│       ├── Accordion.tsx
│       └── Badge.tsx
├── lib/
│   ├── services.ts           ← DIP: interface definitions
│   ├── email-provider.ts     ← DIP: implementation
│   ├── constants.ts          ← OCP: data driven content
│   └── utils.ts
├── hooks/
│   ├── useScroll.ts
│   ├── useInView.ts
│   └── useMediaQuery.ts
└── types/
    └── index.ts              ← ISP: specific interfaces
```

## Quick Reference Card

| Principle | Inti | Pertanyaan |
|-----------|------|------------|
| **S** — Single Responsibility | 1 file = 1 job | "Apakah file ini punya lebih dari 1 alasan untuk berubah?" |
| **O** — Open/Closed | Data driven | "Apakah saya harus edit component untuk tambah data baru?" |
| **L** — Liskov Substitution | Konsisten API | "Apakah variant ini bisa menggantikan base tanpa break?" |
| **I** — Interface Segregation | Specific props | "Apakah component terima props yang tidak dia butuhkan?" |
| **D** — Dependency Inversion | Abstraction | "Apakah component langsung import implementation?" |
