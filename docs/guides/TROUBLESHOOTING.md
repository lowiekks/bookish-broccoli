# Troubleshooting Guide

Common issues and solutions when following the YAICO Vibe Code workflow.

## 🔍 General Debugging Strategy

1. **Read the error message carefully**
2. **Check the file and line number**
3. **Review recent changes**
4. **Test in isolation**
5. **Use vibe to fix**

---

## 🐛 Common Issues

### Installation & Setup

#### Issue: `npm install` fails
**Symptoms:**
```
npm ERR! code ERESOLVE
npm ERR! ERESOLVE unable to resolve dependency tree
```

**Solutions:**
```bash
# Option 1: Use legacy peer deps
npm install --legacy-peer-deps

# Option 2: Clear cache and retry
npm cache clean --force
rm -rf node_modules package-lock.json
npm install

# Option 3: Use exact versions from workflow
# Copy package.json from Phase 1 command exactly
```

#### Issue: TypeScript not recognizing path aliases
**Symptoms:**
```
Cannot find module '@/components/ui/Button'
```

**Solutions:**
```bash
# Check tsconfig.json has paths configured
vibe "Fix TypeScript path aliases. Ensure tsconfig.json has baseUrl and paths for @/* mapping to ./src/*"

# Restart TypeScript server in VS Code
# Cmd/Ctrl + Shift + P → "TypeScript: Restart TS Server"
```

---

### Authentication Issues

#### Issue: Firebase config error
**Symptoms:**
```
Error: Firebase: No Firebase App '[DEFAULT]' has been created
```

**Solutions:**
```bash
# 1. Check .env.local exists and has values
cat .env.local

# 2. Ensure variables are prefixed correctly
# Next.js requires NEXT_PUBLIC_ prefix for client-side variables
# Example: NEXT_PUBLIC_FIREBASE_API_KEY

# 3. Restart dev server after adding env vars
npm run dev

# 4. Use vibe to fix
vibe "Fix Firebase initialization. Check environment variables are loaded correctly and Firebase app is initialized before use."
```

#### Issue: Auth state not persisting
**Symptoms:**
- User logged out on page refresh
- AuthContext shows loading forever

**Solutions:**
```bash
vibe "Fix authentication persistence. Ensure Firebase onAuthStateChanged is set up correctly in AuthContext and runs on mount."

# Check browser localStorage
# Open DevTools → Application → Local Storage
# Should see firebase:authUser
```

#### Issue: Protected routes not redirecting
**Symptoms:**
- Can access /dashboard without login
- Middleware not running

**Solutions:**
```bash
# Check middleware.ts is in root (not src/)
ls middleware.ts

# Check middleware config matches protected routes
vibe "Fix middleware to protect routes: /dashboard, /transactions, /compliance, /settings. Redirect to /sign-in if not authenticated."
```

---

### Data & Firestore Issues

#### Issue: Firestore permission denied
**Symptoms:**
```
FirebaseError: Missing or insufficient permissions
```

**Solutions:**
```bash
# For development, update Firestore rules:
# Firebase Console → Firestore → Rules

# Temporary dev rules (NOT for production):
# allow read, write: if request.auth != null;

# Use vibe to create proper rules:
vibe "Create Firestore security rules that: allow users to read/write only their own data, validate schema with request.resource.data, check authentication."
```

#### Issue: Zod validation errors
**Symptoms:**
```
ZodError: Invalid input
```

**Solutions:**
```bash
# Log the data being validated
console.log('Validating:', data)

# Check schema matches data structure
vibe "Debug Zod validation error in [file]. The data is: [paste data]. The schema is: [paste schema]. Fix the mismatch."

# Common fixes:
# - Optional fields: use .optional()
# - Nullable: use .nullable()
# - Default values: use .default(value)
```

---

### AI Integration Issues

#### Issue: Gemini API key not working
**Symptoms:**
```
Error: API key not valid
```

