---
title: AI Delivery Pipeline
---

[RU](../../projects/ai-dev-pipeline.html) · [EN](./ai-dev-pipeline.html)

**2026 · Solo portfolio project · Working local prototype**

A workflow of three AI agents (planner, coder and reviewer) takes a small task from a Jira issue to a GitHub pull request, with mandatory code checks and human review.

## The challenge

With AI code generation, a developer usually pastes a task, gets code and checks it by hand. This does not replace a delivery process: there is no task intake, review, traceability or human accountability. Generated code can also be unsafe.

## My contribution

**IT Project Manager on a solo project: owned the goal, plan and results, and built the prototype.**

- Defined the goal, scope and Definition of Done; prepared the risk register and rollback plan.
- Designed the Jira-to-pull-request workflow with two modes: a dry run that changes nothing and a controlled real run.
- Built the agents on the Claude API and connected them to Jira and GitHub.
- Introduced mandatory checks: tests in a Docker container with no network access, a secret scan, human review and an API spending limit.

## What I built

<ol class="project-flow">
  <li><strong>Jira issue</strong><span>The pipeline picks up an issue with acceptance criteria and moves it to In Progress.</span></li>
  <li><strong>Plan and code</strong><span>The planner drafts a plan, and the coder writes the code and tests.</span></li>
  <li><strong>Checks</strong><span>A secret scan, tests in an isolated container and review. The reviewer can send the code back for rework.</span></li>
  <li><strong>Pull request</strong><span>Once approved, a pull request is created and the Jira status is updated. A person does the merge.</span></li>
</ol>

## The result

**A working prototype: the Jira-to-pull-request path works on real Jira and GitHub, and the checks stop faulty code.**

- For two Jira issues, the pipeline created pull requests and updated the issue status. In these runs, a built-in template produced the code, not the language model.
- In the run with Claude, the tests failed and the reviewer identified the cause, an import error. The error remained after a retry, so no pull request was created. The run cost about $0.01.
- Each run keeps a report: stage statuses, test results, stop reason and costs.
- Most recorded runs were made without the model, so the success rate is not measured yet.

<figure class="demo-figure">
<iframe class="demo-frame demo-pipeline" src="{{ '/demos/ai-dev-pipeline.html' | relative_url }}" title="Interactive demo: Multi-Agent AI Development Pipeline" loading="lazy"></iframe>
<figcaption style="font-size:.9em;color:#667085;margin-top:.4em;">Interactive animation of the Jira → PR workflow. <a href="{{ '/demos/ai-dev-pipeline.html' | relative_url }}" target="_blank" rel="noopener">Open full screen ↗</a></figcaption>
</figure>

## Next steps

The next stage is to run the model on real tasks and measure how many reach a pull request, the cost and the run time. Then come a metrics dashboard and role-based approvals (PM, tech lead, security).

<details markdown="1"><summary>Details: decisions and risks</summary>

- With controls enabled, a real run is allowed only in GitHub Actions after approval; by default, the pipeline makes a dry run.
- The spending limit stops a run when model costs exceed the budget.

[Sample status report](/evidence/pipeline-status-report.html) (in Russian)

</details>

**Technologies:** Python, Claude API, Jira Cloud, GitHub API, GitHub Actions, Docker and pytest.
