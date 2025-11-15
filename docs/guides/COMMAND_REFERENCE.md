# Vibe Code Command Reference

Quick reference for all Vibe commands used in the YAICO workflow.

## 📋 Command Structure

All Vibe commands follow this pattern:
```bash
vibe "[Context instruction]. [Main task]. [Requirements]. [Output expectations]."
```

**Best Practice**: Always start with context reading:
```bash
vibe "Read PROJECT_CONTEXT.md first, then [your command]"
```

---

## 🎯 Phase-by-Phase Commands

### Phase 1: Project Initialization

**Initialize Next.js Project**
```bash
vibe "Read the PROJECT_CONTEXT.md file first. Then create a Next.js 14 TypeScript project with app router. Include package.json with these exact dependencies: next@14.2.13, react@18.3.1, react-dom@18.3.1, typescript@5.6.2, @types/react, @types/react-dom, @types/node, tailwindcss, autoprefixer, postcss. Add firebase@10.14.0, firebase-admin@12.6.0, zod@3.23.8, @tanstack/react-query@5.56.2, @hookform/resolvers@3.9.0, react-hook-form@7.53.0, lucide-react@0.446.0, clsx@2.1.1, tailwind-merge@2.5.2, date-fns@3.6.0. Create src directory structure: app/(auth)/sign-in, app/(auth)/sign-up, app/(app)/dashboard, app/(app)/transactions, app/api/webhooks, components/ui, components/auth, components/dashboard, lib/firebase, lib/types, lib/utils, server/actions, server/gateways. Add TypeScript config with strict mode and path aliases (@/*). Create tailwind.config.ts with custom yaico-mint color (#00CC99). Add .env.example with placeholders for Firebase, Stripe, and Gemini keys. Create a basic README.md. Make the project immediately runnable with npm run dev."
```

### Phase 2: Authentication

**Firebase Configuration**
```bash
vibe "Create Firebase configuration files. First, create src/lib/firebase/config.ts that reads environment variables for Firebase configuration (apiKey, authDomain, projectId from process.env). Export initialized Firebase app and auth instances. Create src/lib/firebase/admin.ts for server-side Firebase Admin SDK initialization. Add proper error handling for missing environment variables. Create a type-safe wrapper for Firestore with converter functions. Include connection state management."
```

**Auth Context**
```bash
vibe "Create a complete authentication system. Build src/contexts/AuthContext.tsx with React Context that provides useAuth hook. Include: current user state, loading state, sign in with email/password, sign up with email/password, sign out, Google OAuth sign in, password reset functionality. Add proper TypeScript types for User. Create src/hooks/useAuth.ts that exports the useAuth hook. Handle authentication persistence with Firebase onAuthStateChanged. Show loading state while checking auth."
```

**Auth UI**
```bash
vibe "Create authentication UI components. Build src/app/(auth)/sign-in/page.tsx with email/password form and Google sign-in button. Use react-hook-form with zod validation. Create matching sign-up page with name, email, password, confirm password fields. Add password strength indicator. Include 'Remember me' checkbox. Style with Tailwind CSS using card layout, proper spacing, and hover states. Add loading states for form submission. Include error message display. Link between sign-in and sign-up pages."
```

**Protected Routes**
```bash
vibe "Implement route protection. Create src/middleware.ts that checks authentication status and redirects unauthenticated users to /sign-in for protected routes (dashboard, transactions, etc). Create src/components/auth/ProtectedRoute.tsx wrapper component. Add src/app/(app)/layout.tsx that wraps protected app routes. Include automatic redirect after sign in. Handle loading states properly."
```

### Phase 3: Data Layer

**Zod Schemas**
```bash
vibe "Create comprehensive Zod schemas in src/lib/types/schemas.ts. Include: 1) TransactionSchema with id (uuid), userId (uuid), amount (number, max 2 decimals), currency (EUR/USD/GBP), description (3-500 chars), status (draft/processing/pending_review/approved/error), clientId (optional), aiAnalysis object with suggestedCategory and certaintyScore (0-1), taxDetails with vatRate and vatAmount. 2) ClientSchema with clientName, classification (b2b/b2c/unknown), countryCode, vatNumber with Dutch format validation (NL + 9 digits + B + 2 digits). 3) SubscriptionSchema with tier (starter/professional/enterprise), status, limits object. 4) UserSchema with profile fields. Export all types using z.infer. Add helper functions for Dutch IBAN and VAT validation."
```

**Firebase Converters**
```bash
vibe "Create Firestore converters in src/lib/firebase/converters.ts. For each Zod schema (Transaction, Client, User, Subscription), create a converter that: validates data with Zod on read, transforms dates properly, handles undefined fields, provides type safety. Export typed collection references using these converters. Create helper functions for common queries like getTransactionsByUser, getClientsByUser. Include error handling that logs validation errors but doesn't crash."
```

**Server Actions**
```bash
vibe "Set up Next.js server actions foundation. Create src/server/actions/base.action.ts with createServerAction wrapper that: checks authentication using cookies, validates input with Zod schemas, handles errors consistently, returns typed responses. Create standard response types: ActionSuccess<T> and ActionError. Add rate limiting logic. Create src/server/actions/transaction.actions.ts with createTransaction action that validates input, checks user subscription limits, creates Firestore document with status 'processing' for AI analysis. Include proper error messages."
```

