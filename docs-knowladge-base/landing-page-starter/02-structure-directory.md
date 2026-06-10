# Structure & Directory

> **Referensi:** Cara ngoding tiap file mengikuti prinsip SOLID — lihat [09-solid-coding-guide.md](./09-solid-coding-guide.md)
>
> - **SRP** → Setiap file punya satu tanggung jawab (components/sections/ui dipisah)
> - **OCP** → Data content di `lib/constants.ts`, bukan hardcoded di component
> - **DIP** → Abstraction layer di `lib/services.ts`, implementation di file terpisah

## Folder Structure

```
landing-page/
├── public/
│   ├── images/
│   │   ├── hero/
│   │   ├── logos/
│   │   ├── testimonials/
│   │   └── icons/
│   ├── fonts/
│   └── favicon.ico
├── src/
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── globals.css
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Navbar.tsx
│   │   │   ├── Footer.tsx
│   │   │   └── Container.tsx
│   │   ├── sections/
│   │   │   ├── HeroSection.tsx
│   │   │   ├── FeaturesSection.tsx
│   │   │   ├── PricingSection.tsx
│   │   │   ├── TestimonialsSection.tsx
│   │   │   ├── FAQSection.tsx
│   │   │   └── CTASection.tsx
│   │   └── ui/
│   │       ├── Button.tsx
│   │       ├── Card.tsx
│   │       ├── Badge.tsx
│   │       └── Accordion.tsx
│   ├── lib/
│   │   ├── constants.ts
│   │   └── utils.ts
│   ├── hooks/
│   │   └── useScroll.ts
│   └── types/
│       └── index.ts
├── tailwind.config.ts
├── next.config.js
├── package.json
├── tsconfig.json
└── .env.local
```

## Naming Convention
- **Components:** PascalCase (`HeroSection.tsx`)
- **Hooks:** camelCase dengan prefix `use` (`useScroll.ts`)
- **Utils:** camelCase (`formatDate.ts`)
- **Constants:** UPPER_SNAKE_CASE
- **CSS classes:** kebab-case via Tailwind
