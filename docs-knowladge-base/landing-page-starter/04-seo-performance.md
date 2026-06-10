# SEO & Performance

## SEO Checklist

### Meta Tags
- [ ] `<title>` - max 60 karakter, mengandung keyword utama
- [ ] `<meta name="description">` - max 160 karakter
- [ ] Open Graph tags (og:title, og:description, og:image, og:url)
- [ ] Twitter Card tags
- [ ] Canonical URL

### Structured Data
- [ ] JSON-LD schema (Organization, WebPage)
- [ ] Breadcrumb schema (jika ada)
- [ ] FAQ schema (untuk FAQ section)

### Technical SEO
- [ ] Semantic HTML (header, main, section, article, footer)
- [ ] Heading hierarchy (H1 > H2 > H3, hanya 1 H1)
- [ ] Alt text pada semua images
- [ ] Internal linking
- [ ] Robots.txt
- [ ] Sitemap.xml

### Content SEO
- [ ] Keyword research done
- [ ] Primary keyword: _
- [ ] Secondary keywords: _, _, _
- [ ] Keyword di H1, meta description, first 100 words
- [ ] URL slug yang SEO-friendly

---

## Performance Checklist

### Core Web Vitals Target
| Metric | Target | Current |
|--------|--------|---------|
| LCP (Largest Contentful Paint) | < 2.5s | |
| FID (First Input Delay) | < 100ms | |
| CLS (Cumulative Layout Shift) | < 0.1 | |
| INP (Interaction to Next Paint) | < 200ms | |

### Image Optimization
- [ ] Format WebP/AVIF
- [ ] Lazy loading (`loading="lazy"`)
- [ ] Responsive images (`srcset`)
- [ ] Width & height attributes (prevent CLS)
- [ ] Hero image: preload dengan `<link rel="preload">`

### Bundle Optimization
- [ ] Dynamic imports untuk heavy components
- [ ] Tree shaking
- [ ] Font optimization (`next/font`)
- [ ] Minimal third-party scripts
- [ ] Compress assets (gzip/brotli)

### Caching
- [ ] Static assets: cache header 1 year
- [ ] HTML: revalidate strategy
- [ ] CDN configuration

### Lighthouse Target
| Category | Target Score |
|----------|-------------|
| Performance | > 90 |
| Accessibility | > 95 |
| Best Practices | > 90 |
| SEO | > 90 |
