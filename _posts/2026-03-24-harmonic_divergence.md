---
title: "A probabilistic proof of divergence of the harmonic series"
---

<div style="text-align: center; max-width: 600px; margin: auto;">
  <img 
    src="/assets/img/blog/Oresme.jpg" 
    alt="Nicole Oresme" 
    style="width: 70%; height: auto; display: block; margin: 0 auto;" 
  />
  <p style="font-style: italic; color: #555; margin-top: 0.5rem;">Nicole Oresme (c. 1325 - 1382), an influential polymath who discovered the first known proof of divergence of the harmonic series.</p>
</div>

When researching for [my last blog post](https://n2pakbong.github.io/blog/2026/02/27/infinity/) on large numbers, I had a lot of fun reading some very creative proofs of the divergence of the harmonic series [1,2]. In this post, I'm going to present (a slightly modified version of) my favourite one so far: a probabilistic proof based on the invariance of random i.i.d. sequences under reindexing, originally due to [Omer Adelman](https://perso.lpsm.paris/~oadelman/) [3]. I was originally planning to include this proof in the previous post, but as that got already longer than I had anticipated, I thought I'd better dedicate a post to it (which will also give me more space to flesh out the argument a bit). Before getting started with the proof, I have to mention that another very slick probabilistic proof of the divergence of the harmonic series has been given by [Arnab Kumar Laha](https://www.iima.ac.in/faculty-research/faculty-directory/arnab-kumar-laha) [4], it is most definitely worth a read!

## The Proof

Now, onto the proof. Recall that our goal is to show the following

> **Theorem.** The sum 
>
> $$
  H_n:= \sum_{k=1}^n\frac1k = 1 + \frac12 + \frac 13 + \cdots + \frac1n, 
  $$
> 
> diverges as $n\to\infty$.

**Proof.** Let $X_1,X_2,\ldots$ be a sequence of i.i.d. random variables uniformly distributed on the unit interval, and denote by $R$ the (random) set of *record times*, namely:

$$
R := \{n\in\mathbb N: X_n > X_j \text{ for all } j< n\}.
$$

We claim that $R$ has almost surely infinitely many elements. To see this, we will need the following

> **Lemma.** let $\sigma : \mathbb N \to \mathbb N$ be an injective mapping. Then, the sequences $X_1,X_2,\ldots$ and $X_{\sigma(1)},X_{\sigma(2)},\ldots$ have the same distribution.

Although this Lemma seems very obvious, proving it rigorously is still a nice little exercise in measure-theoretic probability, and I'll thus include a full proof of it in the appendix. Assuming that the Lemma is true, let's now show that $R$ has infinite cardinality: define for all $n\in\mathbb N$ the *"no more records after time $n$"* events:

$$
A_n := \{X_j \le X_n \text{ for all } j> n\}.
$$

By the Lemma applied to the injection $\sigma:k\mapsto k+ (n-m)$, we see that for any $n>m$, $A_n$ and $A_m$ have the same probability (and of course the same is true if $m<n$). We have thus shown that $\mathbb P(A_n) = \mathbb P(A_1)$ for all $n$, but it is not hard to see that $A_1$ has zero probability: indeed, denote by $A_1^{(N)}$ the event

$$ 
A_1^{(N)} := \{X_j \le X_1 \text{ for all } 2\le j\le N\}.
$$

It is easily seen (by another application of the Lemma, if you want to do it formally) that $A_1^{(N)}$ has probability $1/N$, since $X_1$ has same probabliity as $X_2, X_3,\ldots,X_N$ to be the maximum of these $N$ variables. It is also clear that 

$$ 
A_1^{(N+1)}\subseteq A_1^{(N)}, \text{ and } A_1 = \bigcap_{N=2}^\infty A_1^{(N)}.
$$

Therefore, by [monotonicity and continuity](https://en.wikipedia.org/wiki/Measure_(mathematics)#Continuity_from_above) of probability measures, it follows that $\mathbb P(A_1) = \lim_{N\to\infty} 1/N = 0$. Going back to our random set $R$, we can thus conclude the following:

$$
\mathbb P(R \text{ is finite}) = \mathbb P\left(\bigcup_{n=1}^\infty A_n\right) \le \sum_{n=1}^\infty \mathbb P(A_n) =  \sum_{n=1}^\infty \mathbb P(A_1) =  \sum_{n=1}^\infty 0 = 0.
$$

So we have shown that the cardinal of $R$ is almost surely infinite. In particular, its expectation is also infinite. Now, recall by the same symmetry argument from above that the event $\left\{ n\in R \right\} = \left\{ X_n > X_j \text{ for all } j< n \right\}$ has probability $1/n$ by invariance under permutation. By [Tonelli's theorem](https://en.wikipedia.org/wiki/Fubini's_theorem#Tonelli's_theorem_for_non-negative_measurable_functions) for non-negative measurable functions, we can therefore exchange $\mathbb E$ and $\sum$ to conclude that

$$ 
\infty = \mathbb E[|R|] = \mathbb E\left[\sum_{n=1}^\infty \mathbf{1}_{n\in R}\right] = \sum_{n=1}^\infty \mathbb P(n\in R) = \sum_{n=1}^\infty \frac 1n,
$$

and so we are done. Neat, wasn't it?

## References
[1] Kifowit, Steven J., and Terra A. Stamps. “The Harmonic Series Diverges Again and Again.” AMATYC Review 27.2 (2006): 31–43.

[2] Kifowit, Steven J. “More proofs of divergence of the harmonic series.” Unpublished article available at http://skifowit.prairiestate.edu (2006).

[3] Adelman, Omer. “Σ∞: A Micro-Lesson on Probability and Symmetry.” The American Mathematical Monthly 114.9 (2007): 809–810.

[4] Laha, Arnab Kumar. "A proof of divergence of the harmonic series using probability theory." International Journal of Mathematical Education in Science and Technology 37.4 (2006): 502-503.


## Appendix

**Proof of Lemma.** Let $\sigma:\mathbb N \to \mathbb N$ be injective, $d\in\mathbb N$ be any natural number, and $n_1,\ldots,n_d$ be a finite collection of *distinct* numbers. We will begin by showing that the random vectors $\vec X_d := (X_{n_1},\ldots, X_{n_d})$ and $\vec Y_d := (X_{\sigma(n_1)},\ldots, X_{n_d})$ have the same distribution. To that end, let $R_d := [a_1,b_1]\times \cdots\times [a_d, b_d]$ be a "rectangle", and observe by the i.i.d. assumption on the $X_i$'s that

$$ 
\mathbb P(\vec X_d \in R_d) = \prod_{i=1}^d \mathbb P(X_{n_i} \in [a_i, b_i]) = \prod_{i=1}^d \mathbb P(X_{i} \in [a_i, b_i]) = \prod_{i=1}^d \mathbb P(X_{\sigma(n_i)} \in [a_i, b_i]) = \mathbb P(\vec Y_d \in R_d). 
$$
