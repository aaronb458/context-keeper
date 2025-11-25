---
name: log-decision
description: Log an architecture decision record (ADR) to track why choices were made
---

Log an important technical or architectural decision for future reference.

## What This Does

Creates or updates a decision log that tracks:
- **What** was decided
- **Why** it was chosen
- **What alternatives** were considered
- **Trade-offs** accepted

Decisions are stored in `DECISIONS.md` and also added to `CHANGELOG.md`.

## Step 1: Check for Decisions File

```bash
ls DECISIONS.md 2>/dev/null
```

If doesn't exist, create it with header:

```markdown
# Decision Log

This file tracks important technical and architectural decisions made in this project.

Format: ADR (Architecture Decision Record)

---

```

## Step 2: Gather Decision Information

Ask the user (or extract from recent conversation):

**Required:**
1. **Title**: Short name for the decision (e.g., "Use JWT for authentication")
2. **Context**: What problem or situation prompted this decision?
3. **Decision**: What was decided?
4. **Reason**: Why was this chosen?

**Optional:**
5. **Alternatives Considered**: What other options were evaluated?
6. **Trade-offs**: What downsides were accepted?
7. **Related Files**: Which files implement this decision?

## Step 3: Generate Decision Entry

Create entry in ADR format:

```markdown
## [NUMBER]. [Title]

**Date:** YYYY-MM-DD
**Status:** Accepted

### Context
[What situation or problem required a decision?]

### Decision
[What was decided?]

### Reason
[Why was this chosen over alternatives?]

### Alternatives Considered
- **[Alternative 1]**: [Why rejected]
- **[Alternative 2]**: [Why rejected]

### Trade-offs
- [Downside 1 that was accepted]
- [Downside 2 that was accepted]

### Related Files
- `path/to/file.ts` - [How it relates]
- `path/to/other.ts` - [How it relates]

---
```

## Step 4: Determine Decision Number

Count existing decisions in DECISIONS.md and increment:

```bash
grep -c "^## [0-9]" DECISIONS.md 2>/dev/null || echo "0"
```

New decision number = count + 1

## Step 5: Append to DECISIONS.md

Add the new decision entry to the file.

## Step 6: Also Log to CHANGELOG.md

If CHANGELOG.md exists, add to today's entry under "### Decisions":

```markdown
### Decisions
- **[Title]**: [One-line summary of decision and reason]
```

## Step 7: Confirm to User

Display:

```
✅ Decision logged!

## [NUMBER]. [Title]
**Reason:** [Short reason]
**Trade-offs:** [Key trade-off]

Saved to: DECISIONS.md
Also added to: CHANGELOG.md

View all decisions: cat DECISIONS.md
```

## Quick Mode

If user provides decision inline, extract and log directly:

**User says:** "/log-decision Use PostgreSQL over MongoDB because we need relational data"

**Extract:**
- Title: "Use PostgreSQL over MongoDB"
- Decision: Use PostgreSQL
- Reason: Need relational data
- Alternatives: MongoDB

**Then generate full entry and confirm.**

## Examples

### Example 1: Authentication Decision
```
/log-decision

Title: JWT authentication with refresh tokens
Context: Need stateless auth for API-first architecture
Decision: Use JWT with short-lived access tokens and refresh tokens
Reason: Better for mobile apps, scales horizontally
Alternatives: Session-based auth (rejected: requires sticky sessions)
Trade-offs: More complex token refresh logic
```

### Example 2: Database Decision
```
/log-decision Use PostgreSQL because we have complex relational data with many joins

→ Auto-generates:
Title: Use PostgreSQL for database
Decision: PostgreSQL over NoSQL options
Reason: Complex relational data with many joins
Alternatives: MongoDB, DynamoDB
Trade-offs: Less flexible schema changes
```

### Example 3: Framework Decision
```
/log-decision

We decided to use Next.js App Router instead of Pages Router because
we want server components for better performance. The trade-off is
less community resources since it's newer.

→ Auto-extracts and generates full ADR
```

## Auto-Detection

When working in conversation, if you detect decision-making language:
- "Let's go with..."
- "I think we should use..."
- "The better approach is..."
- "We decided to..."

Prompt: "Would you like me to log this as a decision? `/log-decision`"

## Notes

- Keep decisions concise but complete
- Focus on the "why" - the reasoning is most valuable later
- Include enough context that future-you understands
- Link to related files when possible
- Decisions can be updated - add "**Updated:** date" if revisited

Done!
