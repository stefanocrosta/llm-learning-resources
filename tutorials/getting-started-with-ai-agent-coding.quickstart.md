# AI Agentic Coding: Quick Start Guide

This quick-start guide has been written by Claude from the "getting-started" long version.

**Time to read:** 15 minutes  
**Time to first productive use:** 2-3 hours  
**Full guide:** See `ai-coding-bootstrap-guide.md` for comprehensive version

---

## The Core Philosophy (Read This First)

Learn with **constrained tools before magic tools**. You'll understand what breaks when the magic fails.

AI coding agents = **fluent junior developers who are often confidently wrong**. Your job shifts from writing to directing and reviewing.

This quick start gets you productive fast. Read the full guide when you have time.

---

## ⚠️ Security First (NON-NEGOTIABLE)

### NEVER Send to Cloud AI Models:

- ❌ MARSS NiDAR C2 code
- ❌ Tactical mission management systems
- ❌ Proprietary defense algorithms
- ❌ Customer data, PII, credentials
- ❌ Code under NDA or export control
- ❌ Security-critical implementations

**For sensitive work:** Use local models (Ollama) or traditional coding.

**When in doubt:** Ask security officer before uploading.

---

## Fast Setup (30 minutes)

### 1. Install Mistral Vibe CLI

```bash
npm install -g @mistralai/vibe
vibe --version
```

**Why Mistral first?**
- Currently FREE
- Forces you to learn fundamentals
- Decent coding capability
- CLI-only = understand the interaction model

### 2. Create Project Context File

In any project root, create `AGENTS.md`:

```markdown
# Project Context

## What This Does
[1-2 sentence description]

## Tech Stack
- Language: [Python/JS/etc]
- Framework: [if any]
- Key libraries: [list]

## Coding Standards
- [Your key rules, e.g., "TypeScript strict mode"]
- [Error handling approach]
- [Testing requirements]

## Current Focus
Working on: [current task]
Avoid touching: [sensitive areas]
```

### 3. Run First Test

```bash
cd ~/test-project
vibe "Create a simple REST API endpoint that returns 'Hello World' in Python Flask"
```

**Review the output carefully.** Does it run? Make sense? Match what you asked?

---

## Essential Commands

### Accept Edit Mode (START HERE)
```bash
vibe "your request here"
# Review each proposed change
# Accept/reject/modify
```

### YOLO Mode (DANGEROUS - use later)
```bash
vibe --yolo "your request here"
# AI makes changes directly
# Only use when confident
```

### Provide Context
```bash
vibe "@file src/main.py implement user authentication"
vibe "@folder src/auth/ add JWT refresh mechanism"
```

---

## Core Workflow (Memorize This)

### 1. Generate → Review → Refine

```bash
# Step 1: Generate
vibe "Add input validation to user registration"

# Step 2: Review (RUN THE CODE)
python test_registration.py

# Step 3: Refine
vibe "The email validation is too strict. Accept emails with + signs"

# Step 4: Review again
python test_registration.py
```

**Golden Rule:** NEVER trust "I've tested this" - always run it yourself.

### 2. Be Specific

❌ **Bad:** "Make this better"  
✅ **Good:** "Add error handling for network timeouts with exponential backoff, max 3 retries"

❌ **Bad:** "Fix the bug"  
✅ **Good:** "getUserProfile returns undefined for invalid IDs. Return 404 with descriptive message instead"

### 3. Include "Why" Context

```bash
vibe "Add Redis caching to product search. We're seeing 1000+ 
queries/second for the same searches causing database load. 
Use 5-minute TTL."
```

Context = better results.

---

## Code Review Checklist (Use for ALL AI Code)

Before accepting AI-generated code:

**Basics:**
- [ ] Does it compile/run?
- [ ] Does it do what you asked?
- [ ] Does it handle errors?

**Security:**
- [ ] Input validation?
- [ ] SQL injection prevented?
- [ ] No sensitive data in logs?

**Sanity:**
- [ ] No over-engineering?
- [ ] Matches project style?
- [ ] You understand it?

**If you check all boxes:** Accept. **If not:** Refine or reject.

---

## First Real Task (2 hours)

Pick a **non-critical** feature in a **personal** (not work) project:

### Good First Tasks:
- Add logging to existing endpoints
- Generate documentation for a module
- Write unit tests for a pure function
- Create a CLI wrapper for an API
- Add input validation to a form

### Bad First Tasks:
- Refactor core authentication
- Implement complex algorithm
- Anything with MARSS proprietary code
- Anything time-critical

**Process:**
```bash
1. Start vibe in your project directory
2. Reference your AGENTS.md: vibe "@file AGENTS.md"
3. Make your request with context
4. Review carefully
5. Test thoroughly
6. Iterate until correct
```

---

## What to Expect (Reality Check)

You WILL experience:

✅ **Fast boilerplate generation** - this is where AI shines  
✅ **Good scaffolding** - project structure, files, basic patterns  
✅ **Decent documentation** - better than you'd write manually

⚠️ **Repetition** - asking same thing multiple times  
⚠️ **Scope creep** - AI doing more than you asked  
⚠️ **"Tested" code that doesn't run** - always verify yourself  
⚠️ **Over-engineering** - add "keep this simple" to requests  
⚠️ **Getting stuck in loops** - AI trying/undoing same fix

