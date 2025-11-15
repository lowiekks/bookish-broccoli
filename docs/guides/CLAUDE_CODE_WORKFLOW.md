# Advanced Claude Code Workflow
## Explore → Plan → Code → Commit

This document describes the optimal development workflow when using Claude Code, synthesizing proven tactics into a powerful, end-to-end development cycle.

---

## 🎯 The Core Philosophy

**"Explore, Plan, Code, Commit"** - Never jump straight into coding. Always understand first, plan second, execute third, and ship fourth.

---

## 📍 Phase 1: Explore & Plan (The "Look")

Before writing any code, give Claude context and agree on a plan. This prevents wasted effort.

### 1.1 Prime the Context with CLAUDE.md

Create a `CLAUDE.md` file at your project's root. Claude automatically loads this file, giving it essential context for every session.

**Example CLAUDE.md for YAICO:**

```markdown
# YAICO Project Context for Claude

## Project Structure
- Main application code: `src/app/`
- React components: `src/components/`
- Business logic: `src/lib/` and `src/services/`
- Server-side code: `src/server/actions/` and `src/server/gateways/`
- Type definitions: `src/lib/types/schemas.ts`
- Firebase configuration: `src/lib/firebase/`

## Tech Stack
- Next.js 14 (App Router) with TypeScript (strict mode)
- Firebase (Auth + Firestore)
- Google Gemini AI for transaction categorization
- Stripe for payments
- Tailwind CSS for styling
- React Hook Form + Zod for forms and validation

## Key Libraries
- `@/lib/utils.ts` - Shared helper functions
- `@/lib/types/schemas.ts` - All Zod schemas and TypeScript types
- `@/server/actions/base.action.ts` - Server action wrapper with auth

## Testing
- Unit tests: Vitest (`npm run test`)
- Tests location: `src/__tests__/`
- Run specific test: `npm run test -- [filename]`

## Development Commands
- `npm run dev` - Start dev server (port 3000)
- `npm run build` - Production build
- `npm run type-check` - TypeScript validation
- `npm run lint` - ESLint check

## Important Patterns
- All server actions must use `createServerAction` wrapper
- All forms use react-hook-form with zodResolver
- All database operations use Firestore converters for type safety
- All AI operations go through gateway pattern in `src/server/gateways/`

## Dutch Business Context
- VAT rate: 21% (standard)
- DBA risk threshold: >70% income from one client
- Currency: EUR
- VAT number format: NL + 9 digits + B + 2 digits
```

**Why CLAUDE.md matters:**
- Loaded automatically at session start
- Keeps context consistent across sessions
- Reduces repetitive explanations
- Guides Claude to your project's conventions

### 1.2 Use Plan Mode for Safe Analysis

For any complex task (refactor, new feature, architecture change), start in **Plan Mode**. This read-only mode lets Claude analyze your codebase and build a plan without making any changes.

```bash
# Start a new session in Plan Mode
claude --permission-mode plan
```

**Example prompts for Plan Mode:**

```
"I need to add a new /api/users endpoint. Using @src/api/ and
@src/lib/types.ts as references, create a detailed, step-by-step
implementation plan."
```

```
"Analyze the authentication flow in @src/contexts/AuthContext.tsx
and @src/middleware.ts. I want to add OAuth providers for GitHub
and LinkedIn. What's the best approach?"
```

```
"Review the transaction processing pipeline starting from
@src/server/actions/transaction.actions.ts through the AI gateway.
Plan how to add batch processing for multiple transactions."
```

**When to use Plan Mode:**
- ✅ New features that touch multiple files
- ✅ Refactoring existing code
- ✅ Architecture changes
- ✅ Performance optimization planning
- ✅ Security audits
- ❌ Simple bug fixes (just fix directly)
- ❌ Trivial changes (CSS tweaks, typos)

### 1.3 Reference Files with @ Syntax

**Never paste code.** The `@` syntax is your most powerful tool for adding context.

**Syntax:**
- `@src/main.ts` - Single file
- `@src/components/` - Entire directory
- `@src/components/*.tsx` - Glob pattern
- Type `@` and press `Tab` to autocomplete paths

**Examples:**

```
"Compare the authentication in @src/app/(auth)/sign-in/page.tsx
with @src/app/(auth)/sign-up/page.tsx and standardize the error
handling."
```

