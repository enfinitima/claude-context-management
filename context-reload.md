---
description: Intelligently reload project context from versioned CONTEXT files with structured understanding
---

# Context Reload Command

You are about to reload comprehensive project context from a saved state. This is a **multi-step guided process** designed to help you understand and internalize the context efficiently.

## Step 1: Discover the Latest Context File

First, determine which context file to load:

1. List all context files with proper version sorting:
   ```bash
   ls CONTEXT*.md 2>/dev/null | sort -V
   ```

2. Select the **latest version** (the last file in the sorted list):
   ```bash
   ls CONTEXT*.md 2>/dev/null | sort -V | tail -1
   ```
   
   This uses version-aware sorting (`sort -V`) which correctly handles:
   - CONTEXT.md
   - CONTEXT-v2.md
   - CONTEXT-v3.md
   - CONTEXT-v10.md (correctly placed after v9, not v1)

3. If no context files exist, inform the user and suggest using `/context-dump` first

**Custom file override (if provided):**
$ARGUMENTS

If the user provided a specific filename in $ARGUMENTS, use that file instead of auto-detection.

## Step 2: Progressive Context Loading with Chain of Thought

Now, read the context file **section by section** using this structured approach. After reading each section, **pause and synthesize** your understanding before moving to the next.

### Phase 1: Architectural Understanding
**Read:** The "ARCHITECTURAL DECISIONS" section

**Think about:**
- What are the core technologies, frameworks, and libraries in use?
- What are the key architectural patterns (microservices, monolith, event-driven, etc.)?
- What are the major system components and how do they interact?
- What integration points exist (APIs, databases, external services)?
- What design trade-offs were made and why?

**Synthesize:** In 2-3 sentences, summarize the system architecture mentally. You should now understand the "shape" of the system.

---

### Phase 2: Current State Assessment
**Read:** The "CURRENT IMPLEMENTATION STATE" section

**Think about:**
- What files have been modified, created, or deleted?
- What features or components are complete vs in-progress?
- What is the current state of each major subsystem?
- Are there any partial implementations that need completion?
- Which areas are stable vs actively being changed?

**Synthesize:** Map the current implementation state onto your architectural understanding. You should now know WHERE the project currently stands.

---

### Phase 3: Strategic Context
**Read:** The "CURRENT PLAN & PROGRESS" section

**Think about:**
- What is the overall goal or objective?
- What has been completed so far?
- What milestones have been reached?
- What is the current phase (planning, implementation, testing, deployment)?
- How much of the plan remains (estimate if provided)?

**Synthesize:** You should now understand the project's DIRECTION and MOMENTUM. You know the destination and how far along the journey you are.

---

### Phase 4: Problem Awareness
**Read:** The "OPEN ISSUES & BLOCKERS" section

**Think about:**
- What are the active problems or bugs?
- What is blocking forward progress?
- Are there dependencies on external factors?
- What edge cases or scenarios need special attention?
- Are there any critical errors or failures documented?
- What external dependencies exist (APIs, services, decisions)?

**Synthesize:** You should now be AWARE of the known landmines and obstacles. This will help you avoid repeating mistakes or going down already-failed paths.

---

### Phase 5: Action Planning
**Read:** The "NEXT STEPS & TODO" section

**Think about:**
- What is the prioritized list of tasks?
- What specific files or functions need work?
- What is the recommended order of operations?
- Are there any dependencies between tasks (task A must complete before task B)?
- What testing or verification is needed?

**Synthesize:** You should now have a clear ACTION PLAN. You know exactly what needs to be done next and in what order.

---

### Phase 6: Knowledge Integration
**Read:** The "KEY LEARNINGS & GOTCHAS" section

**This section is CRITICAL - read it carefully!**

**Think about:**
- What surprising behaviors or quirks exist in the system?
- What debugging insights were discovered?
- Are there performance considerations or optimization opportunities?
- What patterns or code snippets should be reused?
- **What approaches FAILED and should NOT be tried again?**
- **What anti-patterns or pitfalls must be avoided?**
- What non-obvious issues cost significant time?

**Synthesize:** You should now have TRIBAL KNOWLEDGE - the hard-won insights that prevent wasted time. Pay special attention to the "what NOT to do" items, as these represent failed attempts that should not be repeated.

---

