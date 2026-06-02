---
layout: distill
title: Generative Models in WebPPL
description: A short introduction to running generative models using the WebPPL language.
tags: notes
date: 2026-06-02
giscus_comments: false

bibliography: 2026-06-02-genmodels.bib

toc:
  - name: Generative Models
    subsections:
    - name: Prediction, Simulation, and Probabilities
    - name: Marginal Distributions
    - name: Rules of Probability
  - name: Stochastic Recursion
  - name: Persistent Randomness
---
These are notes from Chapter 1 of [@goodman2016probmods]
## Generative Models

We would like to generate states of the world, or more specifically steps that unfold and lead to some potentially observable states. These processes can be described as computations - those that involve random choices to capture uncertainty about the process.

For example, the `flip` function can simulate a (possibly biased) coin toss. Although, we note that technically it samples from a Bernoulli distribution.

~~~~javascript
flip()
~~~~

~~~~javascript
// visualization and repetition
viz(repeat(1000,flip))
~~~~

We can also construct more complex expressions, for example: sampling up a number adding up several flips. In JavaScript, a boolean will be turned into a number, 0 or 1, by the `+` operator:

~~~~javascript
flip() + flip() + flip()
~~~~

~~~~javascript
var sumflips = function() {
  return flip() + flip() + flip()
}
viz(repeat(100, sumflips))
~~~~

Here is a stochastic function that will only sometimes double its input x:

(Structure: `condition ? value-if-true : value-if-false`)

~~~~javascript
var noisydouble = function(x) {
  return flip() ? x+x : x
}
noisydouble(3)
~~~~

The following program defines a fair coin, and flips it 20 times:

~~~~javascript
// making a coin
var faircoin = function() {
  return flip(0.5) ? 'h' : 't'
}
viz(repeat(20,faircoin))
~~~~

~~~~javascript
var trickcoin = function() {
  return flip(0.9) ? 'h' : 't'
}
viz(repeat(20,trickcoin))
~~~~

We can also define functions within functions. For example, to manufacture a coin:

~~~~javascript
var makecoin = function(weight) {
  return function() {
    return flip(weight) ? 'h' : 't'
  }
}

var faircoin = makecoin(0.5)
var trickcoin = makecoin(0.95)
var bentcoin = makecoin(0.25)

viz(repeat(20,faircoin))
viz(repeat(20,trickcoin))
viz(repeat(20,bentcoin))
~~~~

To encode causal knowledge as models: an example in medical diagnosis. 

It first specifies the base rates of two diseases the patient could have: lung cancer is rare while a cold is common, and there is an independent chance of having each disease. 

The following program specifies a process for generating a common symptom of these diseases - an effect with two possible causes. The patient coughs if they have a cold or lung cancer (or both).

~~~~javascript
// defining probabilities
var lungcancer = flip(0.01)
var cold = flip(0.2)
var cough = cold || lungcancer

cough
~~~~

Now we define four possible diseases and four symptoms. Each disease causes a different pattern of symptoms. The causal relations are probabilistic: only some patients with a cold have a cough (50%), or a fever (30%). The initial variables define the probability of you having each affliction, and we further go into each symptom to check for causation.

~~~~javascript
// independent probabilities of afflictions
var lungcancer = flip(0.01)
var tb = flip(0.005)
var stomachflu = flip(0.1)
var cold = flip(0.2)
var other = flip(0.1)

// probability of symptoms
var cough = 
    (cold && flip(0.5)) || // patient with a cold develops a cough 50% of the time
    (lungcancer && flip(0.3)) || // patient with lungcancer develops a cough 30% of the time
    (tb && flip(0.7)) ||
    (other && flip(0.01))
// or signifies that the patient coughs if any of these mechanisms causes coughing

var fever = 
    (cold && flip(0.3)) ||
    (stomachflu && flip(0.5)) ||
    (tb && flip(0.1)) ||
    (other && flip(0.01))

var chestpain = 
    (lungcancer && flip(0.5)) ||
    (tb && flip(0.5)) ||
    (other && flip(0.01))

var shortnessofbreath = 
    (lungcancer && flip(0.5)) ||
    (tb && flip(0.2)) ||
    (other && flip(0.01))

// generating a hypothetical patient's symptoms
var symptoms = {
  cough: cough,
  fever: fever,
  chestpain: chestpain,
  shortnessofbreath: shortnessofbreath
}

symptoms
~~~~

### Prediction, Simulation, and Probabilities

Suppose we flip two fair coins and return the list of their values.

~~~~javascript
[flip(), flip()]
~~~~

How can we predict the return value of this program? For instance, how likely is it that we will see `[true, false]`? We can examine the probability distribution on values that can be returned by the above program by sampling many times and examining the histogram of return values.