```
"Review all transaction-related components in @src/components/transactions/
and create a consistent loading state pattern."
```

```
"Using the schema in @src/lib/types/schemas.ts as reference, create
a new InvoiceSchema with similar validation patterns."
```

**Pro Tips:**
- Reference multiple files in one prompt
- Use directory references to analyze related code
- Combine with Plan Mode for thorough analysis

---

## 🚀 Phase 2: Code & Iterate (The "Leap")

Once you have a plan, start executing. This phase is about iterative, test-driven development.

### 2.1 Generate Tests First (TDD)

Ask Claude to write your safety net **before** writing the feature code.

**Example flow:**

```bash
# In Plan Mode, you reviewed the plan
# Now exit Plan Mode and start coding
```

**Prompt:**
```
"Great, the plan looks good. First, create a new test file
@src/api/users.test.ts with test cases for the createUser function
we planned. It should cover:
1. Happy path - successful user creation
2. Error case - duplicate email
3. Error case - invalid email format
4. Error case - missing required fields"
```

**Why TDD:**
- Catches bugs before they happen
- Documents expected behavior
- Enables refactoring with confidence
- Forces you to think about edge cases

### 2.2 Implement and Fix

Run the (failing) tests and feed the output directly to Claude.

```bash
npm run test
```

**Prompt:**
```
"I ran npm run test and got this failure:

[paste test output]

Update @src/api/users.ts to fix this and make the tests pass."
```

**Iterative development loop:**
1. Claude writes code
2. You run tests
3. Tests fail (or reveal edge cases)
4. Paste failure to Claude
5. Claude fixes
6. Repeat until green

### 2.3 Use Extended Thinking for Hard Problems

If you hit a complex bug or architectural challenge, enable **Extended Thinking**. This gives Claude more processing time before answering.

**When to use Extended Thinking:**
- 🧮 Complex algorithms or math
- 🏗️ Architecture decisions with tradeoffs
- 🐛 Hard-to-reproduce bugs
- ⚡ Performance optimization challenges
- 🔐 Security vulnerability analysis

**Example:**
```
[Enable Extended Thinking in UI]

"Analyze the race condition in @src/lib/queues/transaction.queue.ts
where multiple background jobs process the same transaction. Consider:
1. Current locking mechanism
2. Firestore transaction guarantees
3. Potential for distributed locks
4. Trade-offs of each approach

Recommend the best solution for our use case."
```

### 2.4 Work with Visuals

Don't just describe UI issues—**show them**.

**Methods:**
- Drag-and-drop screenshots into the terminal
- Paste images (Ctrl+V / Cmd+V)
- Share design mockups

**Example:**
```
[Paste screenshot of misaligned button]

"The login button looks like this. Here is my code in
@src/components/LoginButton.tsx. Generate the Tailwind CSS to:
1. Fix the alignment
2. Match the design system in @PROJECT_CONTEXT.md
3. Ensure it's mobile responsive"
```

**Visual debugging use cases:**
- UI bugs (alignment, spacing, colors)
- Design implementation (mockup → code)
- Responsive issues (mobile vs desktop)
- Accessibility problems (contrast, sizing)

---

## 📦 Phase 3: Commit & Automate (The "Land")

With the feature working, package it and automate repetitive tasks.

### 3.1 Create Pull Requests

Once your code is ready and committed, ask Claude to write the PR.

**Prompt:**
```
"Summarize my changes on this branch and create a pull request.
Title it 'feat: Add user management endpoint'. Include:
1. Summary of changes
2. Testing done
3. Related issue number"
```

Claude will:
- Analyze your git diff
- Write a clear PR description
- Use conventional commit format
- Include relevant details

### 3.2 Automate with Custom Slash Commands

Stop typing the same prompts. Create slash commands by adding Markdown files to `.claude/commands/`.

**Example: `.claude/commands/test.md`**

```markdown
---
description: "Finds the test file for a given file, runs it, and fixes failures."
argument-hint: [filepath]
---

Here is the file I'm working on: @$1

1. Find the corresponding test file for `$1`.
2. Run the tests in that test file.
3. If any tests fail, analyze the failure and fix the code in `$1`.
```

**Usage:**
```bash
/test src/api/users.ts
```

