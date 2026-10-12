# GH-600 — Define Agentic AI in the SDLC

## 1. What Is Agentic AI in the SDLC?

### What does SDLC mean?

**SDLC stands for Software Development Life Cycle.**

It is the process followed by developers to build, test, release, and maintain software.

Typical SDLC activities include:

1. Planning requirements
2. Writing code
3. Testing the application
4. Reviewing code
5. Deploying the application
6. Maintaining and fixing bugs

### What is Agentic AI?

Agentic AI is AI that can work toward a goal by deciding intermediate steps, using tools, taking actions, and evaluating results.

In software development, an AI agent can participate in development activities rather than simply giving suggestions.

**Simple example:**

Suppose you tell an AI agent:

"Fix the security vulnerability in my Flask application's dependency."

The agent may:

* Inspect the repository and identify the vulnerable dependency.
* Determine which version should be used.
* Update the dependency and lockfile.
* Run relevant tests.
* Create a branch and open a pull request.
* Respond to test failures or review feedback.

The exact actions depend on the agent's capabilities, tools, and permissions.

**Key idea:** Agentic AI can participate in the software development process by taking actions toward a defined goal.

---

## 2. Traditional AI Assistant vs. AI Agent

The main difference is how much responsibility the AI takes for moving the task forward.

### 2.1 Assistant-Based AI

An assistant generally responds to your instructions and gives you information or suggestions.

**Real-world example: Updating a Python dependency**

You ask GitHub Copilot:

"How can I safely update the Flask dependency in my application?"

The assistant provides:

* Commands to update the dependency.
* Recommended version constraints.
* Possible compatibility risks.
* Suggested testing steps.

You must perform the actions yourself, such as creating a branch, modifying files, running tests, and opening a pull request.

**Remember:** Assistant = Suggests and explains; the user performs the actions.

### 2.2 Agent-Based AI

An agent can interpret a goal, determine intermediate steps, use tools, and execute actions within its permitted environment.

**Real-world example: Updating a Python dependency**

You ask an agent:

"Update the vulnerable dependency in my Flask application and prepare the changes for review."

The agent may:

1. Inspect the dependency files.
2. Identify a suitable patched version.
3. Create a separate branch.
4. Update the dependency files.
5. Run tests and inspect the results.
6. Open a pull request.
7. Revise the changes if checks or review feedback reveal problems.

You supervise the work and review the proposed changes before they are merged.

**Remember:** Agent = Works toward the goal; the human supervises and validates.

### 2.3 Comparison Table

| Feature             | Assistant-Based AI                   | Agent-Based AI                                     |
| ------------------- | ------------------------------------ | -------------------------------------------------- |
| Behavior            | Primarily reactive                   | Goal-driven                                        |
| Main responsibility | Suggests solutions                   | Takes steps toward a goal                          |
| Planning            | May suggest a plan                   | Can determine intermediate steps                   |
| Tool usage          | May have limited or no action tools  | Can use tools to perform authorized actions        |
| Repository changes  | User applies suggestions             | Can create or modify repository artifacts          |
| Feedback            | User may ask follow-up questions     | Can iterate using test results and review feedback |
| Example             | Suggests a dependency update command | Updates dependency files and opens a PR            |

> **GH-600 exam tip:** Do not classify a system only by whether it uses an AI model or generates code. Look at its behavior: goal-directed steps, tool usage, actions, and iteration based on feedback.

---

## 3. What Makes an AI System Agentic?

There are five important characteristics to understand.

### 3.1 Goal-Oriented Behavior

The agent receives an objective and works toward achieving it.

**Example:**

"Fix the failing employee update API tests."

The goal is to make the relevant tests pass while preserving the expected application behavior.

### 3.2 Multi-Step Planning

The agent determines the intermediate steps required to complete the task.

**Example:**

1. Locate the failing test.
2. Inspect the relevant API endpoint.
3. Investigate the database query.
4. Propose a fix.
5. Run the tests again.

### 3.3 Tool Usage

The agent uses available tools to interact with its environment.

Examples include:

* GitHub API
* Repository file operations
* Command-line tools
* CI workflows
* Test runners

**Example:**

The agent runs a test command to verify whether a code change resolves a bug.

### 3.4 Durable Artifacts

The agent can create or modify outputs that persist beyond a chat response.

Examples include:

* Git branches
* Commits
* Modified source files
* Pull requests

**Example:**

Instead of merely showing you a suggested code fix, the agent commits the change to a working branch and opens a pull request.

### 3.5 Iteration Based on Feedback

The agent uses feedback to decide what to do next.

Feedback may include:

* Failed tests
* CI errors
* Security scan results
* Reviewer comments
* Approval or rejection

**Example:**

A CI workflow reports that a dependency update breaks a unit test. The agent investigates the failure, revises the changes, and reruns the relevant checks.

**Important:** Iteration does not guarantee success. Human review and required validation remain necessary.

---

## 4. How Agentic AI Works in GitHub

GitHub agents participate in the development workflow through familiar repository features.

| GitHub feature       | What it tells us                             |
| -------------------- | -------------------------------------------- |
| Branches             | Where changes are being developed            |
| Commits              | What changes have been recorded              |
| Pull requests        | What changes are proposed and why            |
| Workflows and checks | What automated validation has occurred       |
| Review comments      | What feedback reviewers have provided        |
| Approvals            | Whether required human review has been given |

### Real-World Example: Fixing a Security Vulnerability

