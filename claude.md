# Project Guidelines

## Tech Stack
- **HTML**: Static files only
- **CSS**: Tailwind CSS via CDN + design system
- **No JavaScript**: Unless explicitly requested
- **No build tools**: Pure static files only

## Design System Reference
For typography, colors, components, and patterns:
→ `/design-system/design_system.md`

**Key principles:**
- 95% neutral, 5% accent color rule
- Bauhaus geometric simplicity
- Typography: Bricolage Grotesque (display), IBM Plex Sans (body), IBM Plex Mono (code)

## Core Principles

### 1. Pixel-Perfect Implementation
- Build EXACTLY what is shown in Figma
- Never add features or elements not shown
- If something seems missing, ASK before adding

### 2. Layout Strategy
- **Always Flexbox** for layouts (no absolute positioning)
- Use `gap-*` over margins for spacing
- CSS Grid only when explicitly beneficial
- Container setup: `flex flex-col` or `flex flex-row`

### 3. Development Workflow
1. Receive Figma design
2. Implement with Tailwind + design system
3. Verify with Chrome DevTools MCP
4. Iterate until pixel-perfect
5. Test responsive breakpoints: 375px, 768px, 1440px

## Implementation Standards

### HTML Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[Page Title]</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <!-- Apply design system classes -->
</body>
</html>
```

### Tailwind + Design System
- Use design system color variables: `--color-primary`, `--color-danger`, etc.
- Apply typography classes: `.h1`, `.body-base`, `.code-small`
- For exact Figma values use arbitrary: `w-[237px]`, `gap-[18px]`
- Component patterns from design system over custom solutions

### Chrome DevTools Verification
After implementation, verify:
- Exact spacing measurements
- Computed font sizes and line heights  
- Color values match design
- Responsive behavior

## What NOT to Do
❌ Create additional pages/sections  
❌ Add JavaScript without explicit request  
❌ Use absolute/fixed positioning (except overlays)  
❌ Implement "nice to have" features  
❌ Make assumptions about missing elements  

## Quality Checklist
- [ ] Matches Figma exactly
- [ ] Uses Flexbox layouts
- [ ] Follows design system patterns
- [ ] Verified with DevTools
- [ ] Responsive at key breakpoints
- [ ] No added embellishments

## Quick Reference
- **Spacing**: Use Tailwind scale or exact values from Figma
- **Colors**: Use design system variables (see `/design-system/design_system.md#color-system`)
- **Typography**: Use predefined scales (see `/design-system/design_system.md#typography`)
- **Components**: Reference patterns (see `/design-system/design_system.md#component-patterns`)

**Remember**: When in doubt, reference the design system or ask for clarification.