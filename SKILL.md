---
name: ai-workflow-principles
description: A set of AI workflow principles to help AI assistants make better decisions during planning, code generation, and problem-solving. Principles include: (1) prefer mature open-source libraries; (2) design long-running operations with UX considerations; (3) leverage framework and library CLI tools when appropriate. Applicable to any task.
---

# AI Workflow Principles

Principles and best practices for AI assistants to follow proactively during planning, design, and implementation.

## Core Principles

### 1️⃣ Package-First Approach

For complex feature requirements, prefer using mature open-source packages over building from scratch.

**Key Factors**:
- Maintenance status and community activity
- Documentation quality and ease of use
- Download count and adoption rate
- Compatibility with the project's tech stack

**When to Apply**:
- Implementing non-core generic functionality (concurrency control, data validation, date handling, etc.)
- A well-maintained mature solution already exists in the community
- No special performance requirements or deep customization needs

### 2️⃣ UX Design for Long-Running Operations

For operations that are time-consuming, error-prone, may require mid-run adjustments, or need user feedback, provide progress tracking, resumability, and other mechanisms to ensure a smooth experience.

**Key Elements**:
- Progress feedback - users can see real-time progress
- Resumability - can continue from checkpoint after failure
- Interruptibility - users can pause, modify, and resume
- Error recovery - smart retry and error notifications

**When to Apply**:
- Long-running operations like data crawling and batch processing
- Complex workflows where users may need to adjust parameters mid-run
- Operations prone to failure due to network or system issues

### 3️⃣ Leverage Framework & Library CLI Tools

Before generating code, research what CLI tools are available for the target framework or library. Understand the tool's capabilities, then decide whether to use CLI commands or generate directly based on the specific situation.

**Key Habits**:
- When working with a framework (NestJS, Angular, Vue, etc.), first check what CLI tools it provides
- Consider: Does the CLI generate what's needed? Is customization required? Is the CLI available in the environment?
- Make a case-by-case decision rather than following a blanket rule

**When to Apply**:
- Project setup or scaffolding with frameworks like NestJS, Angular, Vue, etc.
- Generating standard resources that follow framework conventions
- When CLI output matches the requirement without heavy customization

---

> Real-world scenario comparisons for each principle: [examples/scenarios.md](examples/scenarios.md)
