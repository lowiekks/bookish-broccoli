---
description: "Fix all TypeScript errors in the project"
---

1. Run `npm run type-check` to find all TypeScript errors
2. Analyze each error carefully
3. Fix errors following these priorities:
   - Fix actual bugs first
   - Add proper types (never use `any` unless absolutely necessary)
   - Follow patterns from @CLAUDE.md
   - Maintain type safety with Zod schemas from @src/lib/types/schemas.ts
4. Re-run `npm run type-check` to verify all fixes
5. Provide a summary of what was fixed

**Note**: Ensure all fixes follow the strict TypeScript configuration in tsconfig.json.
