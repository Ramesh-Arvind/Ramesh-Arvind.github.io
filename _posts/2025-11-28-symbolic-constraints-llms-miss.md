---
layout: post
title: "Symbolic constraints, optimisation, and what LLMs miss"
date: 2025-11-28
description: "Why a model that can quote the textbook on KKT conditions still cannot reliably reason about them."
tags: [Optimisation, LLM, Control]
---

Ask a modern frontier model to state the Karush-Kuhn-Tucker
conditions for a constrained optimisation problem. It will give you
a clean answer. Stationarity, primal feasibility, dual feasibility,
complementary slackness. It can recite the textbook. Ask it to
identify the active set in a small numerical example, and it
sometimes gets that right too.

Now embed the same problem inside a control loop, give the model the
sensor readings and the cost weights and the constraint bounds, and
ask it to predict which constraint will become binding at the next
sample period. The accuracy collapses.

This is not a knowledge gap. The model knows the math. It is a
reasoning gap, and it is structural.

Optimisation reasoning has a particular shape that does not match how
language models compute. Three patterns make this concrete.

The first pattern is global feasibility. To know whether a candidate
solution is feasible, you have to evaluate every constraint, not the
ones that look relevant. Language models are very good at attending to
the most relevant tokens, which is the wrong attention pattern for
feasibility checking. They will quietly skip over a constraint that
looks numerically uninteresting and miss exactly the one that
matters.

The second pattern is the active set. In a constrained optimum, only
some constraints are tight. Identifying the active set is the central
combinatorial step in QP and NLP solvers, and there are mature
algorithms for it. Asking an LLM to do this implicitly, by reasoning
through it in natural language, is asking it to simulate a solver. It
can do this for very small problems. It does not scale. The error
mode is interesting: the model picks a plausible-looking active set,
then writes a confident justification for it, regardless of whether
the active set is actually correct.

The third pattern is the duality argument. KKT logic flows in both
directions. From the primal you can reason about the dual, and the
dual gives you the shadow prices that explain the primal. Language
models tend to flatten this into a single direction. They will
explain a primal decision in primal terms (we did X because the cost
of X was lowest) and skip the dual reasoning (we did X because the
shadow price on the constraint that would have ruled out Y was
higher than the shadow price on the constraint that would have ruled
out X). The dual story is often the more useful one for an operator,
and it is the one most likely to be lost.

These three patterns are not unique to LLMs. They show up in any
system that tries to reason about optimisation without actually
solving the optimisation. The difference is that a numerical solver
will tell you when it cannot find a feasible point. An LLM will
generate a fluent paragraph that sounds like it found one.

The engineering response, in our work, is to never ask the LLM to do
the optimisation reasoning by itself. The optimiser does the
optimisation. The LLM reads the optimiser's output, the active set,
the dual variables, the slack values, and translates that into a
human-readable explanation. The LLM is a translator, not a solver.

Once you accept that split, a lot of the disappointment with LLMs in
optimisation contexts goes away. The model is being used inside its
competence, on the linguistic and compositional side. The numerical
heavy lifting stays where it has always been good, in the solver.

The interesting research question that remains is the one in the
middle. Can a language model, given access to a solver as a tool,
reliably decide when to call the solver, what problem to pose to it,
and how to interpret its output? That is a non-trivial reasoning
problem in its own right, and it is closer to where the field is
going than the "just prompt the model harder" line.

I am cautiously optimistic about that direction. I am not optimistic
about LLMs as standalone optimisers, and I do not think any amount of
scaling alone fixes the three patterns above.
