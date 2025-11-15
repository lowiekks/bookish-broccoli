---
description: "Generate comprehensive unit tests for a file"
argument-hint: [filepath]
---

Create comprehensive unit tests for @$1 following YAICO testing patterns.

Reference existing test patterns in @src/__tests__/ and @CLAUDE.md.

Include:
1. **Happy path cases** - Normal, expected behavior
2. **Error cases** - Invalid inputs, API failures
3. **Edge cases** - Boundary conditions, empty states
4. **Dutch-specific cases** - If relevant (VAT format, DBA thresholds, EUR formatting)

Use:
- Vitest as the test framework
- React Testing Library for component tests
- MSW for mocking API calls
- Proper TypeScript types

Save the test file to the appropriate location in `src/__tests__/` following the project structure.
