---
layout: page
title: Language Modeling Foundations
description: This article is still in the works
img: 
importance: 1
category: nlp
mathjax: true
---
<script async src="https://www.googletagmanager.com/gtag/js?id=G-0823RLC0T3"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-0823RLC0T3');
</script>

To begin our discussion of what a language model is, we first have to formally define what a language is. The simplest structure we can start with is the idea of an alphabet, which is merely just a set of symbols denoted as $$\Sigma$$. From our alphabet, we can build strings, which are finite sequences of symbols formed by concatenation. Since concatenation is associative, we can define a concatenative closure, often called the Kleene closure of an alphabet, denoted as $$\Sigma^*$$. Intuitively, this is just the set of all finite strings generated from our alphabet. Since the Kleene closure also includes the empty string, it provides a nice monoid we can work with. We then define a language as a subset of $$\Sigma^*$$.

<b>Note on terminology:</b> The terminology we use here is drawn from formal language theory. In practical settings, we can normally substitute "alphabet" with any building block we use for language, like a <b> vocabulary </b> that will be comprised of either words or tokens. A string will then be synonymous with what we call sentences, and our language will be a subset of all possible sentences.

We now have enough tools to define what a language model is. In natural language processing, we define a language model as a probability distribution over $$\Sigma^*$$. There are two main types of language models, though only one is actually practical at the moment: we can either define one that is locally normalized (in which we define probabilities of strings through a sequence model) or globally normalized (which is energy based).

<h2>Globally Normalized Language Models</h2>
We will first talk about the less practical of the two models: globally normalized language models. A globally normalized language model is a model equipped with an energy function $$\hat{p}$$, that maps values from $$\Sigma^*$$ to the reals. Just as we commonly do in statistical learning theory, we define the unnormalized probability of a string $$y$$ to be $$\text{exp}(-\hat{p}(y))$$, and divide it by the normalization constant, $$Z = \sum_{y \in \Sigma^*}\text{exp}(-\hat{p}(y))$$, to get the probability of generating that string. In other words:

$$p_{\text{LM}}(y) = \frac{\text{exp}(-\hat{p}(y))}{\sum_{y' \in \Sigma^*}\text{exp}(-\hat{p}(y'))}$$

At a first glance, this approach seems attractive since all we have to do is define an energy function to have our language model, which is often easier than setting a probability distribution. However, on a second glance, computing the normalization constant $$Z$$ seems a bit unintuitive, as $$\Sigma^*$$ contains infinitely many strings. Does it even converge? In addition, how would we sample?

<h2>Locally Normalized Language Models</h2>

The inherent difficulty of computing the normalization constant motivates the definition of the locally normalized language model. Rather than directly defining a language model as a distribution over $$\Sigma^*$$, we can break our problem down into pieces, by generating a string a symbol at a time. The idea is, for each possible context $$y \in \Sigma^*$$, we define the probability distribution over the conditional $$y$$. In other words, we define a sequence model $$p_{\text{SM}}$$ such that

$$p(xy) = p_{\text{SM}}(x \mid y)$$

where

$$\sum_{x \in \Sigma} p_{\text{SM}}(x \mid y) = 1$$

The probability of generating a string $$y$$ is then

$$p_{\text{LM}}(y) = \prod_{l = 1}^L p_{\text{SM}}(y_l \mid y_{<l})$$

It's tempting to think that we're done. But we're not. Notice how since probabilities are at most 1, the probability of generating a string decreases with its length. Thus, statements like "Upenn is in" are more likely to be sampled than "UPenn is in Philadelphia". To get around this, we introduce the EOS (End of String) symbol into the distribution, so that $$p_{S\text{SM}}$$ maps values from $$\overline{\Sigma} = \Sigma \cup \{\text{EOS}\}$$ to the reals. We then have that

$$p_{\text{LM}}(y) = p_{\text{SM}}(\text{EOS} \mid y)\prod_{l = 1}^L p_{\text{SM}}(y_l \mid y_{<l})$$