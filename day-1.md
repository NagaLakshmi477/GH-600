# GH-600 Certification Notes: Understanding AI Agents

## 1. Traditional AI vs. AI Agents

### 1.1 Traditional AI Assistant

A traditional AI assistant primarily provides answers, suggestions, and recommendations. The user performs the actual work.

**Example:**

You are developing a Flask API and encounter a Python error.

1. You ask GitHub Copilot to explain the error.
2. Copilot suggests a solution.
3. You manually update your Python code.
4. You run the application and verify the fix.

**Key idea:** The AI assists you, but you perform the work yourself.

### 1.2 Agentic AI

An AI agent can work toward a goal by investigating a problem, planning steps, using tools, making changes, and evaluating the results.

Its capabilities depend on the tools, permissions, and environment available to it.

**Example: Fixing a Flask API bug**

Imagine your employee management API crashes when an employee ID does not exist.

An AI agent can:

1. Explore the relevant source code.
2. Identify the possible cause of the error.
3. Plan a solution.
4. Modify the code on a separate branch.
5. Run tests to verify the changes.
6. Open a pull request for human review.

**Key idea:** An agent can take action toward a goal instead of only suggesting a solution.

> **GH-600 exam tip:** An AI agent can use tools to perform actions toward a goal. However, its output must still be validated.

### 1.3 Comparison: Assistant vs. Agent

| Feature              | Traditional AI Assistant                 | AI Agent                                              |
| -------------------- | ---------------------------------------- | ----------------------------------------------------- |
| Primary purpose      | Answer questions and provide suggestions | Work toward a defined goal                            |
| Takes action         | Primarily assists the user               | Can perform actions using tools                       |
| Executes tasks       | Usually relies on the user               | Can execute tasks within its permissions              |
| Uses feedback        | May respond to user feedback             | Can use evaluation results to adjust its next actions |
| Human responsibility | Perform and verify the work              | Guide, supervise, and validate the work               |

---

## 2. The Core Agent Loop: Plan → Act → Evaluate

The core agent loop describes how an agent works toward a goal, checks its progress, and adjusts its actions based on feedback.

### 2.1 Plan

The agent determines what needs to be done and identifies the steps required to achieve the goal.

**Example:**

An agent investigates a failing SQLAlchemy query and plans to inspect the query, check the model, and reproduce the error.

**Remember:** Plan = Decide what to do.

### 2.2 Act

The agent performs actions using available tools.

These actions may include:

* Reading source code.
* Editing files.
* Running commands.
* Executing tests.
* Interacting with external systems when authorized.

**Example:**

The agent modifies the SQLAlchemy query to correct the identified problem.

**Remember:** Act = Perform the planned work.

### 2.3 Evaluate

The agent checks whether the result meets the required goal.

It may examine:

* Unit test results.
* Error messages.
* Command output.
* Expected versus actual behavior.
* Other available feedback.

**Example:**

The agent runs the unit tests after modifying the SQLAlchemy query.

If a test fails, the agent investigates the failure and determines what to try next.

**Remember:** Evaluate = Check whether the action worked.

### 2.4 How the Loop Works in Practice

```text
        PLAN
          |
          v
         ACT
          |
          v
       EVALUATE
          |
          v
   Did the result meet
      the goal?
       /     \
     Yes      No
      |        |
      v        v
    Finish   Use feedback
             and revise
                 |
                 v
              PLAN AGAIN
```

**Real-world example: Fixing a SQLAlchemy bug**

1. **Plan:** Identify why the database query returns an incorrect result.
2. **Act:** Modify the query.
3. **Evaluate:** Run the relevant unit tests.
4. **Repeat:** If a test fails, investigate the failure and revise the solution.
5. **Finish:** Stop when the required conditions are satisfied and the result has been adequately verified.

> **GH-600 exam tip:** Evaluation feedback can guide the agent's next action. It does not necessarily mean that the agent permanently learns or updates its underlying model.

---

## 3. GitHub as the Control Room for AI Agents

AI agents may be able to modify code, but their actions must be governed to reduce the risk of introducing bugs or security vulnerabilities.

GitHub provides mechanisms for tracking changes, enforcing repository policies, reviewing contributions, and controlling access to protected resources.

