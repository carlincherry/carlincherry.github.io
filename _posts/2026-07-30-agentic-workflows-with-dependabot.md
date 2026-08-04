---
layout: post
title: "Why I'm excited about GitHub agentic workflows"
date: 2026-07-30
feed_order: 20260730
---

One of the AI projects that has kept my attention this year is the GitHub Next team's [Agentic Workflows](https://github.github.com/gh-aw/) (currently in public preview). I've had the privilege to work with members of the team and watch them evolve. This article will discuss how I've used Agentic Workflows, how I've seen them evolve, and what's exciting to me about what's next!

## What they are

Agentic Workflows in the GitHub sense let you describe arbitrary repository automation in plain-language Markdown. The `gh aw` CLI compiles that file into a standard GitHub Actions workflow, then runs a coding agent such as GitHub Copilot, Claude, Codex, Gemini, etc. In my view, they are a good fit for jobs that require judgment, including issue triage, CI failure analysis, documentation maintenance, and recently, some security work.

## My first, highly experimental, agentic workflows

I started testing Agentic Workflows with Dependabot in February 2026. I built workflows that grouped arbitrary security findings (first and third party to GitHub), created tracking issues, added them to a GitHub Project, and handed the deterministic security findings to an agent for remediation.

My experience in those very early experimental days was full of difficult-to-decipher token errors, project integration friction, and chaining of workflows together in kind of weird and unintuitive ways. I also had no evaluation framework for the agent's suggested fixes. HOWEVER: even with those rough edges, the core experience felt incredibly powerful, and it's really stuck with me. It has also gotten a **lot** better!

## Agentic Workflows have grown up quickly

[Agentic Workflows entered public preview in June](https://github.blog/changelog/2026-06-11-github-agentic-workflows-is-now-in-public-preview/), and I am super jazzed with where they're heading.

The safety model is explicit (for example, agents run with read-only permissions by default). Proposed writes pass through [safe outputs](https://github.github.com/gh-aw/reference/safe-outputs/), where separate jobs enforce limits before creating an issue, updating a project, or opening a pull request. Sandboxing, network controls, and threat detection add more boundaries around each run.

Since my February experiment, though, the change that excites me most is the move from repository-level automation to a centrally managed control plane. Agentic workflows can now be packaged through [`aw.yml`](https://github.github.com/gh-aw/reference/aw-yml-package-manifest/), installed with `gh aw add-wizard`, and authenticated in a centrally managed way (no longer has to be a PAT!!)

A private control repository for your entire Organization or even Enterprise can define shared guardrails once, then dispatch bounded workers to eligible repositories. The model works across three layers: an enterprise can distribute shared packages such as Dependabot automation across organizations, each organization can add its own packages and policies, and repositories can continue running local workflows alongside centrally managed automation.

Rollouts can start with report-only or staged behavior so you can get a sense for what's going to happen when you "go live". Scoped permissions, safe outputs, dispatch limits, fail-closed behavior, and correlated run IDs keep the work bounded and traceable. The [CentralRepoOps pattern](https://github.github.com/gh-aw/patterns/central-repo-ops/) and [Dependabot rollout example](https://github.github.com/gh-aw/examples/multi-repo/dependabot-rollout/) show what this looks like across many repositories.

There are also some nifty ways to prove that the automation is useful. Evals measure whether a run met its goals, while logs, audit data, AI credit budgets, and OpenTelemetry make cost and performance visible.

## Why I am excited

Security tools like Dependabot already deterministically find vulnerable dependencies and proposes updates. Agentic Workflows can add judgment (informed by you, in natural language) around which work items belong together, which ones need attention first, how the work should be tracked, and when a coding agent should attempt a fix.

Agentic Workflows are still in public preview and I plan to keep building and testing them. I am excited to see how the project grows and what people build with it next!!