**More useful slash commands for YAICO:**

**`.claude/commands/add-test.md`**
```markdown
---
description: "Generate comprehensive tests for a file"
argument-hint: [filepath]
---

Create comprehensive unit tests for @$1. Include:
1. Happy path cases
2. Error cases
3. Edge cases
4. Use existing patterns from @src/__tests__/

Save to the appropriate test file location.
```

**`.claude/commands/fix-types.md`**
```markdown
---
description: "Fix all TypeScript errors in a file"
---

Run `npm run type-check`, analyze errors, and fix all TypeScript
issues. Ensure changes follow patterns in @CLAUDE.md.
```

**`.claude/commands/review.md`**
```markdown
---
description: "Code review with security and performance focus"
argument-hint: [filepath]
---

Perform a code review of @$1. Check for:
1. Security vulnerabilities (XSS, SQL injection, etc.)
2. Performance issues
3. Accessibility problems
4. TypeScript type safety
5. Adherence to project patterns in @CLAUDE.md

Provide actionable feedback.
```

**`.claude/commands/optimize.md`**
```markdown
---
description: "Optimize component for performance"
argument-hint: [filepath]
---

Analyze @$1 for performance optimizations:
1. Unnecessary re-renders
2. Missing memoization
3. Heavy computations that should be cached
4. Large bundle impact

Implement optimizations while maintaining functionality.
```

---

## 🚀 Pro-User Power-Ups

### Resume Any Session

Never lose your work. Claude saves all sessions.

```bash
# Resume your very last session
claude --continue
# or
claude -c

# Show list of recent sessions to choose from
claude --resume
```

**When to use:**
- 💡 Continue work from yesterday
- 🔄 Switch between tasks
- 📋 Review what Claude did in a past session

### Run Parallel Sessions with Git Worktrees

Need to fix an urgent bug without messing up your current feature? Use Git Worktrees.

```bash
# Create a new worktree for bugfix
git worktree add ../yaico-bugfix bugfix-branch

# Switch to the worktree
cd ../yaico-bugfix

# Start a new Claude session
claude
```

**Benefits:**
- ✅ Isolated directory (no stashing needed)
- ✅ Isolated Claude session
- ✅ Original feature branch untouched
- ✅ Can switch between sessions easily

**Cleanup when done:**
```bash
# Remove worktree
git worktree remove ../yaico-bugfix
```

### Use Claude as a Unix Utility

Pipe data directly in and out.

```bash
# Explain a git diff
git diff | claude -p "Explain this diff in bullet points"

# Analyze logs
cat error.log | claude -p "Find the root cause of these errors"

# Generate from template
cat template.json | claude -p "Fill this with sample data for testing"

# Check commit message quality
git log --oneline -5 | claude -p "Review these commit messages for clarity"
```

---

## 🎯 Complete YAICO Development Workflow

Here's how to apply this to building YAICO:

### Session 1: Authentication (Phase 2 from main guide)

```bash
# 1. Start in Plan Mode
claude --permission-mode plan
```

**Prompt:**
```
Read @CLAUDE.md and @PROJECT_CONTEXT.md.

I'm implementing Phase 2 (Authentication System) from
@YAICO_VIBE_WORKFLOW_GUIDE.md.

Analyze the requirements and create a detailed implementation plan for:
1. Firebase configuration (@src/lib/firebase/)
2. Auth context and hooks (@src/contexts/, @src/hooks/)
3. Sign-in and sign-up pages (@src/app/(auth)/)
4. Protected route middleware

Consider security best practices and the patterns in our tech stack.
```

```bash
# 2. Review plan, then exit Plan Mode and start coding
claude
```

**Prompt:**
```
The plan looks good. Let's start with tests.

Create @src/__tests__/auth-flow.test.tsx with tests for:
1. User sign up flow
2. User sign in flow
3. Sign out functionality
4. Protected route access (logged in)
5. Protected route redirect (logged out)

Use Vitest and React Testing Library.
```

**Prompt after tests are written:**
```
Now implement the Firebase configuration in @src/lib/firebase/config.ts
following the plan. Use environment variables from .env.example.
```

**Continue iteratively through each component...**

**When done:**
```
# Test the auth system
npm run dev

# Create commit
git add .
git commit -m "feat: complete authentication system"
```

