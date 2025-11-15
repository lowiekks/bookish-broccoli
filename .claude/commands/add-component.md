---
description: "Create a new UI component following design system patterns"
argument-hint: [component-name]
---

Create a new UI component **$1** following YAICO design system patterns in @CLAUDE.md.

**1. Component File**

File: `src/components/ui/$1.tsx`

Follow patterns from existing UI components in @src/components/ui/:
- Use TypeScript with proper prop interfaces
- Forward refs for composition (`React.forwardRef`)
- Support variants (using `clsx` or similar)
- Include accessibility attributes (ARIA)
- Use Tailwind CSS for styling
- Follow color scheme from @PROJECT_CONTEXT.md:
  - Primary: Blue (#3B82F6)
  - Success: Mint Green (#00CC99 - yaico-mint)
  - Error: Red (#EF4444)

**2. Component Interface**

```typescript
export interface ${1}Props extends React.HTMLAttributes<HTMLElement> {
  variant?: 'primary' | 'secondary' | 'danger' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  // ... other props
}
```

**3. Features to Include**

- [ ] Multiple variants (at least primary/secondary)
- [ ] Size variants (sm/md/lg)
- [ ] Disabled state
- [ ] Loading state (if interactive)
- [ ] Hover and focus states
- [ ] Mobile responsive
- [ ] Keyboard navigation support
- [ ] Screen reader friendly

**4. Export**

Add to `src/components/ui/index.ts`:
```typescript
export { $1 } from './$1';
export type { ${1}Props } from './$1';
```

**5. Storybook/Example Usage**

Provide example usage:
```typescript
// Basic
<$1>Content</$1>

// With variants
<$1 variant="primary" size="lg">Primary</$1>
<$1 variant="secondary" size="sm">Secondary</$1>

// Disabled
<$1 disabled>Disabled</$1>
```

Create the complete component following these specifications.
