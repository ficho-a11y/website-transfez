# Components Reference

> **Referensi:** Cara bikin component mengikuti prinsip SOLID — lihat [09-solid-coding-guide.md](./09-solid-coding-guide.md)
>
> | Principle | Relevansi di Component |
> |-----------|----------------------|
> | **SRP** | 1 component = 1 job. Jangan gabung Navbar + Hero di satu file |
> | **OCP** | Component menerima data via props. Tambah data baru = tidak perlu edit component |
> | **LSP** | Variant (primary/outline/ghost) harus konsisten behavior — lihat contoh `Button.tsx` |
> | **ISP** | Setiap section punya props interface sendiri. Jangan pakai satu giant props |
> | **DIP** | Service (API, newsletter) di-inject via props, bukan hardcoded import |

## Layout Components

### Navbar
- Logo (kiri)
- Navigation links (tengah)
- CTA Button (kanan)
- Mobile: hamburger menu
- Sticky on scroll dengan backdrop blur
- Transparency saat di hero

### Footer
- Logo + deskripsi singkat
- Link navigasi (kolom)
- Social media icons
- Copyright
- Optional: newsletter signup

### Container
- `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8`

---

## Section Components

### Hero Section
- Headline (H1)
- Subheadline
- CTA Button (primary)
- Secondary link/button
- Hero image / illustration / video
- Background: gradient atau solid

### Features Section
- Section title + subtitle
- Grid layout (3 kolom desktop, 1 kolom mobile)
- Feature card: icon + title + description

### Pricing Section
- Section title + subtitle
- 3 tier cards (Basic, Pro, Enterprise)
- Highlight recommended plan
- Toggle monthly/yearly billing
- CTA per card

### Testimonials Section
- Section title
- Grid / carousel layout
- Testimonial card: avatar + nama + role + quote + rating

### FAQ Section
- Section title
- Accordion component
- Expand/collapse per item

### CTA Section
- Background gradient/warna
- Headline
- Subheadline
- CTA Button

---

## UI Components

### Button
Variants: `primary`, `secondary`, `outline`, `ghost`
Sizes: `sm`, `md`, `lg`
States: `default`, `hover`, `focus`, `disabled`, `loading`

### Card
- Padding: `p-6`
- Rounded: `rounded-xl`
- Shadow: `shadow-sm` → `shadow-md` on hover
- Border: `border border-gray-200`

### Badge
- Variants: `default`, `success`, `warning`, `error`
- Size: `text-xs px-2.5 py-0.5 rounded-full`

### Accordion
- Click to expand/collapse
- Chevron animation
- Smooth height transition
