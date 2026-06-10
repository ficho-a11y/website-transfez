# Animation & Interactions

## Animation Library
- **Framer Motion** untuk animasi komplex
- **CSS transitions** untuk simple hover/focus
- **Intersection Observer** untuk scroll-triggered animations

## Animation Guidelines

### Principles
1. **Purposeful** - setiap animasi punya tujuan
2. **Subtle** - tidak berlebihan, 200-500ms duration
3. **Consistent** - timing dan easing yang sama throughout
4. **Performant** - hanya animasi `transform` dan `opacity`

### Standard Timing
| Type | Duration | Easing |
|------|----------|--------|
| Micro (hover, focus) | 150ms | ease |
| Small (toggle, expand) | 200ms | ease-in-out |
| Medium (slide, fade) | 300-500ms | ease-out |
| Large (page transition) | 500-700ms | ease-in-out |

---

## Scroll Animations

### Fade Up (Default section entrance)
```
Initial: opacity 0, translateY 20px
Animate: opacity 1, translateY 0
Trigger: when in viewport
```

### Stagger Children (Cards grid)
```
Parent: staggerChildren: 0.1
Children: fade up dengan delay 0.1s antar item
```

### Scale In (Icons/images)
```
Initial: opacity 0, scale 0.8
Animate: opacity 1, scale 1
```

---

## Hover Effects

### Buttons
- Primary: brightness increase + slight translateY(-1px) + shadow increase
- Outline: background fill + color change
- Ghost: background opacity

### Cards
- translateY(-4px) + shadow increase
- Transition: 300ms ease-out

### Links
- Underline animation (width dari 0 ke 100%)
- Color transition

### Images
- Slight scale(1.02) on hover
- Transition: 500ms ease-out

---

## Interactive Elements

### Navbar
- Scroll: background opacity transition (transparent → blur)
- Mobile menu: slide down animation
- Active link: indicator animation

### Accordion (FAQ)
- Chevron: rotate 180deg
- Content: height animation (AnimatePresence)
- Stagger: sequential open animation

### Pricing Toggle
- Monthly/Yearly: smooth slide toggle
- Price: counter animation atau fade swap
- Badge: scale pop

### Dark Mode Toggle
- Icon: rotate + swap (sun ↔ moon)
- Colors: transition 300ms pada semua warna
