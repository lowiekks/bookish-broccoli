# 🚀 YAICO - Financial SaaS for Dutch Freelancers

> **Y**our **AI**-powered **CO**mpliance companion for Dutch freelancers

A comprehensive financial management platform featuring AI-powered transaction categorization, DBA risk monitoring, and EU VAT calculations - built using the proven Vibe Code workflow.

## 📖 Documentation

This repository contains a complete, step-by-step guide to building YAICO from scratch:

### Core Guides
- **[🚀 YAICO Vibe Workflow Guide](./YAICO_VIBE_WORKFLOW_GUIDE.md)** - Complete 11-phase implementation roadmap (start here!)
- **[📋 Project Context](./PROJECT_CONTEXT.md)** - Project overview and tech stack
- **[🤖 CLAUDE.md](./CLAUDE.md)** - Auto-loaded context file for Claude Code sessions

### Advanced Workflows
- **[⚡ Claude Code Workflow](./docs/guides/CLAUDE_CODE_WORKFLOW.md)** - Advanced "Explore → Plan → Code → Commit" workflow
- **[⚡ Quick Start Guide](./docs/guides/QUICK_START.md)** - Condensed reference for experienced developers
- **[📝 Command Reference](./docs/guides/COMMAND_REFERENCE.md)** - Complete vibe command catalog
- **[🔧 Troubleshooting Guide](./docs/guides/TROUBLESHOOTING.md)** - Common issues and solutions

### Reference Documentation
- **[🏗️ Architecture Overview](./docs/ARCHITECTURE.md)** - System architecture and design decisions
- **[✅ Progress Checklist](./docs/PROGRESS_CHECKLIST.md)** - Detailed phase-by-phase tracker

### Automation
- **[⚙️ Custom Commands](./.claude/commands/)** - 7 slash commands for common tasks (test, review, optimize, etc.)

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

## 🎯 Development Workflow

This project uses two complementary workflows:

### 1. Vibe Code Workflow (Phase-Based Implementation)
A systematic, 11-phase approach for building from scratch - perfect for greenfield projects.

**Phases (~7 hours total):**
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

**Principles:** Context First → Test Immediately → Commit Frequently → Fix Fast → Progress > Perfection

📖 **[Full Vibe Code Guide](./YAICO_VIBE_WORKFLOW_GUIDE.md)**

### 2. Claude Code Workflow (Daily Development)
The advanced "Explore → Plan → Code → Commit" workflow for ongoing development work.

**Process:**
1. **Explore** - Prime context with CLAUDE.md, use @ syntax to reference files
2. **Plan** - Use Plan Mode for safe analysis and detailed planning
3. **Code** - TDD approach, extended thinking for complex problems
4. **Commit** - Automate with custom slash commands

**Features:**
- Auto-loaded context (CLAUDE.md)
- Plan Mode for safe exploration
- 7 custom slash commands (test, review, optimize, etc.)
- Session resumption
- Git worktrees for parallel work

📖 **[Advanced Workflow Guide](./docs/guides/CLAUDE_CODE_WORKFLOW.md)** | **[Custom Commands](./.claude/commands/)**

## 🚀 Quick Start

### Prerequisites
```bash
# Check your environment
node --version  # Should be 18+
npm --version   # Should be 9+
claude --version  # Claude Code CLI
```

### Two Ways to Start

#### Option A: Build from Scratch (Vibe Code Workflow)

Perfect for greenfield projects or learning the full stack.

1. **Read the Context**
   ```bash
   cat PROJECT_CONTEXT.md
   cat CLAUDE.md
   ```