### 3.1 Pull Requests (PRs)

A pull request proposes changes for review before they are merged into a target branch.

**Example:**

An AI agent fixes a payment-processing bug and opens a pull request instead of directly merging the changes into the `main` branch.

The development team can inspect the proposed changes before merging them.

**Purpose:** Enable review of proposed code changes.

### 3.2 Code Review

Code review involves examining proposed changes for correctness, security, maintainability, and compliance with project standards.

**Example:**

A developer reviews the agent's payment-processing changes to ensure that the fix does not introduce a security vulnerability.

**Purpose:** Help identify problems before changes are accepted.

### 3.3 Status Checks

Status checks report whether required automated checks have passed.

Examples include:

* Unit tests.
* Integration tests.
* Code quality checks.
* Security scans.

**Example:**

A pull request cannot be merged until the required unit tests pass, provided the repository's merge rules enforce that requirement.

**Purpose:** Automatically verify required conditions before merging.

### 3.4 CODEOWNERS

The `CODEOWNERS` file identifies individuals or teams responsible for reviewing specific files or directories.

**Example:**

The database team owns database migration files. Changes to those files require the team's approval when the repository's rules enforce code-owner review.

**Purpose:** Assign review responsibility to the appropriate people or teams.

### 3.5 Rulesets

GitHub rulesets define rules governing branches, tags, and other supported repository resources.

Rules may restrict certain operations or require conditions to be satisfied before changes are accepted.

**Example:**

A ruleset prevents unauthorized direct changes to the `main` branch and requires pull requests and successful status checks.

**Purpose:** Enforce repository policies.

### 3.6 Environments

GitHub Actions environments can protect deployment targets and control access to environment-specific secrets.

Depending on the configuration, environments can require approvals or restrict which branches can deploy.

**Example:**

An AI-assisted workflow prepares a production deployment. The deployment must receive approval before it can proceed to the protected production environment.

**Purpose:** Control deployments and access to protected resources.

### 3.7 Quick Comparison of GitHub Controls

| Feature       | Main Purpose                      | Example                                 |
| ------------- | --------------------------------- | --------------------------------------- |
| Pull requests | Propose changes for review        | Agent opens a PR                        |
| Code review   | Examine proposed changes          | Developer reviews the fix               |
| Status checks | Validate automated conditions     | Required tests must pass                |
| CODEOWNERS    | Identify responsible reviewers    | Database team reviews migration files   |
| Rulesets      | Enforce repository policies       | Restrict direct pushes to `main`        |
| Environments  | Protect deployments and resources | Production deployment requires approval |

> **Important:** These controls work together. A pull request alone does not guarantee safety, and a status check alone does not guarantee that code is correct.

---

## 4. System of Record vs. Control Plane

These are two important concepts for understanding how GitHub governs software development workflows.

### 4.1 System of Record

A system of record maintains authoritative information and a history of relevant activities.

**GitHub example:**

* Git commits record committed changes.
* Pull requests record proposed changes and associated discussions.
* Reviews record reviewer feedback and approvals.
* Workflow runs provide records of automation results.

**Remember:** System of record = Track and review what happened.

### 4.2 Control Plane

A control plane is the mechanism through which policies, permissions, and rules govern what actions are allowed.

**GitHub example:**

* Branch rules restrict unauthorized changes.
* Rulesets enforce repository policies.
* Environment protection rules govern deployments.
* Permissions control who or what can access resources.

**Remember:** Control plane = Govern what is allowed.

### 4.3 Comparison

| Concept          | Simple Meaning                 | GitHub Example                                        |
| ---------------- | ------------------------------ | ----------------------------------------------------- |
| System of record | Records changes and activities | Commits, pull requests, reviews, and workflow records |
| Control plane    | Governs permitted actions      | Rulesets, permissions, and protection rules           |

**Easy way to remember:**

* **System of record:** What happened?
* **Control plane:** What is allowed to happen?

GitHub does not automatically record every action an agent performs. The available history and audit information depend on the activity, tools, integrations, and configuration involved.

---

## 5. Your Role as an AI Supervisor

In an agentic workflow, developers do more than write code. They also guide and supervise the work performed by AI agents.

### Key Responsibilities

