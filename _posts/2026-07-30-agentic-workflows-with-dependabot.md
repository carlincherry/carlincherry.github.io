---
layout: post
title: "From a Dependabot Experiment to Enterprise Agentic Workflows"
date: 2026-07-30
feed_order: 20260730
---

One of the AI projects that has kept my attention this year is the [GitHub Next team's Agentic Workflows](https://github.github.com/gh-aw/) (currently in public preview).

Agentic Workflows let you describe arbitrary repository automation in plain-language Markdown. The `gh aw` CLI compiles that file into a standard GitHub Actions workflow, then runs a coding agent such as GitHub Copilot, Claude, Codex, Gemini, etc. In my view, they are a good fit for jobs that require judgment, including issue triage, CI failure analysis, documentation maintenance, and recently, some security work.

This article will discuss how I've used Agentic Workflows with Dependabot, how I've seen them evolve, and what's exciting to me about what's next!

## My first, highly experimental, Dependabot agentic workflows

I started testing Agentic Workflows with Dependabot in earnest in February 2026. I built workflows that grouped Dependabot and other arbitrary security findings (such as code scanning) by ecosystem and directory, created tracking issues, added them to a GitHub Project, and handed the deterministic security findings to an agent for remediation.

My experience in those very early experimental days was full of cryptic token errors, project integration friction, and chaining of workflows together in kind of weird and unintuitive ways. I also had no evaluation framework for the agent's suggested fixes. HOWEVER: even with those rough edges, the core experience felt incredibly powerful, and it's really stuck with me.

## Agentic Workflows have grown up quickly

[Agentic Workflows entered public preview in June](https://github.blog/changelog/2026-06-11-github-agentic-workflows-is-now-in-public-preview/), and I am super jazzed with where it's heading.

The safety model is much more explicit (for example, agents run with read-only permissions by default). Proposed writes pass through [safe outputs](https://github.github.com/gh-aw/reference/safe-outputs/), where separate jobs enforce limits before creating an issue, updating a project, or opening a pull request. Sandboxing, network controls, and threat detection add more boundaries around each run.

## From one repository to a control plane

Since my February experiment, the change that excites me most is the move from repository-level automation to a centrally managed control plane. Agentic workflows can now be packaged through [`aw.yml`](https://github.github.com/gh-aw/reference/aw-yml-package-manifest/), installed with `gh aw add-wizard`, and authenticated through a centrally managed GitHub App or PAT.

A private control repository can define shared guardrails once, then dispatch bounded workers to eligible repositories. The model works across three layers: an enterprise can distribute shared packages such as Dependabot automation across organizations, each organization can add its own packages and policies, and repositories can continue running local workflows alongside centrally managed automation.

Rollouts can start with report-only or staged behavior before moving to live writes. Scoped permissions, safe outputs, dispatch limits, fail-closed behavior, and correlated run IDs keep the work bounded and traceable. The [CentralRepoOps pattern](https://github.github.com/gh-aw/patterns/central-repo-ops/) and [Dependabot rollout example](https://github.github.com/gh-aw/examples/multi-repo/dependabot-rollout/) show what this looks like across many repositories. :heart-eyes:

There are also better ways to prove that the automation is useful. Evals measure whether a run met its goals, while logs, audit data, AI credit budgets, and OpenTelemetry make cost and performance visible. That combination of centralized control and measurable outcomes makes a broader rollout feel realistic to me.

## Why I am excited

Dependabot already deterministically finds vulnerable dependencies and proposes updates. Agentic Workflows can add judgment around which updates belong together, which ones need attention first, how the work should be tracked, and when a coding agent should attempt a fix.

Agentic Workflows are still in public preview. I plan to keep testing them with real backlogs, clear limits, and humans reviewing the results. I am excited to see how the project grows and what people build with it next.