2. **Follow the Workflow**
   Start with [Phase 1 of the Vibe Workflow Guide](./YAICO_VIBE_WORKFLOW_GUIDE.md#-phase-1-project-initialization-30-minutes)

3. **Execute Phase by Phase**
   Each phase includes exact commands to copy and paste

📖 **Guide:** [YAICO_VIBE_WORKFLOW_GUIDE.md](./YAICO_VIBE_WORKFLOW_GUIDE.md)

#### Option B: Develop with Claude Code (Advanced Workflow)

Perfect for ongoing development and feature additions.

1. **Start Claude Code**
   ```bash
   claude
   # CLAUDE.md is automatically loaded
   ```

2. **Use Plan Mode for New Features**
   ```bash
   claude --permission-mode plan
   ```

   Then prompt:
   ```
   "I need to add [feature]. Using @src/relevant/files.ts as
   reference, create a detailed implementation plan."
   ```

3. **Use Custom Commands**
   ```bash
   /test src/lib/tax/vat-calculator.ts
   /review src/server/actions/transaction.actions.ts
   /optimize src/components/dashboard/TransactionList.tsx
   ```

📖 **Guide:** [Claude Code Workflow](./docs/guides/CLAUDE_CODE_WORKFLOW.md) | [Commands](./.claude/commands/)

## 📊 Project Status

This repository contains:
- ✅ Complete implementation guide
- ✅ Project context and architecture documentation
- ✅ Phase-by-phase workflow with exact commands
- ✅ Testing strategies and deployment guides
- ⏳ Implementation (follow the workflow guide to build!)

## 🎓 Learning Resources

### For Developers
- **New to the project?** Read [CLAUDE.md](./CLAUDE.md) and [PROJECT_CONTEXT.md](./PROJECT_CONTEXT.md)
- **Building from scratch?** Follow the [Vibe Workflow Guide](./YAICO_VIBE_WORKFLOW_GUIDE.md)
- **Using Claude Code?** Master the [Advanced Workflow](./docs/guides/CLAUDE_CODE_WORKFLOW.md)
- **Want quick reference?** Check the [Quick Start Guide](./docs/guides/QUICK_START.md)
- **Understanding architecture?** Read the [Architecture Overview](./docs/ARCHITECTURE.md)
- **Stuck on an issue?** Consult the [Troubleshooting Guide](./docs/guides/TROUBLESHOOTING.md)

### Key Concepts
- **Vibe Code Workflow**: Phase-based implementation for building from scratch (~7 hours)
- **Claude Code Workflow**: "Explore → Plan → Code → Commit" for daily development
- **CLAUDE.md**: Auto-loaded context file that guides every Claude Code session
- **Custom Commands**: 7 slash commands for testing, review, optimization, and code generation
- **Human-in-the-Loop AI**: AI suggestions with user review for <90% certainty
- **Dutch DBA Rules**: Client concentration monitoring for freelancer compliance (>70% = risk)
- **EU VAT Complexity**: Reverse charge, distance selling thresholds, 21% standard rate

## 📁 Repository Structure

```
bookish-broccoli/
├── YAICO_VIBE_WORKFLOW_GUIDE.md  # 11-phase implementation guide (START HERE)
├── PROJECT_CONTEXT.md             # Project overview for humans
├── CLAUDE.md                      # Auto-loaded context for Claude Code
├── README.md                      # This file
├── .claude/
│   └── commands/                  # Custom slash commands (7 commands)
│       ├── test.md                # Run and fix tests
│       ├── review.md              # Code review
│       ├── optimize.md            # Performance optimization
│       ├── add-test.md            # Generate tests
│       ├── fix-types.md           # Fix TypeScript errors
│       ├── add-gateway.md         # Create API gateway
│       └── add-component.md       # Create UI component
└── docs/
    ├── ARCHITECTURE.md            # System architecture
    ├── PROGRESS_CHECKLIST.md      # Detailed phase tracker
    └── guides/
        ├── CLAUDE_CODE_WORKFLOW.md # Advanced workflow (Explore→Plan→Code→Commit)
        ├── QUICK_START.md          # Quick reference
        ├── COMMAND_REFERENCE.md    # Complete command catalog
        └── TROUBLESHOOTING.md      # Common issues & solutions
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
