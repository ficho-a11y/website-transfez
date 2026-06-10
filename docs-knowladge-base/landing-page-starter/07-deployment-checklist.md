# Deployment Checklist

## Pre-Launch

### Content
- [ ] Semua copy final (no lorem ipsum)
- [ ] Semua images optimized (WebP, compressed)
- [ ] Links semua berfungsi
- [ ] Form/CTA terkoneksi ke backend/email service
- [ ] Legal pages (Privacy Policy, Terms) jika perlu

### SEO
- [ ] Meta tags lengkap
- [ ] Open Graph image (1200x630px)
- [ ] Favicon & app icons
- [ ] Sitemap.xml generated
- [ ] Robots.txt configured
- [ ] Google Analytics / tracking installed

### Performance
- [ ] Lighthouse score > 90 semua kategori
- [ ] Images lazy loaded
- [ ] Fonts optimized (preload, display: swap)
- [ ] Bundle size < 200KB (first load JS)
- [ ] No render-blocking resources

### Testing
- [ ] Tested di Chrome, Firefox, Safari, Edge
- [ ] Tested di iOS Safari, Android Chrome
- [ ] Responsive di semua breakpoints
- [ ] Dark mode berfungsi
- [ ] Keyboard navigation works
- [ ] Screen reader compatible
- [ ] 404 page exists
- [ ] No console errors

---

## Deployment

### Environment
- [ ] Production env variables set
- [ ] API endpoints pointing to production
- [ ] Domain DNS configured
- [ ] SSL certificate active (HTTPS)

### Platform (Vercel)
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Deploy to production
vercel --prod
```

### Post-Deploy
- [ ] Verify site loads di production URL
- [ ] Test semua CTAs dan forms
- [ ] Check Google Search Console
- [ ] Submit sitemap ke Google
- [ ] Set up uptime monitoring
- [ ] Test page speed (PageSpeed Insights)
- [ ] Share URL ke team untuk final review

---

## Monitoring
- [ ] Error tracking (Sentry / Vercel)
- [ ] Analytics dashboard
- [ ] Performance monitoring
- [ ] Uptime alerts
