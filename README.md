# 🚀 YAICO - Financial SaaS for Dutch Freelancers

> **Y**our **AI**-powered **CO**mpliance companion for Dutch freelancers

A comprehensive financial management platform featuring AI-powered transaction categorization, DBA risk monitoring, and EU VAT calculations - built using the proven Vibe Code workflow.

## 📖 Documentation

This repository contains a complete, step-by-step guide to building YAICO from scratch:

- **[🚀 YAICO Vibe Workflow Guide](./YAICO_VIBE_WORKFLOW_GUIDE.md)** - Complete implementation roadmap (start here!)
- **[📋 Project Context](./PROJECT_CONTEXT.md)** - Project overview and tech stack
- **[⚡ Quick Start Guide](./docs/guides/QUICK_START.md)** - Condensed reference for experienced developers
- **[🏗️ Architecture Overview](./docs/ARCHITECTURE.md)** - System architecture and design decisions

## ✨ Features

### Core Functionality
- **AI Transaction Categorization** - 90% accuracy using Google Gemini AI
- **DBA Risk Monitoring** - Real-time client concentration analysis
- **EU VAT Calculator** - Automated VAT calculations for EU/non-EU transactions
- **Real-time Dashboard** - Live financial overview and metrics
- **Multi-tier Subscriptions** - Starter, Professional, and Enterprise plans

### Dutch-Specific Compliance
- VAT: 21% standard rate
- DBA Risk: Alert when >70% income from one client
- Currency: EUR
- Tax deadlines and documentation checklists

## 🛠️ Tech Stack

**Frontend**
- Next.js 14 (App Router)
- TypeScript (strict mode)
- Tailwind CSS
- React Hook Form + Zod

**Backend**
- Firebase (Auth, Firestore, Functions)
- Next.js Server Actions
- Google Gemini AI
- Stripe Payments

**Infrastructure**
- Vercel (hosting)
- Firebase (database & auth)
- GitHub Actions (CI/CD)

## 🎯 The Vibe Code Workflow

This project is built using the **Vibe Code methodology** - a systematic, phase-based approach to building complete applications efficiently:

### 11 Phases in ~7 Hours
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

**Key Principles:**
- Context First (read PROJECT_CONTEXT.md before each phase)
- Test Immediately (verify before moving forward)
- Commit Frequently (after each successful phase)
- Fix Fast (don't proceed with errors)
- Progress > Perfection (ship incrementally)

## 🚀 Quick Start

### Prerequisites
```bash
# Check your environment
node --version  # Should be 18+
npm --version   # Should be 9+
code --version  # VS Code with Claude extension
```

### Getting Started

1. **Read the Context**
   ```bash
   cat PROJECT_CONTEXT.md
   ```

2. **Follow the Workflow**
   Start with [Phase 1 of the Vibe Workflow Guide](./YAICO_VIBE_WORKFLOW_GUIDE.md#-phase-1-project-initialization-30-minutes)

3. **Execute Phase by Phase**
   Each phase includes exact commands to copy and paste

## 📊 Project Status

This repository contains:
- ✅ Complete implementation guide
- ✅ Project context and architecture documentation
- ✅ Phase-by-phase workflow with exact commands
- ✅ Testing strategies and deployment guides
- ⏳ Implementation (follow the workflow guide to build!)

## 🎓 Learning Resources

### For Developers
- **New to Next.js?** Start with the [Quick Start Guide](./docs/guides/QUICK_START.md)
- **Want to understand the system?** Read the [Architecture Overview](./docs/ARCHITECTURE.md)
- **Ready to build?** Follow the [Vibe Workflow Guide](./YAICO_VIBE_WORKFLOW_GUIDE.md)

### Key Concepts
- **Vibe Code**: Efficient, context-driven development workflow
- **Human-in-the-Loop AI**: AI suggestions with user review for <90% certainty
- **Dutch DBA Rules**: Client concentration monitoring for freelancer compliance
- **EU VAT Complexity**: Reverse charge, distance selling thresholds

## 📁 Repository Structure

```
bookish-broccoli/
├── YAICO_VIBE_WORKFLOW_GUIDE.md  # Complete workflow guide (START HERE)
├── PROJECT_CONTEXT.md             # Project overview
├── README.md                      # This file
└── docs/
    ├── ARCHITECTURE.md            # System architecture
    ├── guides/
    │   └── QUICK_START.md         # Quick reference
    ├── testing/                   # Testing documentation
    └── deployment/                # Deployment guides
```

## 🤝 Contributing

This is a guide repository. To build your own YAICO:

1. Fork this repository
2. Follow the [Vibe Workflow Guide](./YAICO_VIBE_WORKFLOW_GUIDE.md)
3. Customize for your needs
4. Share your experience!

## 📝 License

MIT License - see LICENSE file for details

## 🎯 Next Steps

Ready to build YAICO? Here's what to do:

1. ⭐ Star this repository
2. 📖 Read [PROJECT_CONTEXT.md](./PROJECT_CONTEXT.md)
3. 🚀 Start [Phase 1](./YAICO_VIBE_WORKFLOW_GUIDE.md#-phase-1-project-initialization-30-minutes)
4. 💪 Build your financial SaaS in ~7 hours!

---

**Remember: Progress > Perfection**

Questions? Open an issue or check the workflow guide!
