---
layout: post
title: "How we made greenhouse controllers explain themselves"
subtitle: "A walk through the two papers our group published in 2025."
date: 2025-08-15
reading_time: "12 min read"
tags: [Explainable AI, Model Predictive Control, LLM, Greenhouse]
---

If you have ever stood in front of an industrial controller and asked
"why did you just do that?", you already know the problem this post is about.

Modern greenhouses run on model predictive control. The controller looks a few
hours into the future, predicts what the plants and the building will do under
different ventilation, lighting, irrigation, and CO2 choices, and picks the
sequence of actions that minimizes a cost function. It is genuinely good at
its job. The trouble is that the cost function does not speak English, and the
grower does.

This is the gap our two 2025 papers were trying to close, from two different
sides.

## Paper one: read the setpoint before you criticize the controller

Before you can explain a controller's actions, you have to understand what it
was being asked to do. In greenhouses, that "ask" is a reference trajectory
for temperature, humidity, and CO2 that shifts across the day and the season.
These trajectories are not flat. They have ramps, dwell periods, plateaus,
diurnal cycles, slow seasonal drifts, and the occasional anomaly when someone
opened a vent at the wrong time.

Our *Frontiers in Agronomy* paper, *Automated analysis of reference signals*,
is essentially a piece of plumbing nobody had built carefully: an automated
pipeline that takes a real reference trajectory and decomposes it into the
components a human grower would describe if you asked them to. Diurnal pattern
here, weekly trend there, this hour is the ramp into night setback, that hour
is a recovery from a humidity excursion. The output is a structured,
operator-readable description of the setpoint, not a black-box model of it.

The reason this matters is that any explanation of controller behaviour is
only as good as the description of what the controller was being asked to
track. Without this layer, you end up explaining noise. With it, you can have
a real conversation about whether the setpoint design itself was reasonable.

[Read the paper.](https://doi.org/10.3389/fagro.2025.1547628)

## Paper two: let the grower talk to the controller

Our *Smart Agricultural Technology* paper, *Enhancing greenhouse management
with interpretable AI*, is the second half of the story. Once you can describe
what the controller is being asked to do, you can build something that lets a
grower ask, in plain English, why the controller is doing what it is doing.

The naive way to do this is to hand the question to a large language model
and hope. That does not work for two reasons. First, the model has no idea
what is actually happening inside the optimizer. Second, even when it sounds
confident, its answers are not grounded in the controller's own reasoning,
which means they cannot be audited.

What we built is a thin language layer that does three things in sequence.
It maps the operator's natural-language question onto the structured
artefacts the optimizer already produces, things like the active constraints,
the multipliers, and the per-step contributions to the cost. It uses a domain
knowledge graph, encoding what plants, vents, lights, and humidity actually
do to each other, to constrain which explanations are even physically
plausible. And it returns the answer in the operator's own language, with
the underlying evidence inline so a sceptical grower can dig into the numbers.

The headline result is that the system produces explanations that match
expert annotations far better than off-the-shelf attribution methods. But the
honest reason we are happy with it is more boring: when we sat with growers
and watched them use it, they trusted it. They argued with it. They
occasionally won the argument. That is what an interpretable system should
feel like.

[Read the paper.](https://doi.org/10.1016/j.atech.2025.101041)

## What ties them together

These are two papers, but they are really one thesis. If you want a controller
to explain itself in language a domain expert will accept, you need both an
honest description of the goal it was given and a faithful translation of the
reasoning it actually used. Either half on its own is a demo. Together, they
start to look like a real tool.

## What's next

I am extending these ideas in two directions. One is mechanistic
interpretability for the language layer itself, treating the explainer as an
object of study, not just a wrapper. The other is moving beyond greenhouses
to controlled-environment systems with stricter safety requirements, where
the cost of an opaque decision is much higher.

If any of this resonates, or if you are working on something nearby, I would
genuinely like to hear from you. Email is the fastest way.
