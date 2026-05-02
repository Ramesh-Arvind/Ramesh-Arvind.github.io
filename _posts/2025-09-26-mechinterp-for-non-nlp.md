---
layout: post
title: "Mechanistic interpretability for non-NLP people, a primer"
date: 2025-09-26
description: "What the interpretability community has actually figured out, translated for engineers."
tags: [Interpretability, LLM, Primer]
---

If you work in control, robotics, or any other field where neural
networks are used as components rather than as the whole product, the
mechanistic interpretability literature looks intimidating. There is
a vocabulary problem. Circuits, features, superposition, induction
heads, monosemanticity, sparse autoencoders, probing, patching. Each
of these is a real concept with a real reason to exist, but the way
the field talks about them assumes you have read the last three years
of papers on transformer internals.

This post is a translation. It is the version I wish had existed when
I started reading this literature seriously.

Start with the basic question. Mechanistic interpretability is not
asking "which input features mattered for this output." That is the
SHAP question, and it has a different shape. Mechanistic
interpretability asks "which internal computations did the model
actually run to get from input to output." It treats the network as a
program and tries to reverse-engineer the program.

The unit of analysis is the circuit. A circuit is a small subset of
neurons, attention heads, and connections that together implement a
recognisable computation. The classic example is the induction head,
a two-attention-head circuit in transformers that implements pattern
completion of the form "A B ... A -> B." Once you know the circuit
exists, you can find it, ablate it, and watch the model fail at the
task. That is the kind of evidence the field treats as compelling.

Three concepts do most of the work.

**Features.** A feature is a direction in activation space that
corresponds to a human-interpretable concept. "This input mentions
the Eiffel Tower." "This token is the first one of a list."
"Temperature is rising." Features are the alphabet of the network's
internal language.

**Superposition.** Networks have far more features than neurons. They
solve this by storing features in overlapping linear combinations.
This is why you cannot just look at one neuron and read off "this
neuron is the temperature neuron." It almost never works that way.
The temperature feature is spread across many neurons, and many other
features share those same neurons.

**Sparse autoencoders.** The current best tool for getting around
superposition. You train a wide, sparse autoencoder on the model's
activations and let it discover an over-complete basis. Each basis
direction is a candidate feature. With enough scale, many of those
features turn out to be human-interpretable.

Now the part that matters for non-NLP people.

Almost every mechanistic interpretability technique developed for
language models transfers to any transformer. If you have a
transformer-based controller, a transformer-based world model, a
transformer-based perception module, the same toolkit applies. You
can probe activations, find features, identify circuits, ablate them,
and check whether your causal story holds.

What does not transfer cleanly is the intuition. Language models have
a token vocabulary and a discrete, compositional structure that makes
features feel natural. A controller that reads continuous sensor
inputs does not have tokens. The "alphabet" is harder to even define.
That does not mean features are absent. It means we have to do more
work to find them.

This is where my current work goes. I take a domain-adapted language
model that has been trained to reason about constrained control
problems, and I ask the standard mechinterp questions of it. Which
features encode the notion of a constraint being binding? Which
circuits handle the prediction horizon? Where does the model store
the difference between an objective and a constraint? Some of these
questions have promising preliminary answers. The full version is in
review and I will write about it once it is out.

The takeaway for now is not "go read 40 papers." The takeaway is this.
Mechanistic interpretability is a tractable engineering discipline,
not a philosophy. It has its own tools, its own evidence standards,
and its own failure modes. If you are deploying a neural network in a
loop with humans or hardware, knowing what is inside it is a
reasonable engineering expectation, not a research luxury.
