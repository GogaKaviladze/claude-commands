Document Feature
Generate comprehensive documentation for a new feature — both a technical developer reference and a user-friendly guide.
Usage
/document-feature <feature-name>
Example: /document-feature password-reset
---
Instructions
You are a documentation generator. Follow every step below in order.
Step 1 — Resolve the feature name
The feature name is: $ARGUMENTS
Convert it to:
FEATURE_SLUG: kebab-case (e.g. password-reset)
FEATURE_TITLE: Title Case (e.g. Password Reset)
FEATURE_CLASS: PascalCase (e.g. PasswordReset)
---
Step 2 — Detect the feature layer
Search the codebase to determine whether this is a frontend, backend, or full-stack feature. Use these heuristics:
Backend only: feature code exists under src/api/, src/server/, app/controllers/, lib/, etc., with no corresponding UI components.
Frontend only: feature code exists under src/components/, src/pages/, src/views/, app/frontend/, etc., with no server-side routes or models.
Full-stack: feature spans both layers (routes/controllers AND components/pages), or you find matching files in both frontend and backend directories.
Record this as FEATURE_LAYER (one of: backend, frontend, full-stack).
---
Step 3 — Gather code context
Run the following searches and read the most relevant files (aim for 5–15 files total):
Glob search for files whose path or name contains the feature slug or class name.
Grep for the feature class name, primary export name, or key function names across .ts, .tsx, .js, .jsx, .py, .rb, .go files.
Read the top matches — prioritise:
Route/controller definitions
Model/schema definitions
Primary component or service files
Existing tests (reveal expected behavior and edge cases)
Any existing docs (avoid duplicating; cross-reference instead)
Also check for:
docs/dev/ and docs/user/ directories to understand existing documentation style
A README.md or CONTRIBUTING.md for project conventions
An openapi.yaml / swagger.json for API contracts
---
Step 4 — Build the internal knowledge model
Before writing anything, summarise what you found:
FEATURE_LAYER: <backend|frontend|full-stack>
PURPOSE: <one sentence>
ENTRY_POINTS: <list of key files / routes / components>
PUBLIC_API: <endpoints, props, exported functions — name, signature, description>
DATA_MODELS: <types / schemas involved>
CONFIG: <env vars, feature flags, config keys>
ERROR_CASES: <known failure modes from code or tests>
DEPENDENCIES: <notable internal modules or external packages>
RELATED_DOCS: <paths to existing docs that should be cross-referenced>
---
Step 5 — Generate developer documentation
Create the file docs/dev/$FEATURE_SLUG-implementation.md.
Adjust sections based on FEATURE_LAYER:
Omit "API Reference" for pure frontend features.
Omit "Component API" for pure backend features.
Include both for full-stack features.
# $FEATURE_TITLE — Developer Reference

> **Layer:** $FEATURE_LAYER  
> **Status:** In Development  
> **Last Updated:** <YYYY-MM-DD>

## Overview

<2–4 sentences: what this feature does and why it exists>

## Architecture

<How this feature fits into the broader system. Include a text diagram if helpful.>

### Key Files

| File | Role |
|------|------|
| `<path>` | <what it does> |

---

## API Reference
<!-- Include only for backend and full-stack features -->

### `<METHOD> <path>`

**Description:** <what this endpoint does>  
**Authentication:** <required / optional / none>

**Request**

