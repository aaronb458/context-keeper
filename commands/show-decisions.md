---
name: show-decisions
description: Display all logged architecture decisions
---

Show all architecture decisions logged in this project.

## Step 1: Check for Decisions File

```bash
ls DECISIONS.md 2>/dev/null
```

If doesn't exist:
- Tell user: "No decisions logged yet. Use `/log-decision` to log your first decision."
- Exit

## Step 2: Read and Parse Decisions

```bash
cat DECISIONS.md
```

Parse the file to extract:
- Total number of decisions
- List of decision titles with dates
- Most recent decisions

## Step 3: Display Summary

Show a formatted summary:

```
📋 Decision Log: [Project Name]

Total Decisions: [N]

## Recent Decisions:

[N]. [Title] (YYYY-MM-DD)
    → [One-line summary of decision]

[N-1]. [Title] (YYYY-MM-DD)
    → [One-line summary of decision]

[N-2]. [Title] (YYYY-MM-DD)
    → [One-line summary of decision]

---

View full details: cat DECISIONS.md
Log new decision: /log-decision
```

## Step 4: Optional - Filter by Keyword

If user provides a keyword:

```
/show-decisions auth
```

Filter decisions containing "auth" and display only matching ones.

## Step 5: Optional - Show Full Decision

If user asks for specific decision:

```
/show-decisions 3
```

Display full details of decision #3.

Done!
