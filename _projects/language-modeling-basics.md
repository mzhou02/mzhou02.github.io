---
layout: page
title: Language Modeling Foundations
description: An abstraction of the direct product
img: 
importance: 1
category: algebra
mathjax: true
---
<h3> A Less "Direct" Product </h3>
To begin our discussion of what a language model is, we first have to formally define what a language is. The simplest structure we can begin our discussion with is the idea of an alphabet, which is simply just a set of symbols denoted as $\Sigma$. From our alphabet, we can build strings, which are finite sequences of symbols formed by concatenation. Since concatenation is associative, we can define a concatenative closure, often called the Kleene closure of an alphabet denoted as $\Sigma^*$. Since the Kleene closure also includes the empty string, we have a nice monoid we can work with under the operation of concatenation. A language is then a subset of $\Sigma^*$.

\textbf{Note on terminology:} The terminology we use here is that from formal language theory. In practical settings, we can normally substitute ``alphabet" with any building block we use for language, like a \textbf{vocabulary} that will be comprised of either words or tokens. A string will then be synonymous with what we call sentences, and our language will be a subset of all possible sentences.

We now have enough tools to define what a language model is. In natural language processing, we define a language model as a probability distribution over $\Sigma^*$. There are two main types of language models (only one of which is practical, but we will talk about both in this set of notes): we can either define one that is locally normalized (in which we define probabilities of strings through a sequence model) or globally normalized (which is energy based).