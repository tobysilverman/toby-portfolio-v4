# Portfolio Architecture Reference

Quick reference for the portfolio bento grid implementation.

## Tech Stack
- **HTML**: Static files only
- **CSS**: Tailwind CSS via CDN
- **Fonts**: Bricolage Grotesque (display), IBM Plex Sans (body)
- **No build tools**: Pure static files

## Responsive Breakpoints

| Breakpoint | Width | Layout | Grid |
|------------|-------|--------|------|
| Mobile | < 768px | Stacked (text → grid) | 1 column, 280px rows |
| Tablet (md) | 768px+ | Stacked (text → grid) | 3 columns @ 160px |
| Desktop (xl) | 1280px+ | Side-by-side (text \| grid) | 4 columns @ minmax(180px, 1fr) |

## Layout Structure

### Desktop (xl: 1280px+)
```
┌─────────────────────────────────────────────┐
│ [48px margin]                               │
│   ┌──────────┬──────────────────────┐       │
│   │  Name    │   Bento Grid         │       │
│   │  +       │   (fluid scaling)    │       │
│   │  Bio     │                      │       │
│   └──────────┴──────────────────────┘       │
│                              [48px margin]  │
└─────────────────────────────────────────────┘
```

### Mobile/Tablet
```
┌─────────────┐
│ [24px pad]  │
│   Name      │
│   Bio       │
│             │
│   Card 1    │
│   Card 2    │
│   ...       │
│ [24px pad]  │
└─────────────┘
```

## Typography Scale

| Element | Mobile | Tablet (md) | Desktop (xl) |
|---------|--------|-------------|--------------|
| Name | 40px | 52px | 64px |
| Body | 16px | 18px | 20px |
| Line height | 1.09 (name) | 1.09 | 1.09 |
| Body line-height | 1.61 | 1.61 | 1.61 |

## Bento Grid Configuration

### Grid Container Classes
```css
/* Mobile */
grid-cols-1
auto-rows-[280px]
gap-[8px]

/* Tablet (md) */
md:grid-cols-[160px_160px_160px]
md:grid-rows-[160px_160px_160px_160px]

/* Desktop (xl) */
xl:grid-cols-[minmax(180px,1fr)_minmax(180px,1fr)_minmax(180px,1fr)_minmax(180px,1fr)]
xl:grid-rows-[1fr_1fr_1fr]
xl:flex-1 xl:h-full
```

### Card Configurations

| Card | Type | Tablet (md) | Desktop (xl) |
|------|------|-------------|--------------|
| 1 | Tall (1x2) | col-1, row-1-2 | col-1, row-1-2 |
| 2 | Square (1x1) | col-2, row-1 | col-2, row-1 |
| 3 | Wide (2x1) | col-2-3, row-2 | col-3-4, row-1 |
| 4 | Square (1x1) | col-1, row-3 | col-1, row-3 |
| 5 | Large (2x2) | col-2-3, row-3-4 | col-2-3, row-2-3 |
| 6 | Square (1x1) | col-3, row-1 | col-4, row-2 |
| 7 | Square (1x1) | col-1, row-4 | col-4, row-3 |

### Card Styling
```css
bg-white
border border-[#E8E3DB]
rounded-[16px]
relative
overflow-hidden
p-4 /* 16px padding for images */
```

## Image Handling
- **Fit**: `object-contain` (no cropping, letterboxing allowed)
- **Size**: `w-full h-full`
- **Padding**: 16px (p-4) around images in cards

## Key Design Decisions

### 1. XL Breakpoint for Desktop
Desktop layout uses `xl:` (1280px) instead of `lg:` (1024px) to prevent narrow cards before transitioning to tablet layout.

### 2. Fluid Scaling with Constraints
```css
xl:grid-cols-[minmax(180px,1fr)...]
```
Columns won't shrink below 180px, preventing aspect ratio distortion.

### 3. Single-Column Mobile
Clean vertical scroll with uniform 280px cards. Easier to scan, predictable layout.

### 4. No Image Cropping
`object-contain` preserves full images with letterboxing vs `object-cover` which crops.

## Container Positioning

### Desktop
```css
xl:absolute
xl:left-[48px]
xl:top-[38px]
xl:bottom-[38px]
xl:right-[48px]
```

### Mobile/Tablet
```css
p-[24px] /* mobile */
md:p-[32px] /* tablet */
```

## Color Palette
- Background: `#FDFBF7` (cream)
- Card bg: `#FFFFFF` (white)
- Border: `#E8E3DB` (light warm gray)
- Text: `#4A4540` (dark warm gray)
- Links: `#1886E2` (electric blue)

## File Structure
```
/
├── index.html          # Main portfolio page
├── styles.css          # Custom styles
├── CLAUDE.md           # Project guidelines
├── architecture.md     # This file
├── bento-grid-best-practices.md
└── design-system/
    └── design_system.md
```

## Quick Modifications

### Change card height (mobile)
```css
auto-rows-[280px] → auto-rows-[320px]
```

### Change minimum column width (desktop)
```css
minmax(180px,1fr) → minmax(200px,1fr)
```

### Add hover effect to cards
```css
transition-transform duration-300 hover:scale-[1.02]
```

### Change image fit to cropping
```css
object-contain → object-cover
```
