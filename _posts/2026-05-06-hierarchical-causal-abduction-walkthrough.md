---
layout: post
title: "Inside Hierarchical Causal Abduction, the framework behind our ICML 2026 paper"
date: 2026-05-06
description: A section-by-section walk through the framework, the three evidence streams, the reasoning engine, and the result that landed it at ICML.
tags: [Explainable AI, Model Predictive Control, Causality, ICML, Paper Walkthrough]
---

This is the long-form companion to our ICML 2026 paper, Hierarchical Causal Abduction. The paper is short on prose by design, because every page that is not equation or table is page you do not get for the next experiment. This post is the version with the prose put back in. If you want the formal version, the paper is on OpenReview. If you want the version that explains why we did each piece the way we did, read on.

The problem we kept running into

If you have read the earlier posts on this site, you already know the setup. Model predictive control is genuinely good at running things like greenhouses, building HVAC, and chemical process plants. The optimiser sees the future, picks an action sequence, and the result is usually safe and efficient. The trouble is that the operator standing in front of the controller cannot see what the optimiser saw. When something surprising happens, the answer they want is "why did it do that," and the answer the controller can give is "because I solved this optimisation problem." Both answers are true. Neither one is useful.

Existing post-hoc explainers do not close this gap. LIME and SHAP treat the controller as a black box and try to attribute the action to input features. That is the wrong unit of explanation for control. Operators do not want to know which input mattered most for the action. They want to know which constraint was about to bind, which physical mechanism was driving the dynamics, and what the consequence of doing nothing would have been. Those are not feature-attribution questions. They are causal questions about the system the controller is regulating.

So we set out to build an explainer that produces that kind of answer. The constraints we set ourselves were strict. The explanation has to be grounded in the actual artefacts the optimiser produces, not invented after the fact. It has to be physically consistent with the system being controlled, not just a plausible story. And it has to compose, so that we can say something useful about a chemical reactor today and an HVAC system tomorrow without rewriting the explainer from scratch.

Three evidence streams

The framework rests on the observation that an explanation of a control action has at least three sources of evidence, and each one is incomplete on its own.

The first source is physics and domain knowledge. We encode this as a knowledge graph whose nodes are quantities the operator already knows about, like zone temperature, supply temperature, cooling power, and outdoor conditions, and whose edges are typed relations like "causes," "influences," and "depends on." The graph is small, hand-curated for each domain, and reusable. It is not the model of the plant. It is the operator's mental model of the plant, written down in a form a reasoning system can use.

The second source is the optimiser itself. When the controller solves its problem, it does not just produce an action. It produces a Lagrangian, a set of KKT multipliers, and a sensitivity profile across the prediction horizon. Those artefacts are evidence in the strongest sense of the word. A non-zero multiplier on a constraint means that constraint was binding. A high sensitivity on a future stage means the controller was acting now to prevent a violation later. We extract these directly from the solver and treat them as first-class signals, not as side effects.

The third source is data. Even with a good model, the system surprises you. Things drift, sensors lie, and the model is always slightly wrong. To catch this, we run a temporal causal discovery procedure, in our case a PCMCI variant, on the recent operating history. The output is a sparse causal graph over the observed signals at lagged time steps. It tells us which variables are leading which, on the data we just saw, regardless of what the physics graph says.

Each of these three streams answers a different question. The knowledge graph answers "what could in principle cause what." The optimiser evidence answers "what did this controller think mattered, right now." The temporal causal graph answers "what is actually leading what, in the data." None of the three on its own is enough. The novel piece in the paper is the part that puts them together.

The hierarchical reasoning engine

Causal abduction is the inference pattern of choosing, among the explanations consistent with what you know, the simplest one that is sufficient. We use it as the spine of the framework. Given a control action and the three evidence streams, the reasoner searches for the smallest set of causes, drawn from the knowledge graph, that is consistent with the optimiser's KKT structure and supported by the temporal causal graph.

The "hierarchical" part is the bit that took us the longest to get right. A flat abduction search over a knowledge graph with even a few dozen nodes runs into combinatorial trouble fast, and the explanations it returns tend to be either trivial or unreadable. We instead organise the search in three levels. At the top, we identify which active constraint is the proximate cause, using the multipliers. In the middle, we trace back through the knowledge graph to the physical mechanism that drove the system toward that constraint. At the bottom, we cross-check the implied chain against the temporal causal graph, and we keep only chains whose links survive that check.

The result is a structured explanation with three layers, the constraint that is binding, the mechanism that drove the system there, and the data-grounded check that the chain holds. Each layer is independently auditable, which matters more than people think. An operator who does not trust the data-grounded check can still reason about the constraint and the mechanism. An auditor who does not trust the knowledge graph can still verify the multipliers. Decomposing the explanation in this way is what makes it possible to argue with it, and a system you can argue with is a system you eventually trust.

What the experiments actually show

We ran the framework on three domains, greenhouse climate control, building HVAC, and chemical process control. Each domain has its own knowledge graph and its own controller, but the reasoning engine is the same. The headline number is that the explanations the framework produces match expert annotations on a held-out set with 53 percent higher accuracy than LIME. The number itself is less important than what is hiding behind it. LIME is doing feature attribution and we are doing constraint and mechanism reasoning. The 53 percent gap is partly LIME being asked the wrong question, and we say so in the paper. The more honest comparison is against ablations of our own framework, where we drop one of the three evidence streams and watch what breaks.

The ablations are where the design pays for itself. Without the optimiser evidence, the explanations become physics fiction, plausible but unconnected to the actual decision. Without the knowledge graph, the explanations become data archaeology, statistically correct but physically meaningless. Without the temporal causal graph, the explanations become brittle to model error. The three streams are not redundant. Each of them blocks a specific failure mode that the other two cannot.

The thing we are most happy about is operator behaviour. When we sat with practitioners and watched them use the system, the conversations changed. They stopped asking "what did the controller do" and started asking "do I agree with the chain it gave me." That is a different shape of question, and it is the shape a real engineering tool should be producing.

Limitations and the parts we are still chewing on

The honest list is short and I would rather you hear it from me than discover it on your own.

The knowledge graph is hand-curated, which is fine for one domain at a time but does not scale to a fleet. The temporal causal discovery is run offline on windows of data, not online, and the cost of refreshing it is non-trivial. The framework assumes the optimiser is solving the problem you think it is solving, which is true in our setups and not always true in deployed industrial control. Finally, the 53 percent number is on a benchmark we constructed; we are working on broader benchmarks and would welcome collaboration on that front.

The next step we are excited about is dropping the hand-curation. Mechanistic interpretability of domain-adapted language models gives us a path to extracting something knowledge-graph-shaped directly from a model that has read the relevant literature. That is a separate post, and a separate paper.

Where to go from here

If you want the formal version, the paper is at OpenReview, [22XF7II0Hd](https://openreview.net/forum?id=22XF7II0Hd). If you want a sense of where this framework sits in the broader arc of our work, the [August 2025 post on our two greenhouse papers](/blog/2025/explainable-greenhouse-control/) is the right precursor. And if you want to go deeper into the interpretability machinery underneath the next iteration, the [mechanistic interpretability primer](/blog/2025/mechinterp-for-non-nlp/) is the right starting point.
