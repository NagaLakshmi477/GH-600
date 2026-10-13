# GH-600 — Explain the Agent Lifecycle: Plan, Act, Evaluate

## 1. What Is the Agent Lifecycle?

An AI agent does not necessarily make one decision and stop. It can work through repeated cycles until a task meets its defined success criteria.

The fundamental agent lifecycle is:

**Plan → Act → Evaluate → Repeat if needed**

Each phase has a specific purpose:

* **Plan:** Decide what needs to be done.
* **Act:** Execute the planned actions.
* **Evaluate:** Check whether the result meets the requirements.
* **Repeat:** Use feedback to improve the result if necessary.

### Real-World Example: Fixing a Security Vulnerability

Suppose your company uses a Python Flask application, and a security alert identifies a vulnerable dependency.

You ask an AI agent:

"Update the vulnerable dependency, test the application, and prepare the changes for review."

The agent follows the lifecycle:

1. **Plan:** Identify the vulnerable package, determine a suitable version, and define the required tests.
2. **Act:** Create a branch, update the dependency files, and open a pull request.
3. **Evaluate:** Check whether tests pass and whether the vulnerability has been resolved.
4. **Repeat:** If tests fail or the vulnerability remains, revise the changes and evaluate again.

The task is not complete merely because the agent says it has finished. The result must satisfy the defined requirements.

---

## 2. Phase One: Plan

### What is planning?

Planning is the phase in which the agent interprets the goal and determines the steps required to achieve it.

A good plan explains:

* What needs to change.
* Which files or components are in scope.
* What success looks like.
* How the result will be verified.
* What to do if the task cannot be completed safely.

### What are planning artifacts?

A planning artifact is a visible, reviewable record of the intended work.

In GitHub, examples include:

* A structured plan in a pull request description.
* A linked GitHub issue.
* A checklist of implementation tasks.
* Explicit acceptance criteria.

These artifacts help developers understand and assess the proposed approach.

### Real-World Example

Suppose your Flask application has a vulnerable dependency.

Before changing the code, the agent prepares this plan:

**Goal:** Update the vulnerable dependency safely.

**Scope:**

* Update the relevant dependency declaration.
* Update the lockfile if applicable.
* Avoid unrelated code changes.

**Success criteria:**

* The dependency uses an approved patched version.
* Relevant tests pass.
* The security finding is resolved or its remaining status is explained.

**Fallback:** If the update breaks compatibility, investigate the failure and request human assistance if necessary.

This plan allows the developer to review the approach before accepting the resulting changes.

### Exam Tip

A high-quality plan should make the agent's intended approach understandable and reviewable.

Remember these three important planning elements:

1. **Scope:** What will change?
2. **Success criteria:** How will we know it worked?
3. **Rollback or escalation path:** What happens if something goes wrong?

---

## 3. Phase Two: Act

### What is the action phase?

During the action phase, the agent executes the planned steps using its available tools and permissions.

In GitHub, actions may include:

* Creating a branch.
* Editing repository files.
* Creating commits and pushing changes.
* Opening a pull request.
* Updating a pull request after review feedback.

The goal is to perform work within a controlled development workflow.

### Real-World Example

After preparing the dependency-update plan, the agent:

1. Creates a branch named `agent/update-vulnerable-dependency`.
2. Updates the dependency declaration.
3. Updates the lockfile.
4. Commits the changes.
5. Opens a pull request describing what changed.

The proposed changes are now available for inspection and testing.

### Why use a separate branch?

A separate branch isolates the agent's proposed changes from the default branch.

This makes it easier to:

* Review the changes.
* Run automated checks.
* Identify problems.
* Request modifications.
* Reject the proposal if necessary.

**Important:** A branch does not automatically make an agent safe. Repository permissions, rulesets, and other protections still matter.

### Exam Tip

**Act = Execute the plan through authorized tools and controlled workflows.**

An agent should not need unrestricted access to the default branch just to propose a code fix.

---

## 4. Phase Three: Evaluate

### What is evaluation?

Evaluation is the process of determining whether the agent's actions achieved the intended result.

The agent and its human supervisors use evidence from the development environment rather than relying solely on the agent's confidence.

Evaluation may involve:

* Automated test results.
* Build and lint results.
* Code review comments.
* Security scan findings.
* Dependency vulnerability alerts.
* Verification against the original requirements.