### Session 2: AI Integration (Phase 4)

**Use a custom command:**

Create `.claude/commands/add-gateway.md`:
```markdown
---
description: "Create a new API gateway with standard patterns"
argument-hint: [service-name]
---

Create a new gateway for $1 in @src/server/gateways/$1.gateway.ts

Follow the patterns in @src/server/gateways/base.gateway.ts:
1. Extend BaseGateway
2. Include retry logic
3. Add response caching
4. Implement error handling
5. Add TypeScript types
6. Include mock mode for testing

Also create tests in @src/__tests__/gateways/$1.test.ts
```

**Usage:**
```bash
/add-gateway gemini
```

### Session 3: Bug Fix During Development

**Scenario:** You're working on transactions, but found an auth bug.

```bash
# Create worktree for bugfix
git worktree add ../yaico-auth-bugfix fix/auth-token-refresh

# Switch to bugfix directory
cd ../yaico-auth-bugfix

# Start isolated Claude session
claude
```

**Prompt:**
```
Bug: Auth tokens aren't refreshing properly in @src/contexts/AuthContext.tsx

Symptoms: Users get logged out after 1 hour
Expected: Tokens should refresh automatically

Debug and fix this while maintaining existing auth flow.
```

```bash
# After fix, test and commit
npm run test
git add .
git commit -m "fix: auth token auto-refresh"
git push

# Switch back to main work
cd ../yaico-app
# Your feature work is exactly as you left it!
```

---

## 📋 Best Practices Summary

### ✅ Do This

1. **Always start with CLAUDE.md** - Prime context for every session
2. **Use Plan Mode for complex tasks** - Think before coding
3. **Reference files with @** - Never paste code
4. **Write tests first** - TDD saves debugging time
5. **Use Extended Thinking for hard problems** - Let Claude think deeply
6. **Show, don't tell** - Use screenshots for UI issues
7. **Create slash commands** - Automate repetitive tasks
8. **Use worktrees for parallel work** - Stay organized
9. **Resume sessions** - Continue where you left off
10. **Commit after each phase** - Easy rollback if needed

### ❌ Don't Do This

1. **Don't paste entire files** - Use @ syntax instead
2. **Don't skip planning** - Jumping to code wastes time
3. **Don't ignore test failures** - Fix them immediately
4. **Don't work without context** - Always include relevant files
5. **Don't describe UI bugs** - Show screenshots
6. **Don't repeat prompts** - Create slash commands
7. **Don't lose work** - Use session resumption
8. **Don't stash constantly** - Use worktrees
9. **Don't skip commits** - Commit frequently
10. **Don't ignore CLAUDE.md** - Keep it updated

---

## 🎓 Learning Resources

### Video Tutorials
- [Plan Mode in Claude Code](https://www.youtube.com/watch?v=example) - Demonstrates Plan Mode workflow

### Related Documentation
- [YAICO Vibe Workflow Guide](../../YAICO_VIBE_WORKFLOW_GUIDE.md) - Full implementation guide
- [Command Reference](./COMMAND_REFERENCE.md) - All vibe commands
- [Troubleshooting Guide](./TROUBLESHOOTING.md) - Common issues

---

## 🚀 Quick Reference

### Common Commands

```bash
# Start Plan Mode
claude --permission-mode plan

# Resume last session
claude -c

# Resume from list
claude --resume

# Pipe to Claude
git diff | claude -p "Explain this"

# Run custom command
/test src/api/users.ts

# Create worktree
git worktree add ../project-bugfix bugfix-branch
```

### Essential @ Patterns

```
@src/file.ts           # Single file
@src/components/       # Directory
@src/**/*.test.ts      # Glob pattern
@CLAUDE.md             # Project context
@PROJECT_CONTEXT.md    # YAICO context
```

### Best Prompts

```
"Read @CLAUDE.md. Plan how to implement [feature]
considering [constraints]."

"Using @existing-file.ts as reference, create
@new-file.ts following the same patterns."

"I got this error: [error]. Fix @problematic-file.ts"

"Review @my-file.ts for security, performance, and
adherence to @CLAUDE.md patterns."
```

---

**Remember:** Explore → Plan → Code → Commit

Master this workflow and you'll build better software, faster. 🚀
