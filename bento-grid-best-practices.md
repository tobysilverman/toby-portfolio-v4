# Bento Grid Implementation Guide

## Quick Start Pattern

```html
<!-- Base Structure -->
<div class="grid grid-cols-1 md:grid-cols-4 lg:grid-cols-12 gap-4 md:gap-6 auto-rows-[192px]">
  <div class="col-span-1 md:col-span-2 lg:col-span-6 md:row-span-2">Featured</div>
  <div class="col-span-1 md:col-span-2 lg:col-span-3">Card 2</div>
  <div class="col-span-1 md:col-span-2 lg:col-span-3">Card 3</div>
  <!-- Add more cards -->
</div>
```

## Core CSS Grid Setup

### Foundation (Always Use This)
```css
.bento-grid {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr)); /* 12-column base */
  grid-auto-rows: minmax(192px, auto); /* Flexible height with minimum */
  gap: 1rem; /* 16px default gap */
  grid-auto-flow: dense; /* Tight packing algorithm */
}
```

### Responsive Breakpoints
```css
/* Mobile First */
@media (min-width: 768px) {
  .bento-grid {
    grid-template-columns: repeat(4, 1fr);
    gap: 1.5rem;
  }
}

@media (min-width: 1024px) {
  .bento-grid {
    grid-template-columns: repeat(12, minmax(0, 1fr));
    gap: 2rem;
  }
}
```

## Tailwind Implementation

### Essential Classes
```jsx
// Container
<div className="
  grid 
  grid-cols-1 md:grid-cols-4 lg:grid-cols-12
  gap-4 md:gap-6
  auto-rows-[192px]
">

// Cards - Common Patterns
<div className="col-span-1 md:col-span-2 lg:col-span-6 md:row-span-2"> // Large featured
<div className="col-span-1 md:col-span-2 lg:col-span-4"> // Medium
<div className="col-span-1 md:col-span-1 lg:col-span-3"> // Small
```

### Card Styling
```jsx
<div className="
  bg-white dark:bg-gray-800 
  rounded-lg 
  p-6 md:p-8
  shadow-sm hover:shadow-md
  transition-shadow duration-300
  border border-gray-200 dark:border-gray-700
">
```

## React Component Pattern

```jsx
const BentoGrid = ({ children }) => (
  <div className="grid grid-cols-1 md:grid-cols-4 lg:grid-cols-12 gap-4 md:gap-6 auto-rows-[192px]">
    {children}
  </div>
);

const BentoCard = ({ size = "medium", rowSpan = 1, children }) => {
  const sizeClasses = {
    small: "col-span-1 md:col-span-1 lg:col-span-3",
    medium: "col-span-1 md:col-span-2 lg:col-span-4",
    large: "col-span-1 md:col-span-2 lg:col-span-6",
    full: "col-span-1 md:col-span-4 lg:col-span-12"
  };

  return (
    <div className={`
      ${sizeClasses[size]} 
      ${rowSpan > 1 ? `md:row-span-${rowSpan}` : ''}
      bg-white dark:bg-gray-800 
      rounded-lg p-6 
      shadow-sm hover:shadow-md 
      transition-all duration-300
    `}>
      {children}
    </div>
  );
};
```

## Performance Essentials

### Lazy Loading with Intersection Observer
```jsx
import { useInView } from 'react-intersection-observer';

const LazyBentoCard = ({ children, ...props }) => {
  const { ref, inView } = useInView({
    threshold: 0.1,
    triggerOnce: true,
    rootMargin: '50px'
  });

  return (
    <div ref={ref}>
      {inView ? <BentoCard {...props}>{children}</BentoCard> : <Skeleton />}
    </div>
  );
};
```

### Image Optimization (Next.js)
```jsx
import Image from 'next/image';

<Image
  src={imageSrc}
  alt={alt}
  fill
  className="object-cover"
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
  loading="lazy"
/>
```

### Animation Rules
```css
/* ONLY animate these properties */
.card:hover {
  transform: translateY(-4px) scale(1.02);
  opacity: 0.95;
}

/* NEVER animate width, height, margin, padding */
/* Use transform for position changes */
/* Use opacity for visibility changes */
```

## Accessibility Checklist

### DOM Order = Visual Order
```jsx
// ❌ WRONG - Visual reordering
<div style={{ order: 2 }}>First in DOM</div>
<div style={{ order: 1 }}>Second in DOM</div>

// ✅ CORRECT - Maintain source order
<div>First visually and in DOM</div>
<div>Second visually and in DOM</div>
```

### Focus Management
```css
.card:focus-visible {
  outline: 2px solid #3B82F6;
  outline-offset: 2px;
}
```

### Interactive Cards
```jsx
// Make entire card clickable with pseudo-element
<div className="relative group">
  <a href={url} className="absolute inset-0 z-10" aria-label={title}>
    <span className="sr-only">{title}</span>
  </a>
  {/* Card content */}
</div>
```

## Common Patterns

### Portfolio Layout
```jsx
<BentoGrid>
  <BentoCard size="large" rowSpan={2}>Main Project</BentoCard>
  <BentoCard size="medium">About</BentoCard>
  <BentoCard size="medium">Skills</BentoCard>
  <BentoCard size="small">Project 2</BentoCard>
  <BentoCard size="small">Project 3</BentoCard>
  <BentoCard size="small">Project 4</BentoCard>
  <BentoCard size="full">Contact</BentoCard>
</BentoGrid>
```

### Dark Mode
```jsx
// Tailwind approach
<div className="bg-white dark:bg-gray-900 text-gray-900 dark:text-white">

// CSS variables approach
:root {
  --card-bg: white;
  --card-text: #111827;
}
.dark {
  --card-bg: #1F2937;
  --card-text: white;
}
```

## Critical Rules

1. **Grid Setup**: Always use 12-column base for maximum flexibility
2. **Responsive**: Mobile-first with 768px and 1024px breakpoints
3. **Performance**: Lazy load below fold, only animate transform/opacity
4. **Images**: Use Next.js Image or picture element with WebP/AVIF
5. **Accessibility**: Maintain DOM order, 44px minimum touch targets
6. **Spacing**: Use 8pt grid (8px, 16px, 24px, 32px, 40px, 48px)
7. **Animation**: 300-500ms transitions with ease-in-out
8. **Typography**: 24-32px headlines, 14-16px body, 1.5-1.6 line-height

## Container Query Support (2024+)

```css
@container (min-width: 400px) {
  .card { grid-template-columns: 1fr 1fr; }
}
```

```jsx
// Tailwind
<div className="@container">
  <div className="grid grid-cols-1 @sm:grid-cols-2 @lg:grid-cols-3">
```

## Quick Debug Checklist

- [ ] Grid uses `minmax(0, 1fr)` to prevent content overflow
- [ ] Cards have `overflow: hidden` for image containment  
- [ ] Touch targets are 44×44px minimum
- [ ] Focus indicators meet 3:1 contrast ratio
- [ ] Images have appropriate loading strategy
- [ ] Animations use transform/opacity only
- [ ] Dark mode has sufficient contrast (WCAG AAA)
- [ ] Mobile layout stacks to single column
- [ ] No horizontal scrolling on mobile
