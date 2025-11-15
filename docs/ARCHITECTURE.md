# YAICO Architecture Overview

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend (Next.js)                    │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │  Dashboard   │  │ Transactions │  │  Compliance  │      │
│  │    Pages     │  │    Pages     │  │    Pages     │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
│                           │                                  │
│  ┌────────────────────────────────────────────────┐         │
│  │         React Components & Hooks                │         │
│  │  - useAuth  - useTransactions  - useDBA        │         │
│  └────────────────────────────────────────────────┘         │
└─────────────────────────────────────────────────────────────┘
                             │
                             ├─── API Routes (Next.js)
                             │
┌─────────────────────────────────────────────────────────────┐
│                      Backend Services                        │
│                                                               │
│  ┌─────────────┐  ┌──────────────┐  ┌─────────────┐        │
│  │   Server    │  │   Gateways   │  │   Services  │        │
│  │   Actions   │  │              │  │             │        │
│  │             │  │  - Gemini AI │  │  - AI Cat.  │        │
│  │             │  │  - Stripe    │  │  - DBA Mon. │        │
│  │             │  │  - Firebase  │  │  - VAT Calc │        │
│  └─────────────┘  └──────────────┘  └─────────────┘        │
└─────────────────────────────────────────────────────────────┘
                             │
                             ├─── Firestore (Database)
                             ├─── Firebase Auth
                             ├─── Google Gemini API
                             └─── Stripe API
```

## Directory Structure

```
yaico-app/
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   │   ├── sign-in/          # Authentication pages
│   │   │   └── sign-up/
│   │   ├── (app)/
│   │   │   ├── dashboard/        # Protected app pages
│   │   │   ├── transactions/
│   │   │   ├── compliance/
│   │   │   └── settings/
│   │   └── api/
│   │       ├── webhooks/         # API routes
│   │       ├── stripe/
│   │       └── cron/
│   ├── components/
│   │   ├── ui/                   # Reusable UI components
│   │   ├── auth/                 # Auth-specific components
│   │   ├── dashboard/            # Dashboard components
│   │   ├── transactions/         # Transaction components
│   │   ├── compliance/           # Compliance components
│   │   └── billing/              # Billing components
│   ├── lib/
│   │   ├── firebase/             # Firebase config & utilities
│   │   ├── types/                # TypeScript types & Zod schemas
│   │   ├── utils/                # Utility functions
│   │   ├── compliance/           # DBA monitoring logic
│   │   ├── tax/                  # VAT calculation logic
│   │   ├── subscriptions/        # Subscription limits
│   │   └── queues/               # Background job queues
│   ├── server/
│   │   ├── actions/              # Next.js server actions
│   │   └── gateways/             # External API integrations
│   ├── services/                 # Business logic services
│   ├── hooks/                    # Custom React hooks
│   └── contexts/                 # React contexts
├── docs/                         # Documentation
├── PROJECT_CONTEXT.md            # Project overview
└── YAICO_VIBE_WORKFLOW_GUIDE.md  # Implementation guide
```

## Data Flow

### Transaction Processing Flow

```
1. User creates transaction
   ↓
2. Form validation (Zod)
   ↓
3. Server action: createTransaction
   ↓
4. Save to Firestore (status: processing)
   ↓
5. Queue AI analysis job
   ↓
6. Gemini AI categorizes
   ↓
7. Update transaction with AI suggestion
   ↓
8. If certainty < 0.9 → mark for review
   ↓
9. User approves/modifies
   ↓
10. Update aggregated data
    ↓
11. Trigger DBA monitoring check
```

### Authentication Flow

```
1. User submits credentials
   ↓
2. Firebase Authentication
   ↓
3. onAuthStateChanged listener
   ↓
4. Update AuthContext
   ↓
5. Redirect to dashboard
   ↓
6. Middleware checks auth on protected routes
```

## Core Technologies

### Frontend
- **Next.js 14**: App Router, Server Components, Server Actions
- **React 18**: UI rendering, hooks, context
- **TypeScript**: Type safety
- **Tailwind CSS**: Styling
- **React Hook Form**: Form management
- **Zod**: Schema validation
- **TanStack Query**: Data fetching & caching

### Backend
- **Firebase Auth**: User authentication
- **Firestore**: NoSQL database
- **Firebase Functions**: Serverless functions (optional)
- **Next.js API Routes**: Backend endpoints

### External Services
- **Google Gemini AI**: Transaction categorization
- **Stripe**: Payment processing
- **Vercel**: Hosting & deployment

## Security Considerations

1. **Authentication**: Firebase Auth with secure token management
2. **Authorization**: Firestore security rules + server-side checks
3. **Input Validation**: Zod schemas on both client and server
4. **Rate Limiting**: API route protection
5. **Environment Variables**: Secure credential management
6. **CORS**: Proper configuration for API routes
7. **XSS Prevention**: Input sanitization
8. **CSRF Protection**: Next.js built-in protection

## Performance Optimizations

1. **Server Components**: Reduce client-side JavaScript
2. **Code Splitting**: Dynamic imports for routes
3. **Caching**: TanStack Query for data caching
4. **Firestore Indexes**: Optimized queries
5. **Image Optimization**: next/image
6. **Bundle Analysis**: Regular bundle size checks
7. **Lazy Loading**: Components and routes

## Scalability Considerations

### Current (MVP)
- In-memory queue for background jobs
- Direct Firestore queries
- Single-region deployment

### Future (Production Scale)
- Redis/BullMQ for job queues
- Firestore aggregation pipelines
- CDN for static assets
- Multi-region deployment
- Caching layer (Redis)
- Background workers (Cloud Functions/Cloud Run)
- Database sharding if needed

## Monitoring & Observability

1. **Error Tracking**: Error boundaries + logging
2. **Performance Monitoring**: Web Vitals
3. **User Analytics**: Usage metrics
4. **API Monitoring**: Request/response logging
5. **Cost Monitoring**: Firebase/Stripe usage tracking

## Compliance & Data Privacy

1. **GDPR**: Data export, deletion capabilities
2. **Data Retention**: Configurable retention policies
3. **Audit Logging**: Transaction history
4. **Encryption**: At rest (Firestore) and in transit (HTTPS)