### Phase 4: AI Integration

**AI Gateway Base**
```bash
vibe "Create AI gateway system. Build src/server/gateways/base.gateway.ts with abstract BaseGateway class that includes: retry logic with exponential backoff, circuit breaker pattern, response caching, error handling, TypeScript generics for type safety. Add GatewayResponse<T> type with success, data, error fields. Implement withRetry wrapper. Add request/response logging for debugging."
```

**Gemini Integration**
```bash
vibe "Implement Google Gemini AI integration. Create src/server/gateways/gemini.gateway.ts extending BaseGateway. Add suggestCategory method that: takes transaction description and historical transactions, creates optimized prompt for Dutch financial categorization, calls Gemini API, returns category suggestion with certainty score (0-1) and reasoning. Include Dutch context: KPN/Ziggo = utilities, Albert Heijn/Jumbo = groceries, NS = transport. Add mock mode for testing without API. Cache responses by description hash."
```

**AI Workflow**
```bash
vibe "Build the AI categorization workflow. Create src/services/ai-categorization.service.ts that: processes new transactions, calls Gemini gateway, updates transaction with AI suggestions, implements Human-in-the-Loop (if certainty < 0.9, mark for review). Create src/app/api/webhooks/process-transaction/route.ts webhook that triggers on new transactions. Add src/components/transactions/ReviewQueue.tsx showing transactions pending review with approve/reject/modify buttons. Include optimistic UI updates."
```

---

## 🔧 Utility Commands

### Fix Errors
```bash
vibe "Fix the TypeScript error in [file]. The error is: [error message]"
```

### Install Dependencies
```bash
vibe "Install missing dependencies for [feature]"
```

### Make Responsive
```bash
vibe "Make [component] mobile responsive using Tailwind"
```

### Debug Feature
```bash
vibe "Debug why [feature] is not working. Check console errors and fix"
```

### Add Tests
```bash
vibe "Add unit tests for [component/function] using Vitest"
```

### Optimize Performance
```bash
vibe "Optimize [component] for better performance. Add lazy loading and memoization where appropriate"
```

---

## 💡 Pro Tips

### 1. Always Include Context
```bash
# Good ✅
vibe "Read PROJECT_CONTEXT.md. Create a button component"

# Bad ❌
vibe "Create a button component"
```

### 2. Be Specific with Requirements
```bash
# Good ✅
vibe "Create Button.tsx with primary/secondary variants, small/medium/large sizes, loading state, and TypeScript types"

# Bad ❌
vibe "Create a button"
```

### 3. Specify Output Format
```bash
# Good ✅
vibe "Create VAT calculator that returns { gross, net, vat, rate } object"

# Bad ❌
vibe "Create VAT calculator"
```

### 4. Include Error Handling
```bash
# Good ✅
vibe "Create API route with try-catch, Zod validation, and error responses"

# Bad ❌
vibe "Create API route"
```

### 5. Request Immediate Runnability
```bash
# Good ✅
vibe "Create auth system with mock data so I can test immediately"

# Bad ❌
vibe "Create auth system"
```

---

## 📝 Command Templates

### Create Component
```bash
vibe "Create [component-name] component in [path]. Include [props], [state], [styling with Tailwind]. Add TypeScript types. Make it responsive and accessible."
```

### Create API Route
```bash
vibe "Create API route at [path] that [functionality]. Use Zod for validation, check authentication, handle errors, return typed responses."
```

### Create Hook
```bash
vibe "Create useXXX hook that [functionality]. Include [state], [effects], proper cleanup, TypeScript types, and error handling."
```

### Create Service
```bash
vibe "Create [service-name] service with [methods]. Include error handling, TypeScript types, and unit tests."
```

### Fix Issue
```bash
vibe "Fix the issue in [file] where [description of problem]. The error message is: [error]. Expected behavior: [what should happen]."
```

---

## 🚨 When Commands Fail

### 1. Error: Missing Dependencies
```bash
npm install [package-name]
# Or use vibe:
vibe "Install [package-name] and configure it for [use case]"
```

### 2. Error: Type Errors
```bash
vibe "Fix all TypeScript errors in the project. Show me what was wrong and what you fixed."
```

### 3. Error: Build Fails
```bash
vibe "Debug the build error. The error is: [error message]. Fix all issues."
```

### 4. Error: Runtime Error
```bash
vibe "Debug the runtime error in [component]. Error: [error message]. Add console logs and fix the issue."
```

---

## ✅ Verification Commands

After each phase, verify with these commands:

```bash
# Type check
npm run type-check

# Run dev server
npm run dev

# Run tests
npm test

# Build for production
npm run build

# Check for errors
npm run lint
```

---

## 📚 Related Resources

- [Full Workflow Guide](../../YAICO_VIBE_WORKFLOW_GUIDE.md)
- [Quick Start](./QUICK_START.md)
- [Architecture](../ARCHITECTURE.md)
- [Project Context](../../PROJECT_CONTEXT.md)
