# YAICO Custom Claude Commands

This directory contains custom slash commands for Claude Code to automate common YAICO development tasks.

## 📋 Available Commands

### Testing & Quality

**`/test [filepath]`**
- Finds and runs tests for a specific file
- Analyzes failures and fixes the code
- Re-runs tests to verify fixes

```bash
/test src/lib/tax/vat-calculator.ts
```

**`/add-test [filepath]`**
- Generates comprehensive unit tests for a file
- Includes happy path, error cases, and edge cases
- Follows YAICO testing patterns

```bash
/add-test src/components/ui/Button.tsx
```

**`/fix-types`**
- Runs TypeScript type checking
- Fixes all type errors
- Follows strict mode patterns

```bash
/fix-types
```

---

### Code Review & Optimization

**`/review [filepath]`**
- Comprehensive code review
- Checks: security, performance, accessibility, Dutch business logic
- Provides prioritized feedback

```bash
/review src/server/actions/transaction.actions.ts
```

**`/optimize [filepath]`**
- Analyzes performance bottlenecks
- Implements optimizations (memoization, lazy loading, etc.)
- Provides before/after benchmarks

```bash
/optimize src/components/dashboard/TransactionList.tsx
```

---

### Code Generation

**`/add-gateway [service-name]`**
- Creates a new API gateway following YAICO patterns
- Extends BaseGateway with retry/caching
- Includes tests and environment variables

```bash
/add-gateway openai
```

**`/add-component [component-name]`**
- Creates a new UI component following design system
- Includes variants, accessibility, TypeScript types
- Updates exports automatically

```bash
/add-component Tooltip
```

---

## 🚀 Usage Examples

### Typical Development Workflow

```bash
# 1. Create a new feature component
/add-component DBAMonitor

# 2. Add comprehensive tests
/add-test src/components/compliance/DBAMonitor.tsx

# 3. Run tests and fix failures
/test src/components/compliance/DBAMonitor.tsx

# 4. Review for quality and security
/review src/components/compliance/DBAMonitor.tsx

# 5. Optimize performance
/optimize src/components/compliance/DBAMonitor.tsx

# 6. Fix any type errors
/fix-types
```

### Adding External Integrations

```bash
# Create a new gateway for an external API
/add-gateway mailchimp

# Test the gateway
/test src/server/gateways/mailchimp.gateway.ts

# Review for security
/review src/server/gateways/mailchimp.gateway.ts
```

---

## 📝 Creating Your Own Commands

To create a custom command:

1. Create a new `.md` file in this directory
2. Add frontmatter with description and arguments
3. Write the prompt/instructions

**Example: `.claude/commands/my-command.md`**

```markdown
---
description: "Short description of what this command does"
argument-hint: [arg1] [arg2]
---

Instructions for Claude to follow when this command is run.

You can reference files with @$1, @$2 for arguments.
You can reference any file with @src/path/to/file.ts
```

**Usage:**
```bash
/my-command value1 value2
```

---

## 🎯 Command Best Practices

### Do This ✅

- Reference files with `@filepath` syntax
- Use variables `$1`, `$2` for arguments
- Include clear success criteria
- Provide examples in the command
- Follow YAICO patterns from `@CLAUDE.md`

### Avoid This ❌

- Pasting entire files (use @ syntax)
- Vague instructions
- Skipping error handling
- Ignoring project conventions

---

## 📚 Related Documentation

- [Claude Code Workflow Guide](../../docs/guides/CLAUDE_CODE_WORKFLOW.md) - Complete workflow documentation
- [Command Reference](../../docs/guides/COMMAND_REFERENCE.md) - All vibe commands
- [CLAUDE.md](../../CLAUDE.md) - Project context for Claude

---

## 💡 Tips

1. **Chain commands**: Run multiple commands in sequence for complex tasks
2. **Use with Plan Mode**: Start with planning, then use commands for execution
3. **Customize**: Copy and modify existing commands for your specific needs
4. **Share**: If you create useful commands, share them with the team!

---

**Last Updated**: 2025-11-15
**Commands**: 7 custom commands available
