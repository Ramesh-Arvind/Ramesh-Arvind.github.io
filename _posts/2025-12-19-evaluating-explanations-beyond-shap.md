---
layout: post
title: "Evaluating explanations: why LIME and SHAP are not enough"
date: 2025-12-19
description: "Faithfulness, stability, and operator utility, three axes the standard tools do not measure."
tags: [Explainability, Evaluation, SHAP]
---

LIME and SHAP have done the field a great service. They made
explainability operational. Before them, "the model is interpretable"
was a vibe. After them, it was a number you could put in a paper.
That was real progress.

The cost of that progress is that we now treat the number as the
goal. A SHAP value is a measure of marginal contribution under a
specific game-theoretic assumption. It is one slice of what an
explanation could mean. Used as the only slice, it misleads.

Three axes are missing from the SHAP-shaped picture, and any serious
evaluation of an explanation system has to address all three.

**Faithfulness.** Does the explanation describe what the model
actually did, or does it describe what a simpler surrogate model
would have done in its place? LIME explicitly fits a local linear
surrogate, which is honest but limits the faithfulness ceiling. SHAP
estimates contributions under feature-coalition reasoning, which
makes specific assumptions about feature independence that are
routinely violated. For a deployed control system the right
faithfulness test is operational, not statistical. Take the
explanation, perturb the input in the way the explanation says
matters, and check that the model's output changes the way the
explanation predicts. If it does not, the explanation is not
faithful, regardless of what its SHAP values say.

**Stability.** Does a small change in input produce a small change
in explanation? In safety-critical settings, an explanation that
flips between contradictory stories on neighbouring inputs is
dangerous. It trains operators to ignore the explanation entirely.
Stability is testable. Sample neighbouring inputs, generate
explanations, and measure the distance between explanations under a
sensible metric. If the distance is high while the model output
barely moved, the explanation system is unstable, and you have a
problem.

**Operator utility.** Does the explanation actually help a human do
their job better, or does it just feel informative. This is the test
that gets skipped most often, because it is the most expensive. It
needs a study with real operators, real tasks, and a measurable
outcome. Decision time. Override rate. Detection of induced faults.
The literature on this is thin and mostly comes from medical AI.
Control systems need their own version of this work, and not enough
of it exists yet.

The methods we developed in our 2025 papers were tested on the first
two axes during paper review. The third axis, operator utility, is
where I want the next chunk of work to live. It is harder, slower,
and less publishable per unit time. It is also the only axis that
matters when the system is actually running in a glasshouse with a
real grower making real decisions.

A short note on tooling. SHAP is not the enemy. I still use it as one
diagnostic among several, and I would still default to it for tabular
models in low-stakes settings. The mistake is to treat it as the only
diagnostic, the way too many papers do. The right stance is closer to
how a control engineer thinks about Bode plots: useful, well
understood, decisive in some questions, silent on others. You would
not certify a controller on a Bode plot alone. You should not certify
an explanation system on SHAP alone.

What I would like to see in the next year of explainability papers,
in roughly priority order. More operator-in-the-loop studies. More
stability analyses, especially in time-series and control settings.
More work on explanation methods that are faithful by construction,
like our optimiser-grounded approach, instead of faithful by
post-hoc approximation. And less benchmarking against MNIST.

The benchmark for an explanation is whether it changes a human
decision in the right direction. Everything else is a proxy.
