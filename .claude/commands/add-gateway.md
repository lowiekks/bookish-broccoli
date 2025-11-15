---
description: "Create a new API gateway with standard patterns"
argument-hint: [service-name]
---

Create a new gateway for **$1** following YAICO gateway patterns.

**1. Create Gateway File**

File: `src/server/gateways/$1.gateway.ts`

Follow the pattern from @src/server/gateways/base.gateway.ts:
- Extend `BaseGateway` abstract class
- Include retry logic with exponential backoff (inherited)
- Add response caching (inherited)
- Implement circuit breaker pattern (inherited)
- Add proper error handling
- Use TypeScript generics for type safety

**2. Implement Methods**

Add service-specific methods:
- Use `this.withRetry()` for API calls
- Return `GatewayResponse<T>` type
- Add proper TypeScript types for requests/responses
- Include environment variable validation
- Add mock mode for testing (when API key missing)

**3. Create Tests**

File: `src/__tests__/gateways/$1.test.ts`

Test cases:
- Successful API call
- Retry on failure
- Circuit breaker activation
- Response caching
- Mock mode behavior
- Error handling

**4. Environment Variables**

Add to `.env.example`:
```
# $1 API Configuration
${1^^}_API_KEY=your-api-key-here
${1^^}_API_URL=https://api.example.com
```

**5. Update Documentation**

Add usage example to @CLAUDE.md under "External Integrations" section.

Provide the complete implementation following these specifications.