**Solutions:**
```bash
# 1. Verify API key in Google AI Studio
# https://makersuite.google.com/app/apikey

# 2. Check environment variable
echo $GEMINI_API_KEY

# 3. Ensure it's loaded in gateway
vibe "Fix Gemini gateway to properly load API key from environment variables. Add error message if key is missing."

# 4. Use mock mode for testing
vibe "Add mock mode to Gemini gateway that returns dummy responses when API key is not set."
```

#### Issue: AI categorization not triggering
**Symptoms:**
- Transactions stay in "processing" status forever
- No AI suggestions appear

**Solutions:**
```bash
# Check background job queue
vibe "Add logging to transaction queue processing. Log when jobs are added and when they complete."

# Test AI service directly
vibe "Create test endpoint /api/test-ai that categorizes a sample transaction and returns the result."

# Check webhook route
vibe "Debug process-transaction webhook. Add logging for received data, AI call, and database update."
```

---

### UI & Component Issues

#### Issue: Tailwind classes not working
**Symptoms:**
- Components have no styling
- Classes like `bg-blue-500` don't apply

**Solutions:**
```bash
# 1. Check Tailwind is configured
cat tailwind.config.ts

# 2. Ensure content paths include all files
# Should have: './src/**/*.{js,ts,jsx,tsx}'

# 3. Restart dev server
npm run dev

# 4. Check CSS import in layout
vibe "Ensure globals.css imports Tailwind directives and is imported in app/layout.tsx"
```

#### Issue: React Hook Form not validating
**Symptoms:**
- Form submits with invalid data
- No error messages

**Solutions:**
```bash
vibe "Fix form validation in [component]. Use react-hook-form with zodResolver. Show error messages under each field."

# Common mistakes:
# - Forgot zodResolver in useForm
# - Errors not displayed: use {errors.field?.message}
# - Mode should be 'onBlur' or 'onChange' for live validation
```

#### Issue: State not updating
**Symptoms:**
- UI doesn't reflect changes
- Components don't re-render

**Solutions:**
```bash
# Check useState usage
vibe "Debug state management in [component]. Ensure useState is used correctly and state updates trigger re-renders."

# Check object/array mutations
# BAD: array.push(item)
# GOOD: setArray([...array, item])

# Check dependencies in useEffect
vibe "Fix useEffect dependencies in [component]. Include all used variables in dependency array."
```

---

### Build & Deployment Issues

#### Issue: Build fails with type errors
**Symptoms:**
```
Type error: Property 'x' does not exist on type 'y'
```

**Solutions:**
```bash
# Run type check
npm run type-check

# Fix all errors
vibe "Fix all TypeScript errors in the build. Run tsc --noEmit and fix each error."

# Common fixes:
# - Add proper types to props
# - Use 'any' temporarily (with TODO comment)
# - Add type guards for unions
```

#### Issue: Environment variables undefined in production
**Symptoms:**
- Works locally, fails in Vercel
- API keys not found

**Solutions:**
```bash
# 1. Add env vars to Vercel dashboard
# Project Settings → Environment Variables

# 2. Redeploy after adding vars
vercel --prod

# 3. Check NEXT_PUBLIC_ prefix for client-side vars
vibe "Fix environment variable loading. Client-side variables need NEXT_PUBLIC_ prefix in Next.js."
```

#### Issue: Vercel serverless function timeout
**Symptoms:**
```
Error: Function execution timed out after 10s
```

**Solutions:**
```bash
# 1. Optimize slow operations
vibe "Optimize [function] to run faster. Add caching, reduce database calls, use parallel processing."

# 2. Increase timeout (Pro plan only)
# vercel.json:
{
  "functions": {
    "api/**/*.ts": {
      "maxDuration": 30
    }
  }
}

# 3. Move to background job
vibe "Convert [slow-operation] to background job using a queue system."
```

---

### Database Issues

#### Issue: Firestore reads quota exceeded
**Symptoms:**
```
Quota exceeded: Firestore read operations
```

