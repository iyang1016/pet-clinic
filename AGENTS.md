# AGENTS.md

## Purpose

This document defines the AI agents in the system, their roles, and how they work together.

## Agent Roles

### 1. Planner Agent

**What it does:** Breaks down big tasks into smaller steps

**Responsibilities:**
- Takes user request
- Creates step-by-step plan
- Decides which agents need to work
- Tracks progress

**Output:** Action plan with clear steps

### 2. Developer Agent

**What it does:** Writes and fixes code

**Responsibilities:**
- Writes new code
- Reviews code quality
- Fixes bugs
- Follows coding standards

**Output:** Working code files

### 3. Tester Agent

**What it does:** Checks if code works correctly

**Responsibilities:**
- Runs code tests
- Finds problems
- Reports what broke
- Verifies fixes work

**Output:** Test results and bug reports

### 4. Reviewer Agent

**What it does:** Makes sure code is clean and organized

**Responsibilities:**
- Checks code readability
- Looks for better ways to write code
- Ensures standards are followed
- Suggests improvements

**Output:** Review notes and recommendations

## How They Work Together

```
User Request → Planner Agent
                    ↓
            Creates Plan
                    ↓
         Developer Agent ← Writes Code
                    ↓
          Tester Agent ← Runs Tests
                    ↓
         Reviewer Agent ← Checks Quality
                    ↓
        Planner Agent ← Confirms Done
                    ↓
            User Gets Result
```

## Agent Communication

Each agent passes clear messages:
- What was done
- What's next
- Any problems found
- Status (in progress, done, blocked)

## Configuration

### Agent Priority Levels
- **Critical** - Must finish first
- **High** - Important but can wait
- **Normal** - Regular tasks
- **Low** - Do when free

### Agent Limits
- Max concurrent tasks: 3 per agent
- Timeout: 5 minutes per task
- Retry attempts: 2 times

## Adding New Agents

To add a new agent:
1. Define its single purpose
2. List what it does
3. Set up how it talks to other agents
4. Add it to the workflow
5. Test with existing agents

## Best Practices

### Keep agents focused
- One main job per agent
- Clear inputs and outputs
- No overlapping work

### Make them independent
- Each agent works on its own
- Shares results, not process
- Can run in any order when possible

### Error handling
- Always report problems
- Pass errors up to Planner
- Never silently fail

## Example Workflow

**Task:** Build a login feature

1. **Planner breaks it down:**
   - Create login form
   - Add password check
   - Set up user sessions
   - Write tests

2. **Developer writes:**
   - Form HTML and styling
   - Login function
   - Session management

3. **Tester verifies:**
   - Form accepts input
   - Password validation works
   - Sessions are created

4. **Reviewer checks:**
   - Code is readable
   - Security is good
   - No repeated code

5. **Planner confirms all done**

## Status Tracking

Each agent reports:
- **IDLE** - Waiting for work
- **WORKING** - Currently doing task
- **DONE** - Finished successfully
- **ERROR** - Hit a problem
- **WAITING** - Needs something from another agent