### Common GitHub Evaluation Signals

| Signal            | What it tells you                                                          | Example                                       |
| ----------------- | -------------------------------------------------------------------------- | --------------------------------------------- |
| Workflow runs     | What happened during automated execution                                   | A GitHub Actions workflow completed or failed |
| Status checks     | Whether required checks passed                                             | Unit tests pass                               |
| Code review       | Whether reviewers identified problems or approved the changes              | A reviewer requests changes                   |
| Code scanning     | Whether configured analysis identifies code security issues                | A security finding remains                    |
| Secret scanning   | Whether supported secret-detection mechanisms identify exposed credentials | A committed token triggers an alert           |
| Dependency alerts | Whether known dependency vulnerabilities are reported                      | The package remains flagged as vulnerable     |

### Real-World Example

The agent updates a dependency and opens a pull request.

Evaluation produces these results:

* Unit tests: Passed.
* Lint checks: Passed.
* Security scan: Passed.
* Dependency vulnerability alert: Still present.

Should the task be considered complete?

**No.** The original goal included resolving the vulnerability, and the security signal indicates that the goal may not have been achieved.

The agent must investigate whether the wrong version was selected, another vulnerable dependency remains, or the security finding needs further verification.

### Security-Specific Evaluation

For security-related changes, evaluation may include:

**Code scanning:** Analyzes code for supported security issues. Some workflows upload results in SARIF format.

**Secret scanning:** Detects supported types of exposed secrets, such as credentials, in monitored locations.

**Push protection:** Can block supported secrets from being pushed to a repository when the relevant protection is enabled.

These controls help prevent unsafe changes from moving forward.

### Evaluation as an Enforceable Gate

Evaluation can be more than an informal check.

When repository rules require successful status checks or approvals, GitHub can prevent a pull request from being merged until the required conditions are met.

For example:

* Required tests must pass.
* Required reviewers must approve.
* Applicable branch or ruleset policies must be satisfied.

**Exam Tip:** A check only becomes an enforceable merge gate when the relevant repository policies are configured to require it.

---

## 5. The Lifecycle Is a Loop, Not a One-Time Sequence

The agent lifecycle continues whenever the result fails to meet the required criteria.

### Flowchart

```text
          START
            |
            v
       DEFINE GOAL
            |
            v
           PLAN
            |
            v
            ACT
            |
            v
         EVALUATE
            |
            v
     Are requirements met?
          /       \
        No         Yes
        |           |
        v           v
  Analyze feedback  Verify completion
        |           |
        v           v
  Revise the plan   Finish or hand off
        |
        v
       ACT AGAIN
```

The exact implementation can vary. Some failures require replanning, while others require a small adjustment followed by another evaluation.

If the agent cannot safely resolve the problem, it should escalate the task to a human rather than continue indefinitely.

### Real-World DevOps Example

An agent updates a dependency in a Python application.

**First attempt:**

* Plan: Upgrade the package.
* Act: Update the dependency and lockfile.
* Evaluate: Integration tests fail.

**Second attempt:**

* Plan: Investigate the compatibility issue.
* Act: Adjust the package version or related configuration.
* Evaluate: Run the tests again.

**Final outcome:**

* If all required checks pass and the vulnerability is resolved, the change may be ready for review and merging.
* If the issue remains unresolved, the agent continues investigating or escalates to a human.

The number of attempts is not the success criterion. Meeting the requirements is.

---

## 6. What Makes a High-Quality Agent System?

The official material highlights three important properties.

### 6.1 The Plan Is Inspectable

People can understand what the agent intends to do.

**Example:** The pull request describes the dependency being changed, the reason for the update, and the tests required.

### 6.2 Actions Are Bounded

The agent operates within defined permissions and controlled workflows.

**Example:** It creates a branch and proposes a pull request rather than bypassing required repository protections.

### 6.3 Evaluation Uses Objective Signals

The result is checked using evidence.

**Example:** The team examines test results, security findings, and review feedback rather than trusting the agent's statement that everything is fixed.

### Why Do These Properties Matter?

| Missing property     | Potential problem                                    |
| -------------------- | ---------------------------------------------------- |
| Inspectable plan     | The approach is difficult to understand or review    |
| Bounded actions      | Changes may exceed the intended scope or permissions |
| Objective evaluation | Incorrect or unsafe results may be accepted          |