### Phase 7: Quality Assurance Status
**Read:** The "TESTING & VERIFICATION STATUS" section

**Think about:**
- What has been tested and verified?
- What test coverage exists?
- Are there failing tests that need attention?
- What manual testing procedures are needed?
- What still needs to be tested?
- What test data or environments are required?

**Synthesize:** You should now understand the QUALITY STATE and what verification work remains. You know what's been validated vs what assumptions still need testing.

---

## Step 3: Context Integration Summary

After completing all phases, provide a **structured summary** to confirm your understanding:

```markdown
## Context Loaded Successfully ✓

**Source:** [filename loaded]
**Generated:** [timestamp from file]
**Git Branch:** [branch from file]
**Session Duration:** [duration from file if available]

### Architecture Summary
[2-3 sentences about the system architecture]

### Current Position
[2-3 sentences about where the project currently stands - what's done, what's in progress]

### Active Focus
[1-2 sentences about the main goal/objective]

### Known Issues
[List 2-3 most critical issues/blockers]

### Failed Approaches (Do NOT Repeat)
[List any approaches that were tried and failed - if documented]

### Immediate Next Steps
[List top 3 prioritized tasks with specific file/function names if provided]

---

### Ready to Resume Work

I have loaded and internalized the project context. I understand:
- ✓ The system architecture and design decisions
- ✓ What has been implemented and what remains
- ✓ The current plan and objectives
- ✓ Active problems and blockers
- ✓ What needs to be done next (and in what order)
- ✓ Key learnings and gotchas to avoid
- ✓ Failed approaches to NOT repeat
- ✓ Testing and quality status

**I am ready to continue work. What would you like me to focus on first?**
```

## Step 4: Offer Clarification

After presenting the summary, ask the user if they want:
- Clarification on any section
- More details about specific components
- To start working immediately on a specific task from the Next Steps
- To see the full context document for reference
- To discuss the failed approaches and why they didn't work

## Important Guidelines

1. **Read each section separately** - Don't just dump the entire file into a single read operation
2. **Synthesize between sections** - Build understanding progressively, don't quote large blocks
3. **Connect the dots** - Link architectural decisions to implementation state to next steps
4. **Flag inconsistencies** - If something doesn't make sense between sections, mention it in your summary
5. **Be efficient** - Summarize and synthesize. Don't reproduce the entire document.
6. **Stay focused** - The goal is to understand enough to continue work effectively, not to memorize every detail
7. **Honor the "what NOT to do"** - Pay special attention to failed approaches and anti-patterns

## Advanced Usage Notes

**For large context files:** If the context file is extremely large (>5000 lines), consider:
- Skimming less critical sections
- Focusing more deeply on "Next Steps" and "Open Issues"
- Asking the user which areas need the most attention
- Provide a condensed summary but note that details are available if needed

**For focused work:** If the user specifies a particular area of focus in their prompt:
- Give extra attention to relevant sections
- You can skim sections that aren't directly related
- Always read "Next Steps" and "Open Issues" fully
- Always read "Key Learnings & Gotchas" fully (especially failed approaches)

**For verification mode:** If you want to verify understanding:
- After loading, offer to explain specific sections in your own words
- Confirm understanding of the most complex or critical parts
- Ask if the summary matches the user's mental model

**Error handling:**
- If sections are missing from the context file, note it but continue with available sections
- If the file format doesn't match expectations, do your best to extract useful information
- If the file is corrupted or unreadable, inform the user clearly and suggest re-dumping
- If version detection fails, ask the user to specify which file to load

## Version Detection Troubleshooting

If you encounter issues finding the right file:

```bash
# Debug: Show all context files
ls -la CONTEXT*.md

# Show with version sorting
ls CONTEXT*.md | sort -V

# Manually select highest version
# (look for highest number in CONTEXT-v[N].md pattern)
```

If the auto-detection loads the wrong file, user can always specify:
```
/context-reload CONTEXT-v5.md
```

---

**Remember:** This is about building UNDERSTANDING, not just loading data. Take your time with each phase. The goal is to emerge with a clear mental model of the project that allows you to work as effectively as if you'd been in the conversation from the beginning.

**Special attention to Phase 6:** The "failed approaches" are gold - they represent lessons learned the hard way. By avoiding these, you save significant time and frustration.
