---
layout: page
title: Research
subtitle: Making controllers and models explain themselves.
permalink: /research/
---
My research sits between control theory and machine learning. Modern
optimization-based controllers, like model predictive control, are powerful but
opaque. They make hundreds of trade-offs per step, and the people who actually
operate the systems they govern, growers, plant operators, building managers,
are usually left guessing why a particular decision was taken. I work on
methods that turn those decisions into explanations a domain expert can
inspect, challenge, and trust.

## Current threads

**Language interfaces for control.** A natural-language layer that sits over a
running MPC controller and answers operator questions in plain English,
grounded in the optimizer's structure and a domain knowledge graph rather than
free hallucination. This is the substrate of our 2025 *Smart Agricultural
Technology* paper, applied first to greenhouses.

**Causal reasoning over reference trajectories.** Reference signals in
controlled-environment agriculture are time-varying and seasonal. We built an
automated pipeline that decomposes them into components an operator can audit,
using causal-discovery tools as the workhorse. This is the focus of our 2025
*Frontiers in Agronomy* paper.

**Mechanistic interpretability in non-NLP settings.** I am exploring how the
toolkit developed for understanding language models, probes, circuit analysis,
activation patching, transfers to models that operate over physical systems.
More on this soon.

## Position in the field

A note on what this work is and is not. It is not a foundation-model
paper, I do not train large models from scratch. It is not a pure
benchmark paper, I am not chasing leaderboard numbers on standard NLP
suites. It sits in a narrower place: the use of language models as
auditable reasoning layers on top of optimisation-based controllers,
evaluated against control-engineering criteria like faithfulness,
stability, and operator utility rather than perplexity. The methods
borrow from interpretability research and from causal inference, but
the load-bearing layer underneath is still classical optimal control.
That is the lineage I want the work to be read in.

## Group context

I work in the Chair of Automatic Control and System Dynamics at TU
Chemnitz, led by Prof. Dr.-Ing. Stefan Streif. The group develops
control-theoretic methods for nonlinear systems, with strong lines of
work in optimal and learning-based control, set-based analysis of
uncertain dynamical systems, and hierarchical, fault-tolerant
control. Application areas include controlled-environment agriculture
and circular bioeconomy, hydrogen and fuel-cell systems, carbon
capture and utilisation, multi-physics systems, AI-driven decision
support for energy and automation, and the automation of railway
operations within the Smart Rail Connectivity Campus. Across these
threads the group keeps the full cycle in view, from system modelling
through robustness analysis to deployment on real hardware. My own
thread sits inside the controlled-environment agriculture and
AI-for-decision-support lines: I bring large language models,
mechanistic interpretability, and causal discovery into the
controller stack, and the optimisation and robustness machinery the
group is known for stays as the load-bearing layer underneath.

## Why this matters

The gap between a model that performs well on a benchmark and a model that an
expert is willing to deploy in a working greenhouse, factory, or hospital is
almost entirely about explanation. I think the next decade of applied ML will
be defined less by raw capability and more by interpretability under
distribution shift.

For paper-level summaries, see [Publications](/publications/).
