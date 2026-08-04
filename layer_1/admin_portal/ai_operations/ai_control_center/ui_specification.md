# 📄 UI Timing & Animation
> **Research and compile instructions before starting development.**

### 📋 What to put inside this document (1-4 line guideline):
* Document exact UI components, design system tokens (colors, spacing, typography), hover/active states, and UI animations. Detail GSAP/Framer Motion timing curves, duration constraints, and scroll trigger rules.

# UI Timing & Animation

> **Research and compile UI animation requirements before starting development.**

**Layer:** Layer 1 – Experience Layer

**Module:** Admin Portal

**Submodule:** AI Control Center

**Document Version:** 1.0

**Status:** Active

---

# Purpose

This document defines the UI timing, animation specifications, design system tokens, interaction states, and motion guidelines for the AI Control Center. It ensures a consistent, accessible, and responsive user experience across all administrative interfaces while maintaining enterprise-grade usability.

---

# Scope

This specification applies to:

- Dashboard
- Navigation
- AI Model Management
- Prompt Management
- Analytics
- Monitoring
- Modals
- Tables
- Forms
- Notifications
- Charts
- Buttons
- Dropdowns

---

# Design Principles

The animation system should:

- Feel smooth and responsive.
- Never delay user actions.
- Provide meaningful visual feedback.
- Guide user attention.
- Reduce perceived loading time.
- Follow accessibility guidelines.
- Maintain consistency across the application.

---

# Design System Tokens

## Color Palette

| Token | Value | Usage |
|--------|-------|------|
| Primary | #2563EB | Primary actions |
| Secondary | #6366F1 | Secondary actions |
| Success | #16A34A | Success states |
| Warning | #F59E0B | Warning alerts |
| Error | #DC2626 | Error states |
| Info | #0EA5E9 | Informational messages |
| Background | #0F172A | Application background |
| Surface | #1E293B | Cards & Panels |
| Border | #334155 | Borders |
| Text Primary | #F8FAFC | Main text |
| Text Secondary | #CBD5E1 | Secondary text |

---

## Spacing Scale

| Token | Value |
|--------|-------|
| xs | 4px |
| sm | 8px |
| md | 16px |
| lg | 24px |
| xl | 32px |
| xxl | 48px |

---

## Border Radius

| Token | Value |
|--------|-------|
| Small | 4px |
| Medium | 8px |
| Large | 12px |
| Extra Large | 16px |

---

## Shadow Tokens

| Token | Usage |
|--------|------|
| Shadow XS | Buttons |
| Shadow SM | Cards |
| Shadow MD | Hover Cards |
| Shadow LG | Dialogs |
| Shadow XL | Large Modals |

---

## Typography

| Element | Font | Size | Weight |
|----------|------|------|--------|
| Heading 1 | Inter | 36px | 700 |
| Heading 2 | Inter | 30px | 700 |
| Heading 3 | Inter | 24px | 600 |
| Heading 4 | Inter | 20px | 600 |
| Body | Inter | 16px | 400 |
| Small | Inter | 14px | 400 |
| Caption | Inter | 12px | 400 |

---

# Component States

## Buttons

| State | Behavior |
|---------|----------|
| Default | Primary background |
| Hover | Slight elevation + darker background |
| Active | Scale to 98% |
| Focus | Blue focus ring |
| Disabled | 50% opacity |
| Loading | Spinner + disabled |

---

## Input Fields

| State | Behavior |
|---------|----------|
| Default | Neutral border |
| Hover | Border highlight |
| Focus | Primary border + focus ring |
| Error | Red border |
| Disabled | Reduced opacity |

---

## Cards

| State | Behavior |
|---------|----------|
| Default | Shadow SM |
| Hover | Shadow MD |
| Selected | Primary border |
| Disabled | Reduced opacity |

---

# Hover & Active States

| Component | Hover | Active |
|-----------|--------|--------|
| Button | Elevation + Color Change | Scale 0.98 |
| Card | Shadow Increase | Border Highlight |
| Navigation Item | Background Highlight | Active Indicator |
| Table Row | Background Highlight | Selected State |
| Dropdown Item | Background Color | Selected Color |

---

# Animation Standards

Animations should remain subtle and responsive.

