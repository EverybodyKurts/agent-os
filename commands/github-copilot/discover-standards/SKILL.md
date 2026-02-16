---
name: discover-standards
description: Extract tribal knowledge and patterns from your codebase into concise, documented standards. Use this when asked to identify or document coding patterns, conventions, and best practices from the codebase.
---

# Discover Standards

Extract tribal knowledge from your codebase into concise, documented standards.

This skill helps you identify and document patterns, conventions, and tribal knowledge from your codebase that should be captured as standards.

## Important Guidelines

- **Ask questions interactively** — Always prompt the user for input before proceeding to the next step. Present options the user can confirm, choose between, or correct
- **Write concise standards** — Use minimal words. Standards must be scannable by AI agents without bloating context windows.
- **Offer suggestions** — Don't make users think harder than necessary.
- **Be thorough** — Analyze multiple areas of the codebase and create organized folder structures (e.g., api/, backend/, frontend/, components/)
- **Process iteratively** — Work through one area at a time, then offer to continue with another area

## Process

### Step 1: Determine Focus Area

**IMPORTANT:** Always prompt the user before making assumptions.

Check if the user specified an area when running this command. If they did, skip to Step 2.

If no area was specified:

1. Analyze the codebase structure (folders, file types, patterns)
2. Identify 3-5 major areas. Examples:
   - **Frontend areas:** UI components, styling/CSS, state management, forms, routing
   - **Backend areas:** API routes, database/models, authentication, background jobs
   - **Cross-cutting:** Error handling, validation, testing, naming conventions, file structure
3. **Ask the user to select an area:**

```
I've identified these areas in your codebase:

1. **API Routes** (src/api/) — Request handling, response formats
2. **Database** (src/models/, src/db/) — Models, queries, migrations
3. **React Components** (src/components/) — UI patterns, props, state
4. **Authentication** (src/auth/) — Login, sessions, permissions

Which area should we focus on for discovering standards? (Pick one, or suggest a different area)
```

**Wait for user response before proceeding to Step 2.**

### Step 2: Analyze & Present Findings

Once an area is determined:

1. **Read key files** in that area (5-10 representative files)
2. **Look for patterns** that are:
   - **Unusual or unconventional** — Not standard framework/library patterns
   - **Opinionated** — Specific choices that could have gone differently
   - **Tribal** — Things a new developer wouldn't know without being told
   - **Consistent** — Patterns repeated across multiple files

3. **Present findings and ask user to select:**

```
I analyzed [area] and found these potential standards worth documenting:

1. **API Response Envelope** — All responses use { success, data, error } structure
2. **Error Codes** — Custom error codes like AUTH_001, DB_002 with specific meanings
3. **Pagination Pattern** — Cursor-based pagination with consistent param names

Which would you like to document?

Options:
- "Yes, all of them"
- "Just 1 and 3"
- "Add: [your suggestion]"
- "Skip this area"
```

**Wait for user selection before proceeding to Step 3.**

### Step 3: Ask Why, Then Draft Each Standard

**IMPORTANT:** For each selected standard, you MUST complete this full loop before moving to the next standard:

1. **Ask 1-2 clarifying questions** about the "why" behind the pattern
2. **Wait for user response**
3. **Draft the standard** incorporating their answer
4. **Ask user to confirm** before creating the file
5. **Create the file** if approved

Example questions to ask (adapt based on the specific standard):

- "What problem does this pattern solve? Why not use the default/common approach?"
- "Are there exceptions where this pattern shouldn't be used?"
- "What's the most common mistake a developer or agent makes with this?"

**Do NOT batch all questions upfront.** Process one standard at a time through the full loop.

**Do NOT skip the questioning step.** Understanding the "why" is critical for creating useful standards.

### Step 4: Create the Standard File

