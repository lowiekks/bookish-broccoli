# YAICO Project Context for Claude Code

## Project Overview
YAICO (Your AI-powered COmpliance companion) is a financial management SaaS for Dutch freelancers. It provides AI-powered transaction categorization, DBA compliance monitoring, and EU VAT calculations.

---

## 📁 Project Structure

### Application Code
- **Main app routes**: `src/app/`
  - Auth pages: `src/app/(auth)/sign-in/`, `src/app/(auth)/sign-up/`
  - Protected pages: `src/app/(app)/dashboard/`, `src/app/(app)/transactions/`, `src/app/(app)/compliance/`
  - API routes: `src/app/api/webhooks/`, `src/app/api/stripe/`, `src/app/api/cron/`

### Components
- **UI components**: `src/components/ui/` - Reusable design system components (Button, Input, Card, etc.)
- **Feature components**:
  - `src/components/auth/` - Authentication-related
  - `src/components/dashboard/` - Dashboard widgets
  - `src/components/transactions/` - Transaction management
  - `src/components/compliance/` - DBA monitoring, VAT calculator
  - `src/components/billing/` - Subscription and payment

### Business Logic
- **Libraries**: `src/lib/`
  - `firebase/` - Firebase config, converters, utilities
  - `types/` - TypeScript types and Zod schemas
  - `utils/` - Shared helper functions
  - `compliance/` - DBA monitoring logic
  - `tax/` - VAT calculation logic
  - `subscriptions/` - Subscription limits and gating
  - `queues/` - Background job processing

### Server-Side Code
- **Server actions**: `src/server/actions/`
  - `base.action.ts` - Server action wrapper with auth and validation
  - `transaction.actions.ts` - Transaction CRUD operations

- **External integrations**: `src/server/gateways/`
  - `base.gateway.ts` - Base gateway with retry, caching, circuit breaker
  - `gemini.gateway.ts` - Google Gemini AI integration
  - `stripe.gateway.ts` - Stripe payment integration

- **Services**: `src/services/`
  - `ai-categorization.service.ts` - AI transaction categorization workflow

### Data Layer
- **Schemas**: `src/lib/types/schemas.ts` - All Zod schemas (Transaction, Client, User, Subscription)
- **Converters**: `src/lib/firebase/converters.ts` - Type-safe Firestore converters

### Testing
- **Unit tests**: `src/__tests__/` - Vitest unit tests
- **Integration tests**: `src/__tests__/integration/` - Integration test suites

### Configuration
- **Context files**: `PROJECT_CONTEXT.md`, `CLAUDE.md`
- **Guides**: `docs/`, `YAICO_VIBE_WORKFLOW_GUIDE.md`

---

## 🛠️ Tech Stack

