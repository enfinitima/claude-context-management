---
description: Dump comprehensive project context to CONTEXT.md with automatic versioning
---

# Context Dump Command

You are tasked with creating a comprehensive project state document that will allow seamless continuation of work in a fresh Claude Code session.

## Instructions

Create a detailed document covering ALL of the following sections:

### 1. ARCHITECTURAL DECISIONS
- Document all architectural patterns, design decisions, and rationale made during this session
- Include technology choices, library selections, and integration approaches
- Note any trade-offs or alternatives that were considered and why they were rejected
- Describe how major system components interact and integrate with each other

### 2. CURRENT IMPLEMENTATION STATE
- Summarize what has been implemented so far
- List all files that were created, modified, or deleted (with brief description of changes)
- Describe the current state of each major component or feature
- Include any partial implementations or work-in-progress code
- Note which subsystems are complete vs in-progress

### 3. CURRENT PLAN & PROGRESS
- Outline the overall plan for the feature or task
- Break down completed steps vs remaining steps
- Include any milestones or checkpoints that have been reached
- Note the current phase of implementation (planning, coding, testing, etc.)
- Estimate how much of the plan remains (percentage or time if known)

### 4. OPEN ISSUES & BLOCKERS
- List any bugs, issues, or problems that were discovered but not yet resolved
- Document any blockers or dependencies that are preventing progress
- Include error messages, stack traces, or symptoms if applicable
- Note any edge cases or scenarios that need special attention
- Identify any external dependencies (APIs, services, team members, decisions)

### 5. NEXT STEPS & TODO
- Provide a clear, prioritized list of what needs to be done next
- Include specific file names, function names, or line numbers where work should continue
- Suggest the optimal order of operations for maximum efficiency
- Note any dependencies between tasks (what must be done before what)
- Add any reminders about testing, documentation, or code review needs

### 6. KEY LEARNINGS & GOTCHAS
- Document any surprising behaviors, quirks, or non-obvious aspects discovered
- Include any debugging insights that would be valuable for future work
- Note any performance considerations or optimization opportunities
- Capture any useful patterns or code snippets that should be reused
- **CRITICAL: List any approaches or solutions that were tried and FAILED (what NOT to do)**
- **Document any anti-patterns, pitfalls, or dead-ends to avoid**
- Note any "gotcha" moments that cost significant debugging time

### 7. TESTING & VERIFICATION STATUS
- List what has been tested and what still needs testing
- Document test coverage status (percentage if known, or qualitative assessment)
- Note any failing tests or tests that need to be written
- Include any manual testing procedures or verification steps required
- Describe any test data or test environments needed

## Additional Context (Optional)
$ARGUMENTS

## File Versioning

Before writing the content:

1. List all existing context files to determine the next version:
   ```bash
   ls CONTEXT*.md 2>/dev/null | sort -V
   ```

2. Determine the next filename:
   - If no files exist: Create `CONTEXT.md`
   - If `CONTEXT.md` exists but no versioned files: Create `CONTEXT-v2.md`
   - If versioned files exist: Find highest version number and increment
     - Example: If `CONTEXT-v3.md` exists, create `CONTEXT-v4.md`

3. Important: Use version-aware sorting to find the highest number, not modification time

4. Write the full context document to the determined filename

## Output Format

Structure the document with clear markdown headers, bullet points, and code blocks where appropriate. Make it easy to scan and reference.

Include a timestamp and session metadata at the top of the document:

```markdown
# Project Context Snapshot

**Generated:** [Current Date and Time]
**Session Duration:** [Approximate time spent in this session, e.g., "~2 hours"]
**Git Branch:** [Current branch name]
**Last Commit:** [Latest commit hash and message]

---

## 1. ARCHITECTURAL DECISIONS

[Content here with bullet points and clear structure]

## 2. CURRENT IMPLEMENTATION STATE

[Content here - be specific about files and changes]

## 3. CURRENT PLAN & PROGRESS

[Content here - use checkboxes for completed vs remaining if helpful]

## 4. OPEN ISSUES & BLOCKERS

[Content here - prioritize critical blockers at the top]

## 5. NEXT STEPS & TODO

[Content here - numbered list showing priority order]

## 6. KEY LEARNINGS & GOTCHAS

### What Works Well
[Positive learnings and good patterns]

### What to Avoid (Failed Approaches)
[Anti-patterns, failed solutions, dead-ends]

### Critical Gotchas
[Non-obvious issues that cost time]

## 7. TESTING & VERIFICATION STATUS

[Content here - clearly separate tested vs untested]

---

## Additional Notes

[Any context from $ARGUMENTS or other relevant information]
```

## Writing Guidelines

1. **Be Specific:** Include actual file names, function names, line numbers, error messages
2. **Be Concise:** Use bullet points, not lengthy paragraphs
3. **Be Honest:** Document failures and dead-ends clearly
4. **Be Actionable:** Make "Next Steps" concrete enough that someone can immediately act
5. **Be Complete:** Don't skip sections - if a section doesn't apply, write "N/A - [brief reason]"

## Execution

1. Gather all the information from the current conversation and codebase state
2. Run the bash command to list existing context files and determine version number
3. Create a comprehensive, well-structured context document
4. Write the document to the determined filename
5. Confirm the file has been created with:
   - The filename that was created
   - A brief 2-3 sentence summary of what was captured
   - Confirmation of the next version number for reference

Example confirmation:
```
✓ Created CONTEXT-v3.md

Captured: OAuth2 integration progress (80% complete), Cognito configuration 
challenges, token refresh implementation plan, and 3 critical blockers. 
Next dump will create CONTEXT-v4.md.
```

**Remember:** The goal is to make it possible for a fresh Claude Code session to pick up EXACTLY where this session left off, with full context and no loss of momentum. The reload command will read this file section-by-section with Chain of Thought reasoning, so structure it clearly.
