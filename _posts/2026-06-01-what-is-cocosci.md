---
layout: distill
title: What is CoCoSci?
description: An introduction to Computational Cognitive Science.
tags: notes
date: 2026-06-01
giscus_comments: false

bibliography: cocosci.bib

toc:
  - name: Introduction
  - name: Models
  - name: Bayes
---

*These notes are adapted and framed almost entirely from <d-cite key="griffiths2008bayesian"></d-cite>, <d-cite key="navarro2024compcogsci"></d-cite>, <d-cite key="annualreview2024cocosci"></d-cite>, <d-cite key="probmods"></d-cite>.*

## Introduction

Humans are incredibly smart - incomparably to other living beings that we know of.

<aside>
<p>We have an unusually high <i>encephalisation ratio</i>, i.e. the ratio of brain mass to expected brain mass for any body mass.</p>
</aside>
But what is this intelligence used for?

The world is built on inherent structure. Our sensory systems work to detect that structure, despite being able to access only a small part of this data (for example, we do not have x-ray vision). To put this computation into context, a second of brain activity would be enough to exhaust forty thousand 300GB hard drives. However, we do not perceive the world as it exists; we instead perceive it in terms of a set of structures extrapolated from sense data. To do this, we form beliefs that go beyond the immediate sensory data available to us. We hear a squishy thwack and believe our daughter dropped an egg in the kitchen; we see a fleeting expression and believe our spouse has tired of these dinner guests; we smell a mix of rotten food and wet fur and believe our dog buried a bone in the compost heap.

<aside>
<p>The daughter and spouse are hypothetical, perhaps even the dinner guests if you are antisocial.</p>
</aside>

*How can we know so much from so little? How is this intelligence engineered?* We can see that there is often a strong mismatch between the information (input through our senses) and the outputs of cognition.

<aside>
<p>Cognition is the study of the more complex representations we construct, and how we use them to guide actions and behavior.</p>
</aside>

Regardless, we are able to build rich causal models of the world, by making strong generalizations and abstractions - to the extent that we even create new worlds and attempt to understand them. *Given that input data is often sparse, noisy, and incomplete - how does this happen?* For example: consider a child able to classify things and learn their names in new situations, despite conventional algorithmic approaches from statistics and machine learning requiring far more effort to train.

A similar example: people are usually able to understand the rules of a board game by observing a few played rounds. Even if these inferences were mildly inaccurate guesses with some uncertainty, these would still be far better than you would have made before seeing any play at all.

A third example: suppose you are asked to think of a rule that picks out any subset of numbers between 1 and 100. If you are additionally given four samples of numbers in the target subset, {50, 20, 80, 90}. Most people presume that the rule likely relies on multiples of 10 and respond with other numbers in that subset.

But why? This could just as easily be a subset of even numbers, or a set of numbers from 20 to 90. *Why is one way of representing data so much more compelling than others?* Perhaps even more interestingly; why would that same rule not have been nearly as compelling if you had been given only two samples, say 20 and 50, and asked to generalize at that point?

### Models

The deepest form of the generalization we discuss above is perhaps human learning over cognitive development - as we construct large-scale systems of knowledge for physics, biology, or even morality and social structure. These are known as *world models*, i.e. the human mind's models of causality in the world. Since we are not able to capture the entire state of the world, these models are inherently probabilistic and inferential. Any intuitive theory can then be quite transformative, in the sense that a single demonstration is enough to suggest that the world could work differently than previously perceived, and that this observation is eventually understood.

How do we then know, sometimes only from a single example - which observed generalization should hold and which should not? These questions continue to drive discussions about the origins of knowledge and philosophy of science. *What does it mean for a property to endure, or to be a law of nature? What is our justification for believing that a universal concept that has held everywhere and always will not be falsified some day?*

If the mind goes beyond the given data, then surely some other source of information must make up the difference.

To reverse-engineer the mind, we frame some questions:

1. How does abstract knowledge of the world guide learning and inference from sparse data?
2. What forms does our abstract world knowledge take across different domains and tasks?
3. How are world models themselves acquired or constructed?

We may question this further:

1. How do we use our world models to make decisions and act in the world successfully?
2. How can learning and inference with complex world models be implemented efficiently in minds with bounded computational resources?
3. How are complex world models implemented in a physical machine, brain, or computer?
4. What are the origins of our world models in evolution and development - what is built into our mind, and how do we learn within and beyond that starting point?
5. What is needed to scale up learning to all the knowledge that a human being acquires over their lifetime and human cultures have built over generations?

While there are several approaches to reverse-engineering the mind, these notes will largely continue to deal with the Bayesian and probabilistic approach, in which world models are mental representations or beliefs. Like any realistic knowledge that any realistic agent has, they will be incomplete and imperfect in many ways. Furthermore, they are approximate and probabilistic. But as we acknowledge; these are the best guesses we can make.

### Why Bayes?

Abstract knowledge is encoded in a probabilistic generative model, a kind of mental model that describes the causal processes in the world that give rise to the learner's observations, as well as unobserved latent variables that support effective prediction and action - but only if the learner can infer their hidden state.

<aside>
<p>Generative models must be probabilistic to handle the learner's uncertainty about the true states of latent variables and the true causal processes at work.</p>
</aside>

<aside>
<p>A generative model describes not only the specific situation at hand but also a broader class of situations over which learning should generalize.</p>
</aside>

Bayesian inference provides a rational framework for updating beliefs about latent variables in generative models given observed data. Background knowledge is encoded through a constrained space of hypothesis \( \mathcal{H} \) about possible values for latent variables - candidate world structures that could explain the observed data. Finer-grained knowledge comes in the prior probabilities \(P(h)\) that specify the learner's degree of belief in each hypothesis \(h\) prior to (or independent of) the observations. Bayes' rule updates these prior probabilities to posterior probabilities \(P(h\mid d)\) conditional on the observed data \(d\):

$$
P(h\mid d)=\frac{P(d\mid h)P(h)}{\sum_{h' \in \mathcal{H}}P(d\mid h')P(h')}
$$

For example: if the data is a thwack, one hypothesis is a dropped egg in the kitchen, another hypothesis is a dropped water balloon in the kitchen, and a third hypothesis is a dropped loaf of bread. The first and third have high priors \(P(h)\), while the second does not - eggs and bread are often dropped in kitchens, but water balloons only rarely. The first and second have high likelihoods \(P(d\mid h)\), while the third does not - dropped eggs and water balloons often thwack, but dropped bread rarely does. On this comparative basis, then, the first hypothesis is assigned the highest probability given the data \(P(h\mid d)\).

What form does our abstract world knowledge take across different circumstances? Bayesian models are able to do two fascinating things.

First, they unify a variety of cognitive acts under a single general framework. The very same abstract model usefully explains perception, memory, language, categorization, theory of mind, and much more.

Second, they are rational models; that is, they specify how an idealized agent with unbounded cognitive resources would optimally solve inference problems.

However, to be effective models, they must be:

1. *Parsimonious*: they must accurately fit the data without requiring too many parameters.
2. *Interpretable*: model parameters must specifically map onto something in cognition, or relate to some psychological idea.
3. *Teachable*: these models should teach us something about the human mind.

To achieve a more complete, complementary explanation of behavior, we may also consider *Marr's levels of analysis*, wherein we have:

1. *Computational*: what are the abstract problems the mind needs to solve, and what would a solution look like?
2. *Algorithmic*: what informational processing steps are followed to arrive at the solution?
3. *Implementation*: how does the brain carry these steps out?

<d-bibliography></d-bibliography>
