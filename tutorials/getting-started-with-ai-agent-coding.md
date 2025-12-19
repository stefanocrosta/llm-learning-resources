# AI Agentic Coding: A Bootstrap Guide for Developers

This guide has been written by Claude based on my own personal approach - "learn the hard way"

## Philosophy: Learn the Hard Way First

This guide follows a contrarian approach: **learn with constraints before using magic tools**. Think of it like learning to drive a manual transmission before automatic - you'll understand what's actually happening under the hood.

Most guides rush you into Copilot/Cursor because it's sexy. You'll get impressive demos but won't understand what breaks when the magic fails. We're doing it differently.

## Phase 0: Before You Start (30 minutes - READ THIS)

### What You're Actually Getting Into

AI coding agents are **junior developers on speed**. They're:
- Fluent and confident in their responses
- Often wrong in subtle ways
- Excellent at boilerplate and patterns
- Terrible at novel problem-solving
- Incapable of understanding "why"

**Your role shifts from writing code to directing and reviewing it.** This is a different skill. Budget 2-3 weeks for the learning curve.

### Critical Security & Privacy Considerations

⚠️ **BEFORE uploading ANY code to cloud AI models:**

**NEVER send to cloud models:**
- Authentication credentials, API keys, secrets
- Proprietary algorithms or business logic from defense/aerospace projects
- Customer data or PII
- Code under NDA or export control
- Security-critical implementations (crypto, auth, access control)

**For sensitive work:**
- Use local models (Ollama, LM Studio) for proprietary code
- Use cloud models only for generic patterns/scaffolding
- Different sensitivity = different model choice
- When in doubt, ask your security officer

**At MARSS specifically:** Our NiDAR C2 code, tactical mission management logic, and countermeasure systems must NEVER go to cloud models. Use local models or work traditionally.

### Cost Reality Check

- **$20 Claude credit** = Fun experimentation, NOT production work
- **$100/month** = Maybe enough for part-time AI-assisted development
- **Free tiers (Mistral, Chinese models)** = Good for learning, limited for production
- **Tokens run out fast** when you're working on real problems

Plan your budget accordingly. This isn't free tooling.

### When NOT to Use AI Agents

**Avoid AI for:**
- Critical security code (authentication, authorization, encryption)
- Domains you don't understand (you can't review what you don't know)
- Complex refactoring of large codebases (high error rate, context limitations)
- "Just exploring" without clear goals (expensive token waste)
- Anything under time pressure (debugging AI mistakes takes time)

**AI excels at:**
- Boilerplate generation
- Test case creation
- Documentation writing
- Code structure scaffolding
- Explaining unfamiliar code
- Generating variations of working patterns

---

## Phase 1: Get Your Hands Dirty (Week 1: 5-10 hours)

### 1.1 Install Mistral Vibe CLI

Start here: https://mistral.ai/news/devstral-2-vibe-cli

**Why Mistral first, not Claude?**
- Currently FREE (with limitations)
- Decent coding capability
- Forces you to learn fundamentals (not as forgiving as Claude)
- CLI-only = understand the interaction model

**Setup:**
```bash
# Install Mistral Vibe
npm install -g @mistralai/vibe

# Test it
vibe --version
```

### 1.2 Understand the Interaction Model

**CLI vs. IDE Integration:**
- CLI = explicit, visible interaction (what we start with)
- IDE integration = "magic" autocomplete (what you upgrade to later)

**Run it alongside your editor:**
- Option A: Vibe CLI in terminal + VS Code on side monitor
- Option B: Vibe CLI in VS Code integrated terminal

I used Claude CLI in VS Code terminal for months before IDE integration existed. It teaches you more.

### 1.3 Learn the Modes

**Accept Edit Mode** (careful, reviewed)
- AI proposes changes
- You review each one
- Accept/reject/modify
- ALWAYS use this mode at first

**YOLO Mode** (autopilot, dangerous)
- AI makes changes directly
- You review after the fact
- Use only when you're confident and testing
- High risk of breaking things

**Start with accept edit. Always.**

### 1.4 First Experiments (2-3 hours each)

**Experiment 1: Start from Scratch**
```bash
mkdir ai-experiment-1
cd ai-experiment-1
vibe "Create a simple REST API for a todo list in Python using Flask"
```