| Interaction | Duration | Easing |
|-------------|----------|--------|
| Button Hover | 150ms | ease-out |
| Button Click | 100ms | ease-in-out |
| Card Hover | 180ms | ease-out |
| Modal Open | 250ms | ease-out |
| Modal Close | 200ms | ease-in |
| Sidebar Toggle | 250ms | ease-in-out |
| Dropdown Open | 180ms | ease-out |
| Tooltip | 120ms | ease-out |
| Toast Notification | 300ms | ease-out |
| Page Transition | 300ms | ease-in-out |

---

# Framer Motion Guidelines

Recommended transitions:

```tsx
transition={{
  duration: 0.3,
  ease: "easeInOut"
}}
```

### Standard Durations

| Animation | Duration |
|------------|----------|
| Micro Interaction | 100–150ms |
| Hover Animation | 150–200ms |
| Component Transition | 200–300ms |
| Modal Animation | 250ms |
| Page Transition | 300ms |
| Loading Animation | Continuous |

---

# GSAP Guidelines

For advanced animations:

### Default Ease

```text
power2.out
```

### Standard Durations

| Animation | Duration |
|------------|----------|
| Fade In | 0.3s |
| Slide In | 0.4s |
| Scale In | 0.25s |
| Timeline Animation | ≤ 1.2s |

---

# Page Transition

```text
Current Page
      │
      ▼
Fade Out (150ms)
      │
      ▼
Load Next Route
      │
      ▼
Fade In (150ms)
```

---

# Loading Animations

Use:

- Skeleton loaders
- Circular spinners
- Progress bars
- Pulse animations

Avoid:

- Blocking animations
- Long looping transitions

---

# Dashboard Animations

Dashboard widgets should:

- Fade in on load
- Count up statistics
- Animate charts progressively
- Highlight refreshed metrics

---

# Table Animations

- Smooth pagination
- Row fade-in
- Expand/collapse transitions
- Loading skeleton rows

---

# Modal Animations

Opening:

- Fade overlay
- Scale dialog from 95% → 100%

Closing:

- Fade out
- Scale to 95%

Duration:

250ms

---

# Notification Animations

Toast notifications:

- Slide from top-right
- Fade in
- Auto dismiss after 3–5 seconds

---

# Sidebar Animation

Open:

- Slide from left
- Duration: 250ms

Close:

- Reverse slide
- Duration: 200ms

---

# Chart Animations

Charts should:

- Animate bars progressively
- Fade legends
- Animate line graphs
- Count numeric values

Maximum duration:

800ms

---

# Scroll Trigger Rules

Animate elements only when they enter the viewport.

Recommended triggers:

- Dashboard cards
- Analytics charts
- Tables
- Knowledge Base items

Do not animate:

- Navigation
- Frequently refreshed widgets
- Forms during input

---

# Animation Constraints

- Total page animation ≤ 500ms
- Individual animation ≤ 300ms
- No layout shifts during animation
- Maintain 60 FPS
- Avoid simultaneous heavy animations

---

# Accessibility

Animations should:

- Respect **prefers-reduced-motion**
- Allow users to disable motion
- Avoid flashing effects
- Avoid motion that causes discomfort
- Maintain visible focus indicators

---

# Performance Guidelines

- Use CSS transforms instead of changing layout properties.
- Prefer `transform` and `opacity`.
- Avoid animating width or height.
- Lazy-load animation libraries.
- Use hardware acceleration where appropriate.
- Minimize repaint and reflow operations.

---

# Animation Checklist

- ☐ Consistent easing functions
- ☐ Responsive interaction timings
- ☐ Accessible motion
- ☐ Optimized performance
- ☐ Smooth page transitions
- ☐ No layout shifts
- ☐ Framer Motion configured
- ☐ GSAP timelines optimized

---

# Success Criteria

The UI animation system is considered complete when:

- All interactive components follow defined timing standards.
- Motion enhances usability without distraction.
- Animations remain smooth at 60 FPS.
- Accessibility guidelines are met.
- Performance targets are maintained.
- UI behavior is consistent across all screens.

---

# Related Documents

- overview.md
- interaction_design_spec.md
- interactive_state_flow.md
- architecture.md
- implementation_plan.md
- requirements.md
- performance.md
- ux_specification.md
- ui_specification.md

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | Initial Release | Initial UI Timing & Animation specification for the AI Control Center |