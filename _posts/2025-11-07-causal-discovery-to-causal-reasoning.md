---
layout: post
title: "From causal discovery to causal reasoning"
date: 2025-11-07
description: "Discovering a graph is the easy half. Reasoning over it is the rest of the problem."
tags: [Causality, LLM, Reasoning]
---

Causal discovery is having a moment. Constraint-based methods like PC
and FCI, score-based methods like NOTEARS, time-series methods like
PCMCI and Granger-style approaches, are all in active use. Given
enough data and a tolerable set of assumptions, you can recover a
plausible directed graph over your variables. The literature treats
that graph as the output.

In a control setting, the graph is the input. The output is a
decision an operator will live with.

That gap, between having a causal graph and using it to reason, is
where most of the engineering effort actually goes, and it is
underexposed in the literature.

Three problems show up the moment you try to deploy a discovered
graph in a working system.

The first problem is that the graph is wrong. Not catastrophically
wrong, just wrong in the way real models are wrong. An edge points
the wrong direction. Two variables that should be connected are not.
A latent confounder is misattributed to a spurious direct link. If
you take the graph at face value and feed it to a downstream
reasoning system, the system will produce confidently wrong answers.
The honest fix is to never let the graph stand alone. It needs a
domain expert in the loop, and the system has to make it cheap for
that expert to inspect, edit, and version the graph.

The second problem is that the graph is static and the world is not.
Greenhouse dynamics in summer are not the same as in winter. The
right causal structure for tomato in flowering is not the same as in
fruiting. A single discovered graph collapses time-varying causal
structure into one frozen picture. You can address this with regime
detection, with windowed discovery, with hierarchical graphs that
distinguish slow-varying structure from fast-varying parameters. None
of these fixes are free. They all add complexity and they all need
their own validation story.

The third problem, the deepest one, is that even a correct,
up-to-date graph does not tell you what to do. It tells you how
variables relate. The leap from "ventilation flap causes humidity"
to "should I open the ventilation flap right now" is a planning
problem, not a discovery problem. The graph is a constraint on the
planner, not a substitute for it.

Our 2025 *Frontiers in Agronomy* paper sits in this gap. We use
constraint-based causal discovery on greenhouse sensor streams to
recover a graph over climate, plant, and control variables. Then we
expose that graph to an LLM as a structured reference. The LLM does
not do causal discovery. It reads the discovered graph and uses it as
a scaffold for plain-language recommendations that a grower can
follow.

The split matters. The causal-discovery algorithm is good at finding
relations from data. It is not good at deciding what to do with them.
The LLM is good at composing readable, contextual recommendations.
It is not good at separating correlation from causation. Putting them
in series, with the graph as the bridge, lets each component do what
it is actually good at.

There is an interesting open question hiding in this setup. How
should the LLM handle disagreement with the graph? In some queries
the model's pretraining tells it one thing and the discovered graph
tells it another. The conservative answer is "always defer to the
graph." The more useful answer is probably "flag the disagreement,
explain both views, let the operator decide." The right policy is
not obvious and we are still learning it.

The bigger picture. Causal discovery alone is not the goal. It is
one tool in a longer pipeline that ends with a human making a
decision. The papers that move the field forward in the next few
years will, I think, be the ones that take that pipeline seriously
end to end.