Suppose a security alert identifies a vulnerable dependency in your application.

The agent can follow this workflow:

1. Investigate the security alert.
2. Identify the affected dependency.
3. Determine a suitable update.
4. Create a working branch.
5. Update the dependency and lockfile.
6. Run tests and relevant security checks.
7. Open a pull request with a summary of the changes.
8. Address failures or reviewer feedback.
9. Wait for the required approvals and checks before merging.

This process allows the agent to contribute without bypassing the team's existing controls.

### Flowchart

```text
       Security Alert
              |
              v
     Agent Investigates
              |
              v
      Plans the Update
              |
              v
      Creates a Branch
              |
              v
     Updates Dependencies
              |
              v
       Runs CI Checks
              |
              v
      Opens Pull Request
              |
              v
      Human Code Review
              |
              v
       Are Changes Ready?
          /        \
        No          Yes
        |            |
        v            v
   Revise Changes   Merge
        |
        v
   Run Checks Again
```

The exact workflow depends on repository permissions, automation configuration, and the team's merge policies.

---

## 5. Assistant vs. Agent: Exam Scenario

### Scenario A: Suggestion-Only Assistant

A developer asks an AI assistant how to fix a failing test.

The assistant explains the problem and suggests a code change. The developer manually edits the file, creates a branch, and opens a pull request.

**Answer:** Assistant-based behavior.

**Why?** The AI provides guidance, but the developer performs the repository actions.

### Scenario B: PR-Producing Agent

A developer asks an AI agent to fix a failing test.

The agent inspects the repository, modifies the relevant file, runs tests, creates a branch, and opens a pull request.

**Answer:** Agent-based behavior.

**Why?** The AI works toward a goal through multiple steps and uses tools to create persistent repository artifacts.

### Scenario C: Agent Responds to Feedback

An agent opens a pull request, but a required CI check fails. It inspects the error, updates the code, and reruns the check.

**Answer:** Agent-based behavior with feedback-driven iteration.

**Why?** The agent uses the evaluation result to guide its next action.

---

## 6. Important GH-600 Exam Points

Remember these concepts:

* **SDLC:** The Software Development Life Cycle.
* **Assistant:** Primarily responds with information, explanations, or suggestions.
* **Agent:** Can work toward a goal through intermediate steps and authorized tool usage.
* **Goal-driven:** The agent works toward an intended outcome.
* **Tool usage:** Allows the agent to interact with repositories, commands, APIs, and other systems.
* **Durable artifacts:** Persistent outputs such as branches, commits, and pull requests.
* **Feedback-driven iteration:** The agent can revise its work based on test results and review feedback.
* **Pull-request-oriented workflow:** The agent proposes changes for review instead of automatically bypassing the team's workflow.
* **Human oversight:** Developers remain responsible for reviewing and validating proposed changes.

### Quick Memory Trick

**Assistant = Suggest**

**Agent = Plan + Use Tools + Act + Iterate**

**GitHub = Branches + Commits + Pull Requests + Checks + Reviews**

**Developer = Guide + Supervise + Validate**

---

## 7. Practice Questions

### Question 1

A developer asks an AI system to explain how to fix a Python error. The system provides a code example, but the developer must edit the repository and run the tests manually.

Which behavior is demonstrated?

A. Agent-based behavior
B. Assistant-based behavior
C. Automated deployment
D. Repository governance

**Correct answer: B — Assistant-based behavior**

Explanation: The AI provides a suggestion, while the developer performs the actual work.

### Question 2

An AI system receives the goal of fixing a bug, inspects the repository, modifies code, runs tests, and opens a pull request.

Which characteristic most clearly distinguishes this from suggestion-only assistance?

A. It generates text
B. It uses an AI model
C. It takes authorized actions across multiple steps toward a goal
D. It answers a question

**Correct answer: C**

Explanation: Goal-directed action across multiple steps, enabled by tools, is a key characteristic of agentic behavior.

### Question 3

A dependency update causes a CI test to fail. The agent investigates the failure, revises the change, and reruns the tests.

What does this demonstrate?

A. Feedback-driven iteration
B. Permanent model training
C. CODEOWNERS configuration
D. Manual-only development

**Correct answer: A**

Explanation: The agent uses evaluation feedback to guide further actions. This does not necessarily mean the underlying AI model has permanently learned anything.

### Question 4

Why might an agent create a separate branch and open a pull request instead of changing the protected main branch directly?

A. A pull request guarantees that the code is bug-free
B. The agent should avoid all testing
C. A pull request supports review and validation within the team's workflow
D. A branch eliminates the need for human approval

**Correct answer: C**

Explanation: The pull request enables review and validation. Required protections and approvals depend on repository configuration.

### Question 5

Which combination provides the strongest evidence that an AI system is behaving agentically?

A. It answers questions and generates explanations
B. It summarizes a document
C. It interprets a goal, uses tools, takes actions, and iterates based on feedback
D. It produces a code snippet in a chat

**Correct answer: C**

Explanation: These behaviors show that the system is actively working toward a goal rather than merely providing a response.

---

## Final Revision

If you remember only one idea from this unit, remember this:

**Agentic AI changes AI from a tool that primarily advises developers into a system that can participate in development by planning, acting, using tools, and responding to feedback. GitHub provides the workflow in which those contributions can be tracked, checked, and reviewed.**

Next topic: **Explain the agent lifecycle — Plan, Act, Evaluate.**
