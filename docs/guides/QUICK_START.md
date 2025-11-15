# Quick Start Guide

This is a condensed version of the YAICO Vibe Code workflow for quick reference.

## Prerequisites
- Node.js 18+
- npm 9+
- VS Code with Claude extension
- Git

## Quick Setup (15 minutes)

### 1. Read Context First
```bash
# Always start by reading PROJECT_CONTEXT.md
cat PROJECT_CONTEXT.md
```

### 2. Initialize Project
```bash
# Use the exact vibe command from Phase 1 of the main guide
# See: YAICO_VIBE_WORKFLOW_GUIDE.md - Phase 1, Step 1.2
```

### 3. Test Immediately
```bash
npm run dev
# Visit http://localhost:3000
```

### 4. Commit Early, Commit Often
```bash
git add .
git commit -m "feat: [what you just built]"
```

## Phase Checklist

| Phase | Feature | Time | Status |
|-------|---------|------|--------|
| 1 | Project Init | 30m | [ ] |
| 2 | Authentication | 45m | [ ] |
| 3 | Data Layer | 40m | [ ] |
| 4 | AI Integration | 50m | [ ] |
| 5 | Dashboard & UI | 45m | [ ] |
| 6 | Compliance & Tax | 40m | [ ] |
| 7 | Subscriptions | 35m | [ ] |
| 8 | Testing | 30m | [ ] |
| 9 | Deployment Prep | 25m | [ ] |
| 10 | Mobile & Polish | 20m | [ ] |
| 11 | Launch | 15m | [ ] |

**Total: ~7 hours**

## Key Principles

1. **Context First**: Always read PROJECT_CONTEXT.md before each vibe command
2. **Test Immediately**: Verify each phase works before moving on
3. **Commit Frequently**: Commit after each successful phase
4. **Fix Fast**: Don't move forward with errors
5. **Progress > Perfection**: Ship incrementally

## Emergency Commands

### Fix Missing Dependencies
```bash
vibe "Install missing dependencies for [feature]"
```

### Fix Type Errors
```bash
vibe "Fix TypeScript errors in [file]. Error: [paste error]"
```

### Make Responsive
```bash
vibe "Make [component] mobile responsive using Tailwind"
```

### Debug Feature
```bash
vibe "Debug why [feature] is not working. Check console and fix"
```

## Next Steps

See the full workflow guide: `YAICO_VIBE_WORKFLOW_GUIDE.md`
