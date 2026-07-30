---
layout: post
title: "My experience using Dependabot with GitHub Agentic Workflows"
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

To me, the new [governance layer](https://github.github.com/gh-aw/guides/governance/) is especially interesting. Enterprises and organizations can centrally set model defaults, AI credit budgets, timeouts, and runtime policies. A policy can disable pull request creation across an organization, while a repository-level exception can allow it for a workflow that has earned more trust. This is a cold take, but centralized policy controls and governance are among the most critical components in software, and I've been jazzed to see how the Next team has evolved here.

Teams can maintain reusable workflows and shared components in a central repository, then roll them out across many repositories with consistent controls. The documentation includes a [Dependabot rollout example for 100 repositories](https://github.github.com/gh-aw/examples/multi-repo/dependabot-rollout/), using a central orchestrator to select repositories and dispatch bounded workers. :heart-eyes:

There are new and exciting ways to try to get at the value provided by the agents, too. Evals record whether a run met specific goals. Experimental A/B tests compare prompt variants and outcomes. Logs, audit data, AI credit budgets, and OpenTelemetry help teams understand cost and performance. The repository's own workflows show these ideas in practice: one [reviews dependency updates and groups safe patches](https://github.com/github/gh-aw/blob/main/.github/workflows/dependabot-go-checker.md), while another [bundles related Dependabot pull requests into a bounded remediation wave](https://github.com/github/gh-aw/blob/main/.github/workflows/dependabot-burner.md).

## Why I am excited

Dependabot already deterministically finds vulnerable dependencies and proposes updates. Agentic Workflows can add judgment around which updates belong together, which ones need attention first, how the work should be tracked, and when a coding agent should attempt a fix.

Agentic Workflows are still in public preview. I plan to keep testing them with real backlogs, clear limits, and humans reviewing the results. I am excited to see how the project grows and what people build with it next.