~~~~javascript
var randompair = function() {
  return [flip(), flip()]
}
viz.hist(repeat(1000, randompair), 'return values')
~~~~

We can think of `flip` in two different ways. From one perspective, it is a procedure which returns a sample from a fair coin.

From another perspective, `flip` is itself a characterization of the probability distribution over `true` and `false`. To make this view explicit, WebPPL has a special type of distribution objects. These are objects that can be sampled with the `sample` operator, and that can explicitly return the probability of a return value using the `score` method.

~~~~javascript
// make a Bernoulli distribution
var b = Bernoulli({p: 0.5})

// sample from it with the sample operator
print(sample(b))

// compute log-probability of sampling true
print(b.score(true))

// visualize distribution
viz(b)
~~~~

### Marginal Distributions: `infer`

Marginal distributions are constructed using the `infer` operator, which lets us reify the distribution implicitly with a sampling process.

~~~~javascript
// create a gaussian distribution with mean mu and std dev sigma
var g = Gaussian({mu: 0, sigma: 1})

// sample a random number from the deviation by initially creating a distribution
print(sample(g))

// alternatively we can immediately sample without having to create a distribution
print(gaussian(0,1))

// build complex processes
var foo = function() {return gaussian(0,1) * gaussian(0,1)}
foo()

// make marginal distributions on return values
var d = Infer({method: 'forward', samples: 1000}, foo)

print(sample(d))
viz(d)
~~~~

### Rules of Probability

The probability of two random choices is the product of their individual probabilities. The probability of several random choices together is known as joint probability, or P(A,B). Therefore, when we want the probability of a specific sequence of events, multiply:

We now know `C` being `[true, false]` to have a probability of 0.25, since this follows from the product rule of probability.

~~~~javascript
var A = flip()
var B = flip()
var C= [A, B] // since both are independent, we simply just multiply
C
~~~~

We should be careful here, since the probability of a choice can depend on the probabilities of previous choices. For example: to visualize the exact probability of `[true, false]` using `Infer`:

~~~~javascript
Infer({method: 'forward', samples: 10000}, function() {
  var A = flip()
  var B = flip(A ? 0.3 : 0.7) 
  // B's probability is dependent on A, is near 0.15 and 0.35 using Bayes
  return {'B': B, 'A': A}
})
~~~~

When multiple paths lead to the same outcome, we add them.

As a rule of thumb, we add the probabilities for individual paths for `||`, and multiply for `&&`.

~~~~javascript
flip() || flip() // adds to 0.25 + 0.25 + 0.25 for TF+FT+TT probabilities
~~~~

## Stochastic Recursion

Recursion functions, where the solution depends on solutions to smaller instances of the same problem, are useful for computational problems. In WebPPL we can have stochastic recursion that randomly decides when to stop. 

For example, the _geometric distribution_ is a probability distribution over the non-negative integers. We imagine flipping a weighted coin, returning N-1 if the first `true` is on the Nth flip.

~~~~javascript
var geometric = function (p) {
  return flip(p) ? 0 : 1 + geometric(p)
}
var g = Infer({method: 'forward', samples: 1000}, function() {return geometric(0.6)})
viz(g)
~~~~

## Persistent Randomness: `mem`

This is used to model a set of objects that have a randomly chosen property. For instance, to describe the eye colors of a set of people, we may implement the following.

~~~~javascript
var eyeColor = function (person) {
  return uniformDraw(['blue', 'green', 'brown'])
};

[eyeColor('bob'), eyeColor('alice'), eyeColor('bob')]
~~~~

However, this process is inconsistent; we get a different color for each's eyes every time we run this. We want a model that gives us random, but persistent colors. `memo` helps us do this by storing initial outputs. Below, we notice that Bob's eyes stay the same color for each run.

~~~~javascript
var eyeColor = mem(function (person) {
  return uniformDraw(['blue', 'green', 'brown'])
});

[eyeColor('bob'), eyeColor('alice'), eyeColor('bob')]
~~~~

~~~~javascript
// a simple example to drive the point home
flip() == flip()
~~~~

~~~~javascript
var memflip = mem(flip)
memflip() == memflip()
~~~~

We can use this for _lazy construction_. For example, to compute the result of the nth coin flip without having to flip the coin n times. Here, we notice that `flipAlot` gives us the same result for the first flip both times, and likewise for the rest.

~~~~javascript
var flipAlot = mem(function (n) {
  return flip()
});

[[flipAlot(1), flipAlot(12), flipAlot(47), flipAlot(1548)],
 [flipAlot(1), flipAlot(12), flipAlot(47), flipAlot(1548)]]
~~~~
