---
layout: post
title: "Domain knowledge graphs as scaffolds for LLM reasoning"
date: 2025-10-17
description: "Why a small, hand-built graph beats a large, retrieved corpus in narrow domains."
tags: [Knowledge Graphs, LLM, Grounding]
---

Retrieval-augmented generation is the default answer to "how do I make
an LLM stop hallucinating." Index your documents, retrieve the top-k
chunks, stuff them into the context window, and let the model
generate. It works surprisingly well on broad domains, customer
support, legal search, internal wikis. It works much less well on
narrow, technical, control-heavy domains. There is a structural
reason for this, and it points to a different design.

Retrieval over a corpus assumes that the right answer is somewhere in
the corpus, expressed in roughly the right words. In a narrow domain
like greenhouse climate control, the right answer is almost never
expressed in the corpus. The corpus has fragments. It has a paper on
vapour-pressure deficit, a manual on a specific climate computer, a
PhD thesis on tomato transpiration, an FAQ on dehumidification. The
operator's actual question, "why did the controller open the
ventilation flap right now," is a composition of those fragments, and
the composition is the hard part.

Knowledge graphs are good at exactly that compositional layer. A
graph is a set of entities and a set of typed relations between them.
For a greenhouse, the entities are sensors, actuators, climate
variables, plant physiology states, and constraints. The relations
are things like "ventilation flap actuator influences humidity
variable," "humidity variable affects fungal-disease risk,"
"fungal-disease risk is constrained below threshold X for cultivar
Y." That is a small graph, a few hundred nodes, a few thousand edges.
You can build it by hand with a domain expert in two afternoons.

The interesting move is what you do with the graph at inference time.

Naive use is bad. If you simply dump the entire graph into the
context window, you have just made the LLM read a long, structured
document, and you are back to the corpus-retrieval problem with extra
steps.

The right use is constrained. The graph becomes a vocabulary that the
LLM is allowed to talk about. When the model generates an
explanation, it is required to express the explanation in terms of
graph nodes and edges. Anything that does not reduce to the graph is
flagged as ungrounded. The graph is not extra context. It is a
contract about what the model is allowed to claim.

This is the move our 2025 *Smart Agricultural Technology* paper
makes. We pair a model predictive controller with an LLM, and the LLM
is forced to stay inside the domain knowledge graph when it explains
a control action. The controller decides what to do. The graph
decides what the explanation is allowed to say. The LLM does the
linguistic gluing.

Three things become easier once you do this.

**Verification.** You can check that every entity and every relation
in the explanation actually exists in the graph. If the model invents
a new variable, the verifier catches it. This eliminates a whole
class of confident-sounding hallucinations.

**Editing.** When the domain expert disagrees with an explanation,
they can change the graph. They cannot easily change a 70-billion-parameter
language model. The graph gives the human a steering wheel that the
model cannot ignore.

**Cross-domain reuse.** The LLM stays the same. The graph swaps. Move
from greenhouse to building HVAC and you swap the entities and
relations, you do not retrain anything.

The cost is real. Building the graph is the unglamorous part of the
work. It needs domain interviews, careful ontology decisions, and
upkeep as the underlying plant changes. It also caps the system's
expressivity at whatever the graph contains. If the graph does not
have a node for "leaf wetness," the system cannot explain in terms of
leaf wetness, even if the underlying physics involves it. That is a
feature, not a bug, in safety-critical contexts. The system fails
visibly, in the graph, where a human can see it, rather than
invisibly, inside a transformer, where they cannot.

The pattern generalises beyond control. Any domain where the corpus
is sparse, the variables are well-defined, and the cost of
hallucination is high, is a domain where knowledge graphs as
scaffolds beat retrieval over text. Medicine, manufacturing, energy
systems, all of them fit. The trick is having the patience to build
the graph.
