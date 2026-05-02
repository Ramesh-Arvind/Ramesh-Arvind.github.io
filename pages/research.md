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

## Why this matters

The gap between a model that performs well on a benchmark and a model that an
expert is willing to deploy in a working greenhouse, factory, or hospital is
almost entirely about explanation. I think the next decade of applied ML will
be defined less by raw capability and more by interpretability under
distribution shift.

For paper-level summaries, see [Publications](/publications/).