**Solutions:**
```bash
# 1. Add caching
vibe "Add React Query caching to reduce Firestore reads. Cache for 5 minutes."

# 2. Use pagination
vibe "Implement pagination for transaction list. Load 20 items at a time."

# 3. Optimize queries
vibe "Optimize Firestore queries. Use indexes, limit results, avoid get() in loops."

# 4. Use realtime listeners efficiently
# Only subscribe to data you need
# Unsubscribe when component unmounts
```

---

### Performance Issues

#### Issue: Slow page loads
**Symptoms:**
- Pages take >3 seconds to load
- Large JavaScript bundles

**Solutions:**
```bash
# Analyze bundle
npm run build
npm run analyze

# Fix large dependencies
vibe "Optimize bundle size. Use dynamic imports for heavy components, tree-shake unused code."

# Use Server Components
vibe "Convert [component] to Server Component to reduce client-side JavaScript."

# Optimize images
vibe "Replace <img> with next/image for all images. Add proper sizes and loading priorities."
```

#### Issue: Slow Firestore queries
**Symptoms:**
- Dashboard takes long to load
- Queries timeout

**Solutions:**
```bash
# Add indexes
# Firebase Console → Firestore → Indexes
# Create composite index for common queries

# Denormalize data
vibe "Add aggregated data to user document to avoid complex queries. Update on transaction changes."

# Use pagination
vibe "Implement cursor-based pagination for large lists."
```

---

## 🛠️ Debug Tools

### Browser DevTools
```
Console: Check for JavaScript errors
Network: Check API calls and responses
Application: Check localStorage, cookies
Performance: Profile slow operations
```

### React DevTools
```
Components: Inspect component props and state
Profiler: Find slow renders
```

### Firebase DevTools
```
Authentication: Check current user
Firestore: View live data
```

### VS Code Extensions
```
- Error Lens: Inline TypeScript errors
- ES7 React Snippets: Quick component creation
- Tailwind CSS IntelliSense: Class autocomplete
```

---

## 🔍 Diagnostic Commands

### Check Everything
```bash
# Types
npm run type-check

# Linting
npm run lint

# Tests
npm test

# Build
npm run build

# Start production
npm start
```

### Check Specific Issues
```bash
# Check dependencies
npm list [package-name]

# Check env vars loaded
node -e "console.log(process.env)"

# Check Firestore connection
vibe "Create /api/health endpoint that checks Firebase connection and returns status."

# Check build output
du -h .next/ | tail -20
```

---

## 📞 Getting Help

### Before Asking for Help

1. ✅ Check error message carefully
2. ✅ Search error on Google
3. ✅ Check this troubleshooting guide
4. ✅ Check relevant docs section
5. ✅ Try vibe fix command
6. ✅ Isolate the problem
7. ✅ Create minimal reproduction

### When Asking for Help

Include:
- Error message (full stack trace)
- What you were trying to do
- What you expected to happen
- What actually happened
- Relevant code snippet
- Steps to reproduce

### Using Vibe for Help

```bash
# Describe the problem clearly
vibe "I'm getting [error] when [action]. The code is in [file]. Expected: [behavior]. Actual: [what happens]. Debug and fix."

# Share context
vibe "Read PROJECT_CONTEXT.md. I'm working on [feature] in Phase [X]. Getting error: [error]. Fix it."
```

---

## 💡 Prevention Tips

1. **Commit after each phase** - Easy to rollback if needed
2. **Test immediately** - Catch errors early
3. **Read error messages** - They usually tell you what's wrong
4. **Follow the workflow exactly** - Don't skip steps
5. **Keep dev server running** - See errors as they happen
6. **Use TypeScript strictly** - Catch errors before runtime
7. **Check the browser console** - Many errors show there first

---

## 📚 Related Resources

- [Full Workflow Guide](../../YAICO_VIBE_WORKFLOW_GUIDE.md)
- [Quick Start](./QUICK_START.md)
- [Command Reference](./COMMAND_REFERENCE.md)
- [Architecture](../ARCHITECTURE.md)
