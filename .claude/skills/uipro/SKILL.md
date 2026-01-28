# UI/UX Pro Skill

This skill provides guidance for user interface design, user experience patterns, and visual design best practices.

## When to Use This Skill

**IMPORTANT**: Claude should automatically reference this skill whenever the conversation involves:

- User interface (UI) design or implementation
- User experience (UX) patterns or flows
- Visual design decisions (colors, typography, spacing, layout)
- Component design or styling
- Accessibility (a11y) considerations
- Responsive design or mobile layouts
- Form design and validation UX
- Navigation patterns
- Loading states, empty states, error states
- Animations and micro-interactions
- Design systems or component libraries
- CSS, Tailwind, or styling frameworks
- Frontend frameworks (Vue, React, Svelte, etc.) UI concerns

## Trigger Keywords

When the user mentions any of these topics, consult this skill:

- "design", "UI", "UX", "interface", "layout"
- "component", "button", "form", "modal", "dialog"
- "style", "styling", "CSS", "Tailwind", "colors", "theme"
- "responsive", "mobile", "desktop", "breakpoint"
- "accessibility", "a11y", "screen reader", "ARIA"
- "animation", "transition", "loading", "spinner"
- "navigation", "menu", "sidebar", "header", "footer"
- "typography", "font", "spacing", "padding", "margin"
- "user flow", "user journey", "onboarding"
- "empty state", "error state", "success state"
- "visual design", "look and feel", "modern", "clean"

## Using UI/UX Pro CLI

The project has UI/UX Pro CLI installed. Use these commands to get design guidance:

### Check Design System
```bash
uipro check
```
Reviews the project's design consistency and identifies issues.

### Get Component Patterns
```bash
uipro patterns <component-type>
```
Examples:
```bash
uipro patterns button
uipro patterns form
uipro patterns modal
uipro patterns table
uipro patterns card
```

### Accessibility Audit
```bash
uipro a11y <file>
```
Checks a component for accessibility issues.

### Color Suggestions
```bash
uipro colors <base-color>
```
Generates accessible color palettes based on a primary color.

### Spacing System
```bash
uipro spacing
```
Shows the recommended spacing scale for the project.

## Design Principles

When giving UI/UX advice, follow these principles:

### 1. Clarity Over Cleverness
- Use familiar patterns users already understand
- Prioritize readability and scannability
- Make interactive elements obvious

### 2. Consistency
- Maintain consistent spacing, colors, and typography
- Use the same patterns for similar actions
- Follow established design system conventions

### 3. Feedback
- Provide immediate feedback for user actions
- Show loading states for async operations
- Confirm destructive actions

### 4. Accessibility First
- Ensure sufficient color contrast (WCAG 2.1 AA minimum)
- Support keyboard navigation
- Use semantic HTML elements
- Include ARIA labels where needed

### 5. Mobile-First
- Design for smallest screens first
- Use touch-friendly tap targets (44px minimum)
- Consider thumb zones for mobile interaction

## Common Patterns

### Button Hierarchy
```
Primary   → Main action (1 per section max)
Secondary → Alternative actions
Tertiary  → Less important actions
Danger    → Destructive actions (red)
```

### Form Validation
```
- Validate on blur, not on change (less intrusive)
- Show errors inline near the field
- Use supportive, not accusatory language
- Indicate required fields clearly
```

### Loading States
```
- Skeleton screens for content loading
- Spinners for actions (button loading)
- Progress bars for known duration
- Optimistic UI for fast operations
```

### Empty States
```
- Explain what will appear here
- Provide a clear call-to-action
- Use friendly, encouraging language
- Include helpful illustration if appropriate
```

## Integration with Branch Flow

When creating specs or plans that involve UI work:

1. **In /bf:spec**: Include UI/UX requirements
   - User flow descriptions
   - Accessibility requirements
   - Responsive breakpoints needed

2. **In /bf:plan**: Reference design patterns
   - Component structure
   - State management for UI
   - Animation/transition details

3. **In /bf:review**: Check UI quality
   - Visual consistency
   - Accessibility compliance
   - Responsive behavior

## Example Prompts

User: "Design a login form"
→ Consult this skill, use `uipro patterns form`, provide accessible, user-friendly design

User: "Make this button look better"
→ Consult this skill, review button hierarchy, suggest improvements

User: "The dashboard feels cluttered"
→ Consult this skill, apply whitespace principles, suggest information hierarchy

User: "How should I handle loading states?"
→ Consult this skill, recommend appropriate loading pattern for the context

## Resources

- UI/UX Pro Documentation: `uipro help`
- Project Design System: Check for `design-system.md` or similar
- Component Library: Check `package.json` for UI framework (Vuetify, Material-UI, etc.)
