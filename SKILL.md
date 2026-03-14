---
name: engineering-principles
description: A set of software engineering principles for AI to follow proactively during planning, design, and implementation. Principles include: (1) prefer mature open-source libraries over custom implementations; (2) design long-running or failure-prone operations (crawling, batch processing, file imports) with progress tracking, fault tolerance, and resumability. Applicable to any tech stack or language.
---

# Engineering Principles

Universal software engineering principles and best practices for AI assistants, helping AI make better technical decisions, design better system architectures, and generate higher quality code.

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

---

> Real-world scenario comparisons for each principle: [examples/scenarios.md](examples/scenarios.md)
