---
layout: post
title: "Our ICML 2026 paper"
date: 2026-05-05
description: A short note on the acceptance, what the paper is about, and what comes next.
tags: [ICML, Explainable AI, Announcement]
---

Quick note that our paper, Hierarchical Causal Abduction: A Foundation Framework for Explainable Model Predictive Control, has been accepted at ICML 2026. The conference is in Seoul, at COEX, from the 6th to the 11th of July 2026. The paper is joint work with Zühal Wagner and my supervisor, Prof. Dr. Stefan Streif, at the Professorship of Automatic Control and System Dynamics, TU Chemnitz.

![Hierarchical Causal Abduction: a one-figure summary of the framework, the three evidence streams, and the headline results.]({{ '/assets/img/hca-icml2026.png' | relative_url }})

The figure above is the one-page version of the paper. The black-box MPC controller on the top left is the problem, the operators next to it are the audience, and the three panels in the middle are the framework. Panel one is the physics and domain knowledge encoded as a graph. Panel two is the optimiser's own KKT structure used as evidence about which constraints actually drove the action. Panel three is the temporal causal graph learned from recent data with PCMCI. The hierarchical reasoning engine in the centre is the part that combines the three streams into a single auditable chain. The bottom row is what comes out the other side, a 53 percent improvement over LIME, validation across three domains, and an expert clarity score of 4.3 out of 5. The before-and-after on the right is the part that matters most to me, an operator going from "why did heating activate" to a forward-looking explanation about preventing a constraint violation two hours from now.

ICML 2026 received 23,918 submissions and accepted 6,352 of them, which puts the acceptance rate around 26.6 percent. I am genuinely thankful that this work made it through. The community of reviewers around explainable control at top ML venues is small and rigorous, and getting useful feedback there is itself a privilege.

The short version of the paper. We propose a framework that builds explanations for model predictive control by combining three things, a physics knowledge graph that captures what the operator already knows, the KKT structure of the controller's own optimisation as evidence about which constraints actually drove the action, and a temporal causal graph learned from recent operating data. A hierarchical causal abduction procedure puts the three together and produces explanations that are auditable at three different levels, the binding constraint, the physical mechanism, and the data-grounded check. We test it on greenhouse climate, building HVAC, and chemical process control, and we improve explanation accuracy by 53 percent over LIME on our benchmark.

There is a longer companion post on this blog that walks through the framework section by section, including the failure modes the design was meant to block and the limitations we are still working on. If you came here for the technical version, that one is the right next click.

I will be at ICML in Seoul. If you work on explainable control, causality, mechanistic interpretability of domain-adapted models, or trustworthy AI for safety-critical systems, please come and find me. Conferences are mostly hallway conversations for me, and the hallway is where the most useful arguments tend to happen.