**Key principle:** Visibility, controlled execution, and evidence-based evaluation help make agent workflows more trustworthy.

---

## 7. Complete Example: An Agent Fixes a Dependency Vulnerability

Consider this complete workflow:

| Phase        | Agent activity                                              | Evidence or output                 |
| ------------ | ----------------------------------------------------------- | ---------------------------------- |
| Plan         | Identify the vulnerable package and define success criteria | Issue or PR plan                   |
| Act          | Update the dependency on a separate branch                  | Changed files and commits          |
| Evaluate     | Run tests and inspect security signals                      | Workflow results and scan findings |
| Iterate      | Revise the update if a check fails                          | New commits and updated results    |
| Human review | Review the final proposal                                   | Review comments and approvals      |
| Completion   | Meet required checks and merge policies                     | Change is ready to merge or merged |

This demonstrates how planning, execution, evaluation, and human oversight work together.

---

## 8. GH-600 Exam Revision Notes

Memorize these concepts:

* **Plan:** Interpret the goal and determine the steps.
* **Planning artifact:** A visible, reviewable record of the intended work.
* **Act:** Execute authorized actions in the repository.
* **Evaluate:** Use objective evidence to assess the result.
* **Feedback loop:** Revise the plan or actions when requirements are not met.
* **Workflow runs and status checks:** Provide evidence about automated execution.
* **Code review:** Provides human feedback and approval.
* **Security signals:** Help identify unresolved code, secret, and dependency risks.
* **Enforceable gate:** A required condition that repository policies prevent you from bypassing.
* **Human escalation:** Appropriate when the agent cannot safely resolve a problem.
* **High-quality agent system:** Inspectable plan + bounded actions + objective evaluation.

### One-Line Memory Trick

**Plan = Decide. Act = Execute. Evaluate = Verify. Repeat = Improve or resolve.**

---

## 9. GH-600 Practice Questions

### Question 1

An agent plans a dependency update, changes the files, and runs the test suite. The tests fail.

What should happen next?

A. Merge the pull request immediately.
B. Declare success because the files were updated.
C. Use the failure as feedback, revise the approach, and evaluate again.
D. Disable the failing tests.

**Correct answer: C**

Explanation: The lifecycle is iterative. Failed checks provide feedback that can guide further actions.

### Question 2

Which is the best example of a planning artifact?

A. A successful build log.
B. A pull request description listing the intended changes and success criteria.
C. A commit created after implementation.
D. A production deployment.

**Correct answer: B**

Explanation: A planning artifact makes the intended approach and expected result visible and reviewable.

### Question 3

A developer wants a pull request to remain blocked until required tests pass. What is needed?

A. A planning document alone.
B. An agent's promise to run tests.
C. A branch name containing the word `test`.
D. Required status checks enforced through the repository's merge policies.

**Correct answer: D**

Explanation: Automated checks provide evidence, and repository rules can make their success a condition for merging.

### Question 4

An agent reports that a vulnerability has been fixed, but the relevant security alert remains unresolved.

What is the best response?

A. Trust the agent's confidence.
B. Investigate the remaining security signal and continue evaluating or escalate.
C. Delete the alert without investigation.
D. Skip human review.

**Correct answer: B**

Explanation: Evaluation must be grounded in system evidence and the original success criteria.

### Question 5

Which three characteristics best describe a high-quality agent system in this unit?

A. Hidden plans, unrestricted actions, and self-reported success.
B. Fast execution, no testing, and automatic merging.
C. Inspectable plans, bounded actions, and objective evaluation.
D. More commits, longer pull requests, and fewer reviewers.

**Correct answer: C**

Explanation: Transparency, controlled execution, and evidence-based validation make agent behavior easier to trust.

---

## Final Revision

The agent lifecycle explains how an AI system moves from an intended goal to an evaluated result.

**Plan → Act → Evaluate → Repeat when needed.**

In GitHub, plans can be made visible in issues and pull requests, actions can occur through branches and commits, and evaluation can use workflows, status checks, reviews, and security signals.

The central lesson is that **an agent's task is not complete simply because it performed an action. The result must meet the defined success criteria and pass the required validation.**

### Next unit

**GitHub as the System of Record and Control Plane for Agent Workflows.**
