---
layout: post
title: "Why LLMs need to explain themselves: a control-systems perspective"
date: 2025-09-05
description: "Explanations are not a UX feature. They are a control-loop requirement."
tags: [Explainability, Control, LLM]
---

There is a quiet assumption in most modern AI deployments. The model
makes a decision, the operator accepts the decision, and the
explanation, if anyone bothers, is generated afterwards by a separate
post-hoc tool. SHAP, LIME, attention maps, the usual suspects. The
explanation is treated as a kind of receipt. Optional. Cosmetic.

In a control system this assumption falls apart immediately.

A controller is not a one-shot predictor. It runs in a loop. Every
sample period it picks an action, the plant moves, sensors update, the
controller picks the next action. The grower, the operator, the plant
manager, are not external auditors who occasionally check on it. They
are inside the loop. They override it, they retune it, they switch it
off when something feels wrong. The explanation is the channel through
which the human and the controller stay in sync. If the channel is
broken, the loop is broken.

That single observation reframes the whole explainability problem.
Explanations stop being a UX nicety. They become a control-loop
requirement, with the same status as observability or robustness
margins.

A few consequences follow.

First, latency matters. A 30-second explanation is useless when the
sample period is one minute. The explanation has to live on the same
timescale as the decision, otherwise the operator falls back to the
old habit of trusting their gut and ignoring the controller.

Second, faithfulness matters more than fluency. A confident,
plausible-sounding explanation that does not actually reflect what the
optimiser did is worse than no explanation at all. It teaches the
operator a wrong mental model of the plant. Every time the LLM smooths
over a constraint that was actually binding, it widens the gap between
the human's mental model and the real system. That gap is where
accidents happen.

Third, the explanation has to be grounded in something. Free-form text
out of a base LLM is not grounded in anything except its training
corpus. In a control loop the natural grounding is the optimiser
itself: the constraints, the cost terms, the active set, the
prediction horizon, the reference signal. The LLM should be reading
from those, not from a vibe.

Fourth, the explanation should be testable. If a controller says "I
turned the heater down because the predicted humidity was about to hit
the upper bound," that statement is either true or false in the
optimisation problem. We can check it. Explanations that are
not checkable are not engineering artefacts, they are decoration.

When you stack those four requirements, latency, faithfulness,
grounding, and testability, you end up in a very specific design
corner. Post-hoc methods like SHAP cannot satisfy faithfulness because
they approximate a different model. Pure prompt-engineered LLMs cannot
satisfy grounding because they do not have privileged access to the
optimiser. Attention maps cannot satisfy testability because there is
no map from "this attention head lit up" to "this constraint was
binding."

What does fit is a tighter coupling: an LLM that reads structured
state from the optimiser, a domain knowledge graph that constrains
which entities and relations the LLM is allowed to talk about, and a
verifier that checks every generated explanation back against the
optimisation problem before it is shown to a human.

That stack is what our 2025 *Smart Agricultural Technology* paper
prototypes. The greenhouse is incidental. The same stack would apply
to any plant where decisions are made by an optimiser and consumed by
a human. Building HVAC, district heating, autonomous trains,
fuel-cell stacks, all of them have the same shape.

The deeper claim is this. We have spent ten years arguing about
whether neural networks should be allowed near safety-critical
control. The right question turns out to be different. The question
is whether they can stay in the loop with humans, not whether they
can replace them. And that question is an explainability question
first.

Most of what comes next on this blog is about the engineering of that
loop.
