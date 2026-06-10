# Responsive Breakpoints

## Breakpoint Definition (Tailwind Default)

| Breakpoint | Min Width | Device |
|------------|-----------|--------|
| `sm` | 640px | Mobile landscape |
| `md` | 768px | Tablet |
| `lg` | 1024px | Laptop |
| `xl` | 1280px | Desktop |
| `2xl` | 1536px | Large Desktop |

## Section Responsiveness Guide

### Navbar
| Breakpoint | Behavior |
|------------|----------|
| Mobile | Hamburger menu, logo + CTA only |
| `md+` | Full horizontal nav |

### Hero
| Breakpoint | Layout |
|------------|--------|
| Mobile | Stacked (text → image), `text-4xl` |
| `md` | Stacked, `text-5xl` |
| `lg+` | Side-by-side, `text-6xl` |

### Features Grid
| Breakpoint | Columns |
|------------|---------|
| Mobile | 1 column |
| `sm` | 2 columns |
| `lg` | 3 columns |

### Pricing
| Breakpoint | Columns |
|------------|---------|
| Mobile | 1 column, scroll horizontal |
| `md` | 3 columns |

### Testimonials
| Breakpoint | Columns |
|------------|---------|
| Mobile | 1 column (carousel) |
| `md` | 2 columns |
| `lg` | 3 columns |

### Footer
| Breakpoint | Columns |
|------------|---------|
| Mobile | Stacked, 1 column |
| `md` | 2 columns |
| `lg` | 4 columns |

## Mobile-First Principle
```
// BENAR - mobile first
class="text-sm md:text-base lg:text-lg"

// SALAH - desktop first
class="text-lg lg:text-base"
```

## Touch Target
- Minimum touch target: 44x44px
- Button padding: minimal `py-2.5 px-5`
- Gap antar interactive elements: minimal `gap-2`
