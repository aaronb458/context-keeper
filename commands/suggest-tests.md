---
name: suggest-tests
description: Analyze code and suggest missing tests
---

Analyze your codebase and suggest tests that should be written.

## What This Does

Scans your project to find:
- Functions/components without tests
- New code that needs test coverage
- Critical paths that should be tested
- Edge cases that might be missed

## Step 1: Detect Project Type & Test Framework

Check for test configuration:

```bash
# JavaScript/TypeScript
ls jest.config.* vitest.config.* 2>/dev/null
cat package.json | grep -E "(jest|vitest|mocha|ava)" 2>/dev/null

# Python
ls pytest.ini pyproject.toml 2>/dev/null
grep pytest pyproject.toml 2>/dev/null

# Go
ls *_test.go 2>/dev/null

# Rust
grep "\[dev-dependencies\]" Cargo.toml 2>/dev/null
```

Determine:
- Test framework (Jest, Vitest, Pytest, Go test, etc.)
- Test file pattern (`*.test.ts`, `*.spec.ts`, `test_*.py`, `*_test.go`)
- Test directory (`__tests__/`, `tests/`, `test/`)

## Step 2: Find Source Files

List all source files:

```bash
# JavaScript/TypeScript
find src lib app -name "*.ts" -o -name "*.tsx" -o -name "*.js" -o -name "*.jsx" 2>/dev/null | grep -v ".test." | grep -v ".spec." | grep -v "__tests__"

# Python
find . -name "*.py" | grep -v "test_" | grep -v "_test.py" | grep -v "__pycache__"

# Go
find . -name "*.go" | grep -v "_test.go"
```

## Step 3: Find Existing Tests

List all test files:

```bash
# JavaScript/TypeScript
find . -name "*.test.ts" -o -name "*.test.tsx" -o -name "*.spec.ts" -o -name "*.spec.tsx" 2>/dev/null

# Python
find . -name "test_*.py" -o -name "*_test.py" 2>/dev/null

# Go
find . -name "*_test.go" 2>/dev/null
```

## Step 4: Match Source to Tests

For each source file, check if corresponding test exists:

```
Source File              Test File              Status
-----------              ---------              ------
lib/auth.ts              lib/auth.test.ts       ✅ Has tests
lib/utils.ts             (none)                 ❌ No tests
components/Button.tsx    components/Button.test.tsx  ✅ Has tests
components/Modal.tsx     (none)                 ❌ No tests
```

## Step 5: Analyze Untested Files

For files without tests, analyze to suggest what should be tested:

**Read the file and identify:**
- Exported functions
- Component props/behavior
- API endpoints
- Business logic
- Edge cases (null checks, error handling)

## Step 6: Generate Test Suggestions

For each untested file, generate suggestions:

```markdown
## Missing Tests: `lib/utils.ts`

### Functions to test:
1. **formatDate(date: Date): string**
   - Test with valid date
   - Test with null/undefined
   - Test with invalid date
   - Test timezone handling

2. **parseAmount(str: string): number**
   - Test valid number strings
   - Test with currency symbols ($, €)
   - Test with commas (1,000.00)
   - Test invalid input (returns NaN or throws?)

### Suggested test file: `lib/utils.test.ts`

```typescript
import { formatDate, parseAmount } from './utils';

describe('formatDate', () => {
  it('formats valid date correctly', () => {
    expect(formatDate(new Date('2025-01-15'))).toBe('Jan 15, 2025');
  });

  it('handles null gracefully', () => {
    expect(formatDate(null)).toBe('');
  });
});

describe('parseAmount', () => {
  it('parses simple number', () => {
    expect(parseAmount('100')).toBe(100);
  });

  it('parses with currency symbol', () => {
    expect(parseAmount('$1,234.56')).toBe(1234.56);
  });
});
```
```

## Step 7: Display Results

Show summary to user:

```
🧪 Test Coverage Analysis

## Summary
- Source files: 24
- Files with tests: 18
- Files without tests: 6
- Coverage: 75%

## Files Missing Tests:

### Priority 1 - Critical (business logic)
❌ lib/auth.ts - Authentication functions
❌ lib/payments.ts - Payment processing

### Priority 2 - Important (utilities)
❌ lib/utils.ts - Helper functions
❌ lib/validation.ts - Input validation

### Priority 3 - Nice to have (UI)
❌ components/Modal.tsx - Modal component
❌ components/Toast.tsx - Toast notifications

---

Would you like me to:
1. Generate test file for a specific file?
2. Show detailed suggestions for all files?
3. Create all missing test files?
```

## Step 8: Generate Test File (On Request)

If user wants tests generated:

```
/suggest-tests --generate lib/auth.ts
```

1. Read the source file
2. Analyze exports and logic
3. Generate complete test file
4. Write to appropriate location
5. Show the generated tests

## Quick Commands

```bash
# Analyze entire project
/suggest-tests

# Analyze specific file
/suggest-tests lib/auth.ts

# Generate tests for file
/suggest-tests --generate lib/auth.ts

# Show only files without tests
/suggest-tests --missing

# Show test coverage stats
/suggest-tests --stats
```

## Test Priority Logic

**Priority 1 - Critical:**
- Authentication code
- Payment processing
- Data validation
- Security functions
- Core business logic

**Priority 2 - Important:**
- Utility functions
- API handlers
- Data transformations
- State management

**Priority 3 - Nice to have:**
- UI components
- Formatting helpers
- Static content

## Auto-Suggestion

When you write new code, Claude should prompt:

"I notice you created `lib/newFeature.ts`. Would you like me to suggest tests? `/suggest-tests lib/newFeature.ts`"

## Example Output

```
🧪 Test Suggestions for: lib/auth.ts

## Exported Functions:

### 1. `validateToken(token: string): boolean`
**Tests needed:**
- Valid JWT token → returns true
- Expired token → returns false
- Malformed token → returns false or throws
- Empty string → returns false
- Null/undefined → handles gracefully

### 2. `generateToken(user: User): string`
**Tests needed:**
- Valid user → returns JWT string
- User with all fields → token contains claims
- Token is valid JWT format
- Token expiry is set correctly

### 3. `refreshToken(oldToken: string): string`
**Tests needed:**
- Valid refresh token → returns new access token
- Expired refresh token → throws/returns null
- Blacklisted token → rejected

## Suggested Test File:

Location: `lib/auth.test.ts`
Framework: Jest
Estimated tests: 12-15

Would you like me to generate this test file?
```

Done!
