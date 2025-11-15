---
description: "Comprehensive code review with security, performance, and quality focus"
argument-hint: [filepath]
---

Perform a thorough code review of @$1 following YAICO standards in @CLAUDE.md.

**Security Audit:**
- [ ] XSS vulnerabilities (input sanitization)
- [ ] SQL injection (though we use Firestore, check raw queries)
- [ ] Authentication checks (server actions use createServerAction wrapper?)
- [ ] Authorization (users can only access their own data?)
- [ ] Sensitive data exposure (no API keys in client code?)
- [ ] CSRF protection (Next.js handles this, but verify)

**Performance Review:**
- [ ] Unnecessary re-renders (React.memo needed?)
- [ ] Heavy computations (should be memoized with useMemo/useCallback?)
- [ ] Large bundle impact (dynamic imports needed?)
- [ ] Database queries (properly indexed? Using converters?)
- [ ] API calls (cached with React Query?)

**Code Quality:**
- [ ] Follows TypeScript strict mode
- [ ] Uses Zod schemas for validation
- [ ] Adheres to project patterns (@CLAUDE.md)
- [ ] Proper error handling
- [ ] Accessibility (ARIA labels, keyboard navigation)
- [ ] Mobile responsive (if UI component)

**Dutch Business Logic:**
- [ ] VAT calculations correct (21% standard, reverse charge, etc.)
- [ ] DBA risk thresholds accurate (>70% = risk)
- [ ] EUR currency formatting (€1.234,56)
- [ ] Dutch VAT number validation (NL + 9 digits + B + 2 digits)

Provide:
1. Critical issues (must fix immediately)
2. Important improvements (should fix soon)
3. Nice-to-haves (consider for future)
4. What's done well (positive feedback)