**What to observe:**
- How does the AI structure the project?
- What files does it create?
- Does the code actually run?
- What did it assume vs. what you intended?

**Experiment 2: Modify Existing Code**

Take a personal project (NOT work code) and:
```bash
cd ~/my-personal-project
vibe "Add input validation to the user registration endpoint"
```

**What to observe:**
- How does it understand your existing code?
- What does it change vs. what you expected?
- Does it maintain your code style?
- Does it break anything?

**Experiment 3: Documentation Generation**
```bash
vibe "Generate comprehensive docstrings for all functions in src/"
```

**What to observe:**
- Quality of generated docs
- Understanding of code purpose
- Hallucinations (docs describing things that don't exist)

---

## Phase 2: Learn the Fundamentals (Week 1-2: 10-15 hours)

### 2.1 Prompt Engineering Basics

**The Rule: Specificity Over Vagueness**

❌ Bad: "Make this better"  
✅ Good: "Add error handling for network timeouts with exponential backoff, max 3 retries"

❌ Bad: "Fix the bug"  
✅ Good: "The getUserProfile function returns undefined when the user ID doesn't exist. It should return a 404 error with a descriptive message instead."

❌ Bad: "Optimize this"  
✅ Good: "This function processes 10K records in 5 seconds. Optimize it to handle 100K records in under 10 seconds using batch processing."

**Iterative Refinement Pattern:**
```
You: "Create a user authentication system"
AI: [generates basic auth]
You: "Add JWT token refresh mechanism"
AI: [adds refresh tokens]
You: "Include rate limiting on login endpoint"
AI: [adds rate limiting]
```

**When to Break Down vs. Give Full Context:**
- Break down: Complex features (>500 lines), novel architecture
- Full context: Modifying existing patterns, bug fixes, refactoring

**The "Why" Behind Instructions:**

Include context, not just commands:
```
"Add caching to the product search endpoint. We're seeing 1000+ 
queries/second for the same searches, causing database load. 
Use Redis with 5-minute TTL."
```

### 2.2 Context Window Management

**Understanding Tokens:**
- Everything sent to AI = tokens
- Tokens = money
- Large context = expensive and slow

**What Fits in Context:**
- Small project: entire codebase
- Medium project: relevant modules only
- Large project: specific files only

**Context Strategies:**
```
@file specific.py          # Single file
@folder src/auth/          # Entire folder
@codebase                  # Whole project (careful!)
```

**AGENTS.md / Claude.md Pattern:**
Create a project context file:
```markdown
# Project Context for AI

## What This Project Does
[Brief description]

## Architecture Overview
[Key components and their relationships]

## Coding Standards
- Use TypeScript strict mode
- All functions must have JSDoc comments
- Tests required for business logic
- Error handling: explicit, not try-catch everything

## Current Focus
Working on: User authentication refactor
Avoid touching: Payment processing module (under review)
```

More on this: https://agents.md/

### 2.3 Common Workflows (Study These)

**Essential Reading:**
- Claude's documentation (even for Mistral): https://code.claude.com/docs/en/common-workflows
- Best practices: https://www.anthropic.com/engineering/claude-code-best-practices

**Workflow 1: Generate → Review → Refine**
```
1. Ask AI to generate initial implementation
2. Review thoroughly (does it compile? make sense?)
3. Identify issues/improvements
4. Ask AI to refine specific parts
5. Repeat until acceptable
```

**Workflow 2: Spec → Plan → Implement → Test**
```
1. Write clear specification of what you need
2. Ask AI to create implementation plan
3. Review plan, adjust
4. AI implements according to plan
5. You write/review tests
6. AI fixes failing tests
```

**Workflow 3: Rubber Duck with AI**
```
"I'm trying to [problem description]. I've tried [approaches]. 
What am I missing?"
```

AI as a thinking partner, not just code generator.

### 2.4 Testing Strategy

**Golden Rule: NEVER trust "I tested it locally"**

The AI will confidently tell you code works. It doesn't know. It's guessing.

**Your Testing Workflow:**
```
1. AI generates code
2. YOU run it yourself
3. YOU write/review tests
4. AI can generate test cases (but you verify them)
5. AI can implement tests (but you run them)
```

**Test-First Pattern:**
```
You: "Here are the test cases for user registration:
      - Valid email and password succeeds
      - Invalid email returns 400
      - Weak password returns 400
      - Duplicate email returns 409
      
      Implement the registration endpoint to pass these tests."
```

**Using AI for Test Generation:**
```
"Generate unit tests for the calculateShipping function. 
Cover: zero quantity, negative prices, international addresses, 
edge cases for weight calculation."
```

Then review every test case yourself.

---

## Phase 3: Explore Alternatives (Week 2: 3-5 hours)

### 3.1 Try Other Free Models

**Chinese Models (iFlow and others):**
- Often better than Mistral at specific tasks
- Free tier available
- Good for comparison

**Why try multiple models?**
- Different strengths (Mistral better at X, iFlow better at Y)
- Understand model limitations vs. fundamental AI limitations
- Learn what works across models (robust patterns)

### 3.2 Multi-Model Workflows

**Pattern: Cross-Model Review**
```
1. Ask Mistral to implement feature X
2. Ask Claude to review Mistral's implementation
3. Ask Mistral to address Claude's feedback
4. Or vice versa
```

**Why this works:**
- Different models catch different issues
- Builds your judgment (which feedback is valid?)
- Cheaper than using premium model for everything

**Example:**
```bash
# In Mistral Vibe
vibe "Implement binary search tree with insert and search"

# Save the generated code, then in Claude
claude "Review this BST implementation for bugs and edge cases"
# [paste the code]

# Back to Mistral with feedback
vibe "Fix these issues in the BST implementation: [paste Claude's feedback]"
```

---

## Phase 4: Understand Best Practices (Week 2-3: 5-10 hours)

### 4.1 Version Control Discipline

**AI-Assisted Commits:**
```
DO:
✅ Review ALL generated code before committing
✅ Make atomic commits per AI conversation
✅ Write commit messages that capture the interaction
✅ Reject/modify AI suggestions that don't fit

DON'T:
❌ Blindly accept all changes
❌ Commit without testing
❌ Mix AI-generated and manual changes in same commit
❌ Trust AI-generated commit messages without review
```

**Example Commit Message:**
```
feat: Add Redis caching to product search endpoint

Generated by AI with the following prompt:
"Add Redis caching with 5-minute TTL to reduce database load 
from repeated searches"

Manual changes:
- Adjusted TTL from AI's suggestion of 1 minute to 5 minutes
- Added cache invalidation on product updates
- Fixed error handling for Redis connection failures
```

### 4.2 Code Review Checklist for AI Code

When reviewing AI-generated code, check:

**Correctness:**
- [ ] Does it compile/run?
- [ ] Does it handle edge cases?
- [ ] Are error paths covered?
- [ ] Does it match the specification?

**Security:**
- [ ] Input validation present?
- [ ] SQL injection prevented?
- [ ] XSS vulnerabilities closed?
- [ ] Sensitive data not logged?
- [ ] Authentication/authorization correct?

**Performance:**
- [ ] N+1 queries avoided?
- [ ] Unnecessary loops removed?
- [ ] Efficient data structures used?
- [ ] Memory leaks prevented?

**Maintainability:**
- [ ] Code style matches project?
- [ ] Clear variable names?
- [ ] Comments where needed (not obvious code)?
- [ ] No over-engineering?

**Testing:**
- [ ] Can this be tested?
- [ ] Are tests included?
- [ ] Do tests actually test something?
- [ ] Edge cases covered?

### 4.3 Measuring Your Progress

**After 2-3 weeks, assess:**

✅ **You're getting value if:**
- You're faster at scaffolding new features
- You catch AI mistakes within minutes
- You use AI for boilerplate, not complex logic
- You're still reviewing all code carefully
- You know when to use AI vs. when to code manually

⚠️ **Warning signs:**
- You're accepting code you don't understand
- You're debugging AI mistakes more than you're coding
- Your code quality has decreased
- You're frustrated more than productive

---

## Phase 5: Advanced Patterns (Week 3-4+: Ongoing)

### 5.1 Spec-Kit and Planning Modes

**spec-kit** (GitHub's approach): https://github.com/github/spec-kit
- Write specifications in structured format
- AI generates implementation plans
- More predictable outputs

**Claude's native plan mode:**
- Asks clarifying questions first
- Creates detailed plan
- Implements incrementally

**Compare both:**
```
1. Try same feature with spec-kit
2. Try same feature with Claude plan mode
3. Compare: which gives better results?
4. Understand: why did one work better?
```

### 5.2 Skills and Customization

**Claude Skills:** https://code.claude.com/docs/en/skills

Skills = reusable AI behaviors for your context

**Create project-specific skills:**
```markdown
# MARSS Code Review Skill

When reviewing code:
1. Check against defense system standards
2. Verify error handling for military-critical paths
3. Ensure logging meets audit requirements
4. Validate performance for real-time constraints
5. Check thread safety for concurrent operations
```

### 5.3 MCP (Model Context Protocol)

**MCP Guide:** https://code.claude.com/docs/en/mcp

MCP = Standardized way for AI to interact with external tools

**Use cases:**
- Database queries during code generation
- API testing during implementation
- File system operations
- Custom tooling integration

**Start simple:**
```
1. Pick one tool to integrate (e.g., test runner)
2. Create MCP server for it
3. Let AI use it during development
4. Expand to other tools
```

### 5.4 Multi-Agent Systems (Advanced)

**When you're comfortable with basics**, explore:

**Agent-to-Agent Communication (A2A):**
- https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/
- https://google.github.io/adk-docs/

**Multi-Agent Frameworks:**
- **LangGraph**: Workflow orchestration
- **AutoGen**: Conversational agents
- **CrewAI**: Role-based agent teams
- **Warp**: Terminal-integrated agents

**Warning:** These are powerful but complex. Only explore after mastering single-agent workflows.

---

## What to Expect (The Unvarnished Truth)

### You WILL Experience:

**🔄 Repetition Fatigue**
- Asking the same thing 3+ times
- AI "forgetting" previous instructions
- Having to re-explain context repeatedly

**🎯 Optimizing Context Files**
- Tweaking AGENTS.md constantly
- Learning what context helps vs. confuses
- Finding the right level of detail

**🚀 Scope Creep**
- AI doing more than you asked
- Over-engineering simple requests
- Adding "helpful" features you didn't want

**🤖 Uncanny Valley Moments**
- Getting complimented for basic instructions
- AI apologizing profusely for your mistakes
- Weirdly formal language in code comments

**♾️ Infinite Loops**
- AI making a change, undoing it, remaking it
- Stuck trying to fix the same issue repeatedly
- You having to interrupt and redirect

**🤦 Complete Misunderstandings**
- AI solving the wrong problem confidently
- Hallucinating APIs that don't exist
- "I've tested this thoroughly" [code doesn't compile]

**❤️ Anthropomorphization**
- Feeling grateful to the AI
- Getting frustrated "at" it
- Developing preferred models

**Remember:** These aren't bugs, they're features of statistical language models. Learn to work with them, not against them.

---

## Troubleshooting Common Issues

### Issue: "AI keeps forgetting my requirements"

**Solution:**
```
1. Add requirements to AGENTS.md
2. Reference specific requirements in each prompt:
   "Following the error handling pattern in AGENTS.md, implement..."
3. Keep conversations focused (don't let them drift)
4. Start new conversation if context gets polluted
```

### Issue: "Generated code doesn't run"

**Solution:**
```
1. Don't trust "I've tested this"
2. Always run code yourself
3. Share error messages back to AI
4. Ask AI to explain what it did (catches hallucinations)
5. Break down complex requests into smaller pieces
```

### Issue: "AI is over-engineering everything"

**Solution:**
```
Add to your prompts:
"Keep this simple. No classes unless needed. No design patterns 
unless they solve a real problem. No premature optimization."

Add to AGENTS.md:
## Code Philosophy
- Prefer simple over clever
- Add complexity only when needed
- YAGNI (You Aren't Gonna Need It)
```

### Issue: "Running out of tokens/credits too fast"

**Solution:**
```
1. Use free models (Mistral) for exploration
2. Use premium models (Claude) for critical work
3. Keep context tight (only relevant files)
4. Start new conversations instead of long threads
5. Review code carefully to avoid rework cycles
```

### Issue: "Not sure if this is better than just coding myself"

**Solution:**
```
Track your time for 1 week:
- Time saved on boilerplate/scaffolding
- Time spent reviewing AI code
- Time spent fixing AI mistakes
- Time spent prompt engineering

If (time saved) < (time spent), adjust:
- Better prompts (less rework)
- Use AI for different tasks
- Or don't use AI for this project
```

---

## Concrete First Projects

### Project 1: Personal CLI Tool (4-6 hours)
```
Build a command-line tool for a personal need:
- File organizer
- Log parser
- Backup script
- Data converter

Constraints:
- Single language (Python/JS/Go)
- Clear input/output
- Error handling required
- You understand the domain

Why: Simple enough to review, useful enough to care
```

### Project 2: REST API Wrapper (4-6 hours)
```
Wrap an existing API with better error handling:
- Choose an API you use
- Add retry logic
- Add better error messages
- Add caching

Constraints:
- Use existing API documentation
- Add comprehensive tests
- Handle rate limits

Why: Real integration testing, clear success criteria
```

### Project 3: Documentation Generator (4-6 hours)
```
Generate documentation for an existing project:
- API documentation
- Architecture diagrams (mermaid)
- Setup instructions
- Contributing guide

Constraints:
- Must match actual code
- No hallucinated features
- Keep it updated

Why: Forces AI to read and understand existing code
```

### Project 4: Test Suite Addition (6-8 hours)
```
Add tests to an under-tested project:
- Identify coverage gaps
- Generate test cases
- Implement tests
- Achieve 80%+ coverage

Constraints:
- Tests must pass
- Tests must be meaningful
- Edge cases required

Why: Tests reveal if AI understands the code
```

---

## Progressive Upgrade Path

### After You're Comfortable (4-6 weeks)

**Consider upgrading to:**

**1. Claude with Credits ($20-100/month)**
- Significantly better code quality
- Fewer hallucinations
- Better at complex refactoring
- Worth it when you're productive

**2. IDE Integration (Cursor, Continue, etc.)**
- Now that you understand what's happening
- Faster workflow (but same review discipline)
- Autocomplete feels natural, not magic

**3. Local Models (for sensitive code)**
- Ollama + Llama/Mixtral
- Full privacy
- Slower but secure
- Use for MARSS proprietary code

**4. Specialized Models**
- Code-specific models (DeepSeek Coder)
- Domain-specific fine-tuned models
- Task-specific models (testing, docs, etc.)

---

## Resources

### Essential Documentation
- [Claude Code Common Workflows](https://code.claude.com/docs/en/common-workflows)
- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- [agents.md Standard](https://agents.md/)
- [GitHub spec-kit](https://github.com/github/spec-kit)

### Community & Learning
- Join AI coding communities (Discord, Reddit)
- Follow practitioners (Simon Willison, Swyx, et al.)
- Read post-mortems of AI coding failures
- Share your learnings with team

### Keep Updated
- AI coding tools evolve FAST
- New models every few months
- Best practices change
- Revisit this guide quarterly

---

## Final Thoughts

### The Meta-Point

This guide exists in two versions:
1. **Raw version** (what I wrote first)
2. **AI-polished version** (what you might generate with Claude)

Compare them. Notice:
- What AI adds (structure, clarity)
- What AI loses (personality, specific context)
- What you prefer for different audiences

**The goal isn't to replace your thinking - it's to augment it.**

Use AI as a junior developer on your team:
- Give clear instructions
- Review everything
- Teach it your standards
- But ultimately, you're responsible

### When This Works

AI coding assistance works when:
- You understand what you're building
- You can review generated code
- You use it for appropriate tasks
- You maintain discipline

It fails when:
- You don't understand the domain
- You skip review steps
- You use it for everything
- You trust it too much

### Keep Learning

After this guide:
- Experiment continuously
- Share findings with team
- Develop your own best practices
- Contribute back to community

---

## Version History

**v1.0** - Initial release (December 2024)  
Based on real-world experience with:
- Claude Code (1+ years)
- Multiple model comparisons
- MARSS development context
- Team onboarding learnings

**Feedback welcome:** [Your contact/repo here]

---

## License

This guide is shared freely for educational purposes. Adapt it for your team, improve it, share it.

If you found it helpful, consider contributing improvements back.