\`\`\`json
// Headers
{ "Authorization": "Bearer <token>" }

// Body
{ "<field>": "<type> — <description>" }
\`\`\`

**Responses**

| Status | Meaning | Body |
|--------|---------|------|
| `200` | Success | `{ ... }` |
| `400` | Validation error | `{ "error": "..." }` |
| `401` | Unauthenticated | `{ "error": "Unauthorized" }` |
| `404` | Not found | `{ "error": "Not found" }` |

<!-- Repeat block for each endpoint -->

---

## Component API
<!-- Include only for frontend and full-stack features -->

### `<ComponentName>`

**File:** `<path>`

**Props**

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `<name>` | `<type>` | Yes/No | `<default>` | <description> |

**Usage**

\`\`\`tsx
<ComponentName prop1="value" onSuccess={(r) => console.log(r)} />
\`\`\`

---

## Data Models

\`\`\`typescript
interface <ModelName> {
  id: string;
  // ...
}
\`\`\`

---

## Configuration

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `<ENV_VAR>` | string | — | <what it controls> |

---

## Error Handling

| Error | Trigger | Recommended Handling |
|-------|---------|----------------------|
| `<ErrorName>` | <when it occurs> | <how callers should handle it> |

---

## Testing

\`\`\`bash
# Run tests scoped to this feature
<test command>
\`\`\`

Key scenarios:
- <scenario 1>
- <scenario 2>
- <edge case>

---

## Implementation Notes

<Gotchas, architectural decisions, known limitations, or anything a future maintainer needs to know.>

---

## Related Documentation

- [User Guide — $FEATURE_TITLE](../../user/$FEATURE_SLUG-guide.md)
<links to any RELATED_DOCS found in Step 3>
---
Step 6 — Generate user documentation
Create the file docs/user/$FEATURE_SLUG-guide.md.
Write in plain language — no jargon, no code blocks. Address the reader as "you".
# How to Use $FEATURE_TITLE

> **Last Updated:** <YYYY-MM-DD>

## What Is $FEATURE_TITLE?

<1–2 sentences: what this does for the user, in plain language>

---

## Before You Start

- <Prerequisite 1, e.g. "You must be logged in">
- <Prerequisite 2, e.g. "You need Admin or Editor role">

---

## Step-by-Step Guide

### Step 1 — <Action title>

<Plain-language instruction.>

![Screenshot: <describe what this screenshot should show>](../assets/$FEATURE_SLUG/step-01.png)
<!-- TODO: Replace placeholder with actual screenshot -->

---

### Step 2 — <Action title>

<Plain-language instruction.>

![Screenshot: <describe what this screenshot should show>](../assets/$FEATURE_SLUG/step-02.png)
<!-- TODO: Replace placeholder with actual screenshot -->

---

### Step 3 — <Action title>

<Plain-language instruction.>

![Screenshot: <describe what this screenshot should show>](../assets/$FEATURE_SLUG/step-03.png)
<!-- TODO: Replace placeholder with actual screenshot -->

<!-- Add or remove steps to match the actual feature flow -->

---

## Common Questions

**Q: <Likely question based on the feature's error cases>**  
A: <Plain-language answer>

**Q: <Another likely question>**  
A: <Answer>

---

## Troubleshooting

| Problem | What to Try |
|---------|-------------|
| <Symptom from known error cases> | <Simple fix> |
| <Another symptom> | <Fix> |

---

## Need More Help?

If you're still stuck, contact support or visit our [Help Center](#).

---

## Related Guides

<Links to closely related user docs found in Step 3>

---

*For technical details, see the [Developer Reference](../../dev/$FEATURE_SLUG-implementation.md).*
---
Step 7 — Verify and report
Re-read both files and confirm:
All <placeholder> text has been filled in (screenshot TODO comments and asset paths are intentional exceptions).
<YYYY-MM-DD> has been replaced with today's actual date.
Cross-reference links between the two files are correct and consistent.
FEATURE_LAYER logic was applied correctly (right sections included/excluded).
Then print this summary:
✅ Documentation generated for: $FEATURE_TITLE ($FEATURE_LAYER)

📄 Developer docs → docs/dev/$FEATURE_SLUG-implementation.md
📘 User guide     → docs/user/$FEATURE_SLUG-guide.md

Screenshot placeholders : <N> (see TODO comments in user guide)
Related docs linked     : <list paths, or "none found">

Next steps:
- Replace screenshot placeholders in docs/user/$FEATURE_SLUG-guide.md
- Review API / component signatures for accuracy
- Add to your docs index if one exists