**This is normal.** Learn to work with it.

---

## Troubleshooting (Top 3 Issues)

### Issue 1: "Code doesn't run"

```bash
# Share the error back
vibe "Got this error: [paste error]. Fix it."

# Or break it down
vibe "Just implement the input validation first, nothing else"
```

### Issue 2: "AI forgot my requirements"

**Solution:** Put requirements in `AGENTS.md`, reference it:
```bash
vibe "@file AGENTS.md Following our error handling standards, implement..."
```

### Issue 3: "Running out of credits too fast"

**Solution:**
- Keep context tight (only relevant files)
- Start new conversations instead of long threads
- Use Mistral (free) for exploration
- Save Claude/paid models for critical work

---

## Cost Reality

- **Mistral/Free models:** Good for learning, limited for production
- **$20 Claude:** Fun/experimentation, NOT full-time work
- **$100/month:** Maybe enough for part-time AI-assisted dev
- **Local models:** Free but slower, use for sensitive code

Budget accordingly.

---

## When to Read the Full Guide

Read the comprehensive guide when you:

1. **Hit productivity plateau** - not getting faster
2. **Want advanced patterns** - multi-model workflows, MCP, skills
3. **Need better understanding** - prompt engineering, context management
4. **Face complex projects** - need systematic approach
5. **Join team using AI** - need shared best practices

The full guide has:
- Detailed prompt engineering techniques
- Version control best practices for AI code
- Advanced multi-agent patterns
- Progressive learning path (weeks 1-4+)
- Concrete project templates
- Comprehensive troubleshooting

---

## Upgrade Path (After 2-3 Weeks)

When you're comfortable with Mistral CLI:

**Next Steps:**
1. **Try Claude** ($20 credit) - significantly better quality
2. **Add IDE integration** (Cursor, Continue) - faster workflow
3. **Set up local models** (Ollama) - for MARSS sensitive code
4. **Explore MCP/Skills** - customize AI behavior

**But only after** you understand the fundamentals from CLI use.

---

## Essential Resources

**Must Read:**
- [Claude Common Workflows](https://code.claude.com/docs/en/common-workflows)
- [agents.md Standard](https://agents.md/)
- Your team's `AGENTS.md` (create if missing)

**When You Have Time:**
- Full bootstrap guide (`ai-coding-bootstrap-guide.md`)
- [Claude Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- [spec-kit](https://github.com/github/spec-kit)

---

## Critical Rules (Never Break These)

1. **NEVER send MARSS proprietary code to cloud models**
2. **ALWAYS review AI-generated code before using**
3. **ALWAYS run/test AI code yourself** (don't trust "tested locally")
4. **START with accept-edit mode** (not YOLO mode)
5. **INCLUDE context** in your prompts (the "why")
6. **KEEP conversations focused** (start fresh when context gets polluted)

---

## Quick Reference Card

```bash
# Basic usage
vibe "your request"                    # Generate with review
vibe --yolo "your request"             # Auto-apply (careful!)

# Add context
vibe "@file path.py your request"      # Single file context
vibe "@folder src/ your request"       # Folder context

# Iteration
vibe "Fix: [paste error]"              # Address specific issue
vibe "Now add [feature]"               # Build incrementally

# Reset
# Start new conversation when:
# - AI is confused
# - Context is polluted
# - Approaching token limit
```

---

## Your First Week Checklist

**Day 1-2: Setup & Basics (4 hours)**
- [ ] Install Mistral Vibe CLI
- [ ] Create `AGENTS.md` for test project
- [ ] Generate "Hello World" endpoint
- [ ] Review and test it thoroughly
- [ ] Make AI fix any issues

**Day 3-4: Real Task (6 hours)**
- [ ] Pick non-critical personal project feature
- [ ] Use AI to implement with review workflow
- [ ] Test thoroughly, iterate on issues
- [ ] Commit reviewed code with proper message
- [ ] Reflect: faster or slower than manual?

**Day 5: Experimentation (3 hours)**
- [ ] Try documentation generation
- [ ] Try test case creation
- [ ] Try different prompting styles
- [ ] Note what works vs. what doesn't

**End of Week: Assess**
- **Faster at boilerplate?** ✅ Keep using
- **Catching mistakes quickly?** ✅ You're learning
- **Still reviewing carefully?** ✅ Good discipline
- **Confused more than helped?** 🤔 Read full guide

---

## Final Thought

AI coding tools are **power tools**. Used correctly: massive productivity gain. Used carelessly: expensive, dangerous mistakes.

Learn the constraints first. Master the basics. Then upgrade to the magic.

**Start now:**
```bash
npm install -g @mistralai/vibe
cd ~/your-test-project
vibe "Create AGENTS.md file explaining this project"
# Review the output, refine it, and you're off
```

Good luck! 🚀

---

## Need Help?

- **Stuck?** Read full guide: `ai-coding-bootstrap-guide.md`
- **Security question?** Ask security officer before uploading code
- **Found a better approach?** Share with team and improve this guide

---

**Version:** 1.0 (December 2024)  
**Based on:** Real-world MARSS development experience  
**Full guide:** `ai-coding-bootstrap-guide.md` (8000 words, 4-6 weeks learning path)