1. **Define the goal:** Explain the task and the expected outcome clearly.
2. **Provide context:** Give the agent relevant requirements, files, and constraints.
3. **Set boundaries:** Limit access and actions according to the principle of least privilege.
4. **Review changes:** Inspect proposed code before accepting it.
5. **Validate results:** Check tests, functionality, and security implications.
6. **Approve responsibly:** Follow the team's review and deployment policies.

### Real-World Example

Suppose an agent modifies your employee management API.

Before accepting the changes, you should verify that:

* Existing employee records are not accidentally overwritten.
* Database updates behave correctly.
* Invalid employee IDs are handled properly.
* Relevant tests pass.
* The code does not expose sensitive information.
* The changes satisfy your team's review requirements.

The agent's completion message is not proof that the task was completed correctly.

---

## 6. The Contributor Model

The contributor model means treating an AI agent's work like a contribution from another developer.

The agent may prepare code, tests, documentation, or other changes, but those contributions should be reviewed and validated according to the project's engineering standards.

### Example

A junior developer submits a pull request to fix a database bug.

You review the changes, check the tests, and approve the pull request only when the required conditions are met.

You should apply the same disciplined review process to an AI agent's contribution.

**Key principle:** Delegate the work, but retain responsibility for validating and accepting the result.

---

## 7. GH-600 Exam Revision Notes

Review these points before attempting practice questions.

* **AI assistant:** Primarily provides answers, suggestions, and guidance.
* **AI agent:** Can use tools to take actions toward a defined goal.
* **Agent loop:** Plan → Act → Evaluate.
* **Feedback loop:** Evaluation results can guide subsequent actions.
* **Human oversight:** Agent output must be reviewed and validated.
* **Pull request:** Proposes changes for review before merging.
* **Status checks:** Verify required automated conditions.
* **CODEOWNERS:** Identifies responsible reviewers for specific files.
* **Rulesets:** Enforce repository policies.
* **Environments:** Can protect deployment targets and secrets.
* **System of record:** Maintains records of changes and activities.
* **Control plane:** Governs permitted actions through policies and permissions.
* **Contributor model:** Review agent contributions using the same engineering standards applied to other contributions.

---

## 8. Practice Questions

### Question 1

An AI agent modifies a SQLAlchemy query, runs a test, observes a failure, and modifies the query again.

Which concept does this demonstrate?

A. CODEOWNERS
B. Plan → Act → Evaluate feedback loop
C. System of record
D. Production environment

**Correct answer: B**

**Explanation:** The agent uses evaluation feedback to guide its next action.

### Question 2

A company wants to ensure that only designated database engineers review changes to database migration files.

Which GitHub feature identifies the responsible reviewers?

A. Status checks
B. Git commits
C. CODEOWNERS
D. Agent memory

**Correct answer: C**

**Explanation:** CODEOWNERS maps file patterns to designated owners. Repository rules can require their approval.

### Question 3

A team wants to prevent direct pushes to its protected `main` branch.

Which mechanism can enforce this restriction?

A. Rulesets or branch protection rules
B. An AI assistant's response
C. A successful test alone
D. A README file

**Correct answer: A**

**Explanation:** Repository rules can restrict direct pushes and enforce other conditions.

### Question 4

An AI agent reports that it has fixed a security-sensitive API bug. What should the developer do?

A. Merge the change immediately
B. Trust the agent because it completed the task
C. Review the code, validate the results, and follow required approval policies
D. Disable automated tests

**Correct answer: C**

**Explanation:** Human oversight and validation are essential because agent output is not guaranteed to be correct.

### Question 5

Which statement best describes the difference between a system of record and a control plane?

A. Both terms mean automated testing
B. A system of record tracks relevant information, while a control plane governs permitted actions
C. A system of record executes code, while a control plane writes documentation
D. Both terms mean code review

**Correct answer: B**

**Explanation:** Records help establish what happened; policies and permissions govern what is allowed.

---

## Final Memory Trick

Remember these four lines:

1. **Agent:** Can act toward a goal.
2. **Loop:** Plan → Act → Evaluate.
3. **GitHub:** Records changes and enforces controls.
4. **Developer:** Supervises, reviews, and validates agent contributions.

These notes are based on the supplied study material and are intended for revision, not as a substitute for the official GH-600 exam objectives.
