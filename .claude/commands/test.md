---
description: "Find and run tests for a file, fix any failures"
argument-hint: [filepath]
---

Here is the file I'm working on: @$1

1. Find the corresponding test file for `$1` (usually in `src/__tests__/` with `.test.ts` or `.test.tsx` extension).
2. Run the tests in that test file using `npm run test -- [test-file-path]`.
3. If any tests fail, analyze the failure and fix the code in `$1` to make tests pass.
4. Re-run tests to verify the fix.

Provide a summary of what was tested and any fixes applied.