For each standard (after completing Step 3's Q&A):

1. **Determine the appropriate folder** (create if needed):
   - `api/` — API endpoints, request/response formats, authentication
   - `backend/` — Server-side patterns, business logic, data processing
   - `frontend/` — UI patterns, state management, routing
   - `components/` — Reusable UI components, component structure
   - `database/` — Schema design, queries, migrations, data models
   - `testing/` — Test patterns, mocking, coverage
   - `global/` — Cross-cutting concerns, naming conventions, file structure
   
   **Create subdirectories to organize standards by domain.** Don't put all standards in the root.

2. Check if a related standard file already exists — append to it if so

3. **Draft the content and ask user to confirm:**

```
Here's the draft for api/response-format.md:

---
# API Response Format

All API responses use this envelope:

\`\`\`json
{ "success": true, "data": { ... } }
{ "success": false, "error": { "code": "...", "message": "..." } }
\`\`\`

- Never return raw data without the envelope
- Error responses must include both code and message
- Success responses omit the error field entirely
---

Create this file? (yes / edit: [your changes] / skip)
```

4. Create or update the file in `agent-os/standards/[folder]/[standard-name].md`
5. **Then repeat Steps 3-4 for the next selected standard**

### Step 5: Update the Index

**CRITICAL:** After all standards for the current area are created, you MUST update the index file.

1. Scan `agent-os/standards/` for all `.md` files
2. For each new file without an index entry, **ask user to confirm description:**

```
New standard needs an index entry:
  File: api/response-format.md

Suggested description: "API response envelope structure and error format"

Accept this description? (yes / or type a better one)
```

3. **Create or update `agent-os/standards/index.yml`:**

```yaml
api:
  response-format:
    description: API response envelope structure and error format
  error-codes:
    description: Custom error code patterns for API responses

backend:
  data-validation:
    description: Input validation and sanitization patterns
```

**Alphabetize entries by folder name, then by filename within each folder.**

4. If `index.yml` doesn't exist, create it with proper YAML structure

### Step 6: Offer to Continue

**IMPORTANT:** After completing standards for one area, always offer to continue with another area.

**Ask the user:**

```
Standards created for [area]:
- api/response-format.md
- api/error-codes.md

I can discover standards in other areas of your codebase. Would you like to:
1. Continue discovering standards in another area
2. Finish for now

What would you like to do?
```

If the user chooses to continue:
- Return to Step 1 with remaining areas
- Process the next area through all steps
- Keep iterating until the user is satisfied or all major areas are covered

**The goal is comprehensive coverage across multiple domains (api, backend, frontend, components, etc.), not just a single area.**

## Output Location

**All standards:** `agent-os/standards/[folder]/[standard-name].md`
**Index file:** `agent-os/standards/index.yml`

### Critical Requirements

1. **Always create the index.yml file** — This file is required for the `/inject-standards` skill to work properly
2. **Organize into folders** — Create subdirectories like `api/`, `backend/`, `frontend/`, `components/` to keep standards organized
3. **Be thorough** — Aim to create multiple standards across multiple areas, not just 1-2 files
4. **Update index after each area** — Don't wait until the end; update index.yml after completing each area's standards

## Writing Concise Standards

Standards will be injected into AI context windows. Every word costs tokens. Follow these rules:

- **Lead with the rule** — State what to do first, explain why second (if needed)
- **Use code examples** — Show, don't tell
- **Skip the obvious** — Don't document what the code already makes clear
- **One standard per concept** — Don't combine unrelated patterns
- **Bullet points over paragraphs** — Scannable beats readable

**Good:**
```markdown
# Error Responses

Use error codes: `AUTH_001`, `DB_001`, `VAL_001`

\`\`\`json
{ "success": false, "error": { "code": "AUTH_001", "message": "..." } }
\`\`\`

- Always include both code and message
- Log full error server-side, return safe message to client
```

**Bad:**
```markdown
# Error Handling Guidelines

When an error occurs in our application, we have established a consistent pattern for how errors should be formatted and returned to the client. This helps maintain consistency across our API and makes it easier for frontend developers to handle errors appropriately...
[continues for 3 more paragraphs]
```

## GitHub Copilot Sub-Agent Usage

When processing large codebases or when the user wants comprehensive analysis:

1. **Suggest using sub-agents** to analyze different areas in parallel:
   ```
   I can use sub-agents to analyze multiple areas simultaneously:
   - Sub-agent 1: Analyze API patterns
   - Sub-agent 2: Analyze frontend components  
   - Sub-agent 3: Analyze backend services
   
   This will be faster. Would you like me to do this?
   ```

2. **Coordinate sub-agent findings** by collecting their results and presenting a unified list of standards to document

3. **Still follow the interactive process** even when using sub-agents — ask the user for confirmation and clarification at each step

## Example Complete Workflow

Here's what a successful session should look like:

1. **User:** "Discover standards in my codebase"
2. **Agent:** Analyzes codebase, identifies 5 areas (API, Backend, Frontend, Database, Testing)
3. **Agent:** Asks user which area to start with
4. **User:** "Let's start with API"
5. **Agent:** Analyzes API code, finds 8 potential standards, asks user which to document
6. **User:** "All of them"
7. **Agent:** For each standard:
   - Asks why questions
   - Drafts standard
   - Asks for confirmation
   - Creates file in `agent-os/standards/api/[standard-name].md`
8. **Agent:** Updates `agent-os/standards/index.yml` with all 8 standards
9. **Agent:** "Created 8 API standards. Continue with another area?"
10. **User:** "Yes, do Backend next"
11. **Agent:** Repeats process for Backend area
12. ... continues until user is satisfied

**Final result:** 20-30+ standards organized across multiple folders (api/, backend/, frontend/, database/, testing/) with a complete index.yml file.