### Frontend
- **Framework**: Next.js 14 (App Router)
- **Language**: TypeScript 5.6 (strict mode)
- **Styling**: Tailwind CSS with custom `yaico-mint` color (#00CC99)
- **Forms**: React Hook Form + Zod validation
- **Data Fetching**: TanStack React Query
- **Icons**: Lucide React
- **Date Handling**: date-fns

### Backend
- **Authentication**: Firebase Auth (email/password + Google OAuth)
- **Database**: Firestore (NoSQL)
- **Server Logic**: Next.js Server Actions
- **AI**: Google Gemini API (transaction categorization)
- **Payments**: Stripe (subscriptions)

### Infrastructure
- **Hosting**: Vercel (recommended)
- **Functions**: Next.js API Routes
- **CI/CD**: GitHub Actions

---

## 🎨 Design System

### Colors
- **Primary**: Blue (`#3B82F6`)
- **Success**: Mint Green (`#00CC99` - yaico-mint)
- **Error**: Red (`#EF4444`)
- **Warning**: Orange
- **Font**: Inter

### Component Patterns
All UI components in `src/components/ui/` follow these patterns:
- Forward refs for composition
- Variants using class variance authority (or clsx)
- TypeScript interfaces for props
- Tailwind CSS for styling
- Accessibility attributes (ARIA)

---

## 📋 Key Development Patterns

### 1. Server Actions
**Always** use the `createServerAction` wrapper from `src/server/actions/base.action.ts`:

```typescript
export const createTransaction = createServerAction(
  TransactionInputSchema,
  async (input, userId) => {
    // userId is automatically provided by wrapper (authenticated)
    // input is validated with Zod schema
    // Return ActionSuccess or ActionError
  }
);
```

### 2. Form Handling
**Always** use React Hook Form with zodResolver:

```typescript
const form = useForm({
  resolver: zodResolver(TransactionSchema),
  defaultValues: { ... }
});
```

### 3. Firestore Operations
**Always** use typed converters from `src/lib/firebase/converters.ts`:

```typescript
import { transactionsRef } from '@/lib/firebase/converters';

// Type-safe, validated read/write
const doc = await getDoc(transactionsRef(userId).doc(id));
```

### 4. External API Calls
**Always** use the gateway pattern in `src/server/gateways/`:
- Extend `BaseGateway` for retry logic and caching
- Include mock mode for testing
- Add proper error handling
- Use TypeScript generics for type safety

### 5. Type Safety
**Always** define types in `src/lib/types/schemas.ts` using Zod:

```typescript
export const TransactionSchema = z.object({ ... });
export type Transaction = z.infer<typeof TransactionSchema>;
```

### 6. Component Organization
- **UI components**: Stateless, reusable, in `src/components/ui/`
- **Feature components**: Stateful, feature-specific, in feature folders
- **Export pattern**: Export from `ui/index.ts` for clean imports

---

## 🇳🇱 Dutch Business Rules

### VAT (BTW)
- **Standard rate**: 21%
- **EU B2B with valid VAT**: 0% (reverse charge)
- **EU B2C**: Destination country rate (if >€10,000/year, else Dutch rate)
- **Non-EU**: 0%
- **VAT number format**: `NL` + 9 digits + `B` + 2 digits (e.g., `NL123456789B01`)

### DBA (Dependent Contractor) Rules
- **Risk threshold**: >70% income from single client
- **Risk levels**:
  - Safe: <50%
  - Warning: 50-70%
  - Danger: 70-85%
  - Critical: >85%

### Currency
- **Always EUR** for calculations
- Format: `€1.234,56` (European notation)

---

## 🧪 Testing

### Running Tests
```bash
npm run test              # All tests
npm run test -- [file]    # Specific test file
npm run test:coverage     # With coverage report
```

### Test Locations
- Unit tests: `src/__tests__/*.test.ts(x)`
- Integration tests: `src/__tests__/integration/*.test.tsx`
- Test utilities: `src/test/setup.ts`

### Test Patterns
- Use Vitest + React Testing Library
- Mock Firebase with MSW (Mock Service Worker)
- Test user flows, not implementation details
- Minimum 70% coverage for core features

---

## 📦 Development Commands

### Essential Commands
```bash
npm run dev              # Start dev server (localhost:3000)
npm run build            # Production build
npm run start            # Start production server
npm run type-check       # TypeScript validation
npm run lint             # ESLint check
npm run test             # Run tests
```

### Deployment
```bash
vercel --prod            # Deploy to Vercel
npm run pre-deploy       # Pre-deployment checks (if script exists)
```

---

## 🔐 Environment Variables

### Required Variables
See `.env.example` for all required environment variables.

**Client-side** (NEXT_PUBLIC_ prefix):
- `NEXT_PUBLIC_FIREBASE_API_KEY`
- `NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN`
- `NEXT_PUBLIC_FIREBASE_PROJECT_ID`

**Server-side** (no prefix):
- `FIREBASE_ADMIN_PROJECT_ID`
- `FIREBASE_ADMIN_PRIVATE_KEY`
- `FIREBASE_ADMIN_CLIENT_EMAIL`
- `GEMINI_API_KEY`
- `STRIPE_SECRET_KEY`
- `STRIPE_WEBHOOK_SECRET`

---

## 🚀 AI Integration Specifics

### Gemini AI Categorization
- **Model**: Gemini Pro
- **Certainty threshold**: 0.9 (90%)
- **Human-in-the-Loop**: If certainty < 0.9, mark for review
- **Dutch context**: Knows common Dutch vendors (KPN, NS, Albert Heijn, etc.)
- **Mock mode**: Use when `GEMINI_API_KEY` is not set

### AI Workflow
1. User creates transaction → status: `processing`
2. Background job calls Gemini gateway
3. AI suggests category with certainty score
4. If certainty ≥ 0.9 → auto-approve
5. If certainty < 0.9 → status: `pending_review`
6. User reviews and approves/modifies

---

## 💳 Subscription Tiers

### Tier Limits
Defined in `src/lib/subscriptions/limits.ts`:

- **Starter**: 100 transactions/month, 3 clients, no AI
- **Professional**: 1,000 transactions/month, unlimited clients, AI included
- **Enterprise**: Unlimited everything, priority support

### Feature Gating
- Check limits in `useSubscription` hook
- Show `UpgradePrompt` when limit reached
- Soft limit warning at 80%
- Hard limit block at 100%

---

## 🎯 Development Workflow

### Phase-Based Development
Follow the `YAICO_VIBE_WORKFLOW_GUIDE.md` for systematic implementation:
1. Project Initialization (30 min)
2. Authentication System (45 min)
3. Data Layer & Types (40 min)
4. AI Integration (50 min)
5. Dashboard & UI (45 min)
6. Compliance & Tax (40 min)
7. Subscriptions & Payments (35 min)
8. Testing & Validation (30 min)
9. Deployment Preparation (25 min)
10. Mobile & Polish (20 min)
11. Launch Checklist (15 min)

### Best Practices
1. **Read context first**: Always reference `PROJECT_CONTEXT.md` and this file
2. **Use Plan Mode**: For complex features, start with planning
3. **Test immediately**: Verify each phase before proceeding
4. **Commit frequently**: After each successful phase
5. **Fix fast**: Don't proceed with errors

### Common Tasks

**Add new transaction category:**
1. Update `TransactionSchema` in `src/lib/types/schemas.ts`
2. Update AI prompt in `src/server/gateways/gemini.gateway.ts`
3. Update UI category selector
4. Add tests

**Add new subscription tier:**
1. Update `TIER_LIMITS` in `src/lib/subscriptions/limits.ts`
2. Create Stripe price ID
3. Update billing page comparison table
4. Update feature gating logic

**Add new compliance rule:**
1. Create logic in `src/lib/compliance/`
2. Create widget in `src/components/compliance/`
3. Add to compliance dashboard
4. Add tests

---

## 🐛 Common Issues

### TypeScript Errors
- Check path aliases in `tsconfig.json` match `@/*` → `./src/*`
- Restart TS server in VS Code: Cmd/Ctrl + Shift + P → "TypeScript: Restart TS Server"

### Firebase Errors
- Ensure environment variables are set
- Check Firebase console for correct config
- Verify Firestore rules allow read/write for authenticated users

### Build Errors
- Run `npm run type-check` to see all TypeScript errors
- Run `npm run lint` to see linting issues
- Clear `.next` folder and rebuild: `rm -rf .next && npm run build`

### Test Failures
- Check mock setup in `src/test/setup.ts`
- Ensure MSW handlers are configured for API routes
- Run tests in watch mode: `npm run test -- --watch`

---

## 📚 Additional Resources

- **Full workflow guide**: `YAICO_VIBE_WORKFLOW_GUIDE.md`
- **Claude Code workflow**: `docs/guides/CLAUDE_CODE_WORKFLOW.md`
- **Command reference**: `docs/guides/COMMAND_REFERENCE.md`
- **Troubleshooting**: `docs/guides/TROUBLESHOOTING.md`
- **Architecture**: `docs/ARCHITECTURE.md`
- **Progress tracking**: `docs/PROGRESS_CHECKLIST.md`

---

## 💡 Tips for Working with Claude Code

1. **Reference files with @**: `@src/components/ui/Button.tsx`
2. **Use Plan Mode** for complex features: `claude --permission-mode plan`
3. **Create custom commands** in `.claude/commands/` for repetitive tasks
4. **Show screenshots** for UI issues (drag & drop into terminal)
5. **Resume sessions** with `claude -c` to continue previous work

---

**Last Updated**: 2025-11-15
**Version**: 1.0.0
**Project Status**: Documentation complete, ready for implementation
