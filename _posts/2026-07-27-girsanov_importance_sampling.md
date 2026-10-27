---
title: "Importance sampling, mean exit times, and Girsanov theorem"
published: false
---

<!-- Last updated: 2026-10-28 04:39 +08:00. -->



I have recently-ish gone through the exercise of writing my PhD dissertation (hooray!), which, to nobody's surprise, ended up quite a lot longer than I had initially envisioned (especially the acknowledgement section!!). As a consequence, a good chunk of the material I had initially planned to include therein ended up having to be left out. I therefore decided to write this post to record, a bit informally, one such thesis chunk which did not make the final cut, but which I nonetheless find important and interesting.

## The overdamped Langevin SDE

For <span class="math inline" markdown="0">\(d\ge 2\)</span>, let <span class="math inline" markdown="0">\(\Omega\subset\mathbb R^d\)</span> be a bounded open connected set with smooth enough boundary, and consider a continuous-time <span class="math inline" markdown="0">\(\mathbb R^d\)</span>-valued stochastic process <span class="math inline" markdown="0">\((X_t)_{t\ge 0}\)</span> solving the stochastic differential equation

<div class="math display" markdown="0">
\[

dX_t = b(t, X_t)dt + \sigma(t,X_t)dW_t, \quad X_0= x \tag{1},

\]
</div>



where <span class="math inline" markdown="0">\(x\in\Omega\)</span>, <span class="math inline" markdown="0">\((W_t)_{t\ge0}\)</span> is a standard Brownian motion on <span class="math inline" markdown="0">\(\mathbb R^d\)</span>, and the coefficients <span class="math inline" markdown="0">\(b\)</span> and <span class="math inline" markdown="0">\(\sigma\)</span>, which are respectively <span class="math inline" markdown="0">\(\mathbb R^d\)</span> and <span class="math inline" markdown="0">\(\mathbb R^{d\times d}\)</span>-valued, are assumed to be sufficiently regular for <span class="math inline" markdown="0">\((1)\)</span> to have a unique strong (and continuous) solution. We will not bother with the technicalities here, but one can refer to, e.g., chapter 8 of [these excellent lecture notes by Djalil Chafaï](https://djalil.chafai.net/docs/M2/m2-stochastic-calculus-course-2020-2021.pdf) for details on what such conditions would typically look like.  

In the case which was (and still is) of interest to me and many people, the coefficients can respectively be taken as <span class="math inline" markdown="0">\(b(t,X_t)\equiv -\nabla V(X_t)\)</span> and <span class="math inline" markdown="0">\(\sigma(t,X_t)\equiv \sqrt{2\varepsilon} I_{d\times d}\)</span>, where <span class="math inline" markdown="0">\(\varepsilon&gt;0\)</span> is a <em>noise parameter</em>, and <span class="math inline" markdown="0">\(V:\mathbb R^d\to\mathbb R\)</span> is a <em>potential field</em>. The rough interpretation behind [this model](https://en.wikipedia.org/wiki/Langevin_dynamics#Overdamped_Langevin_dynamics), originating from statistical physics, is that it represents the dynamics of a particle <span class="math inline" markdown="0">\(X_t\)</span> evolving inside a potential field described by <span class="math inline" markdown="0">\(V\)</span>, while being excited by a random noise of intensity <span class="math inline" markdown="0">\(\varepsilon\)</span>. In the absence of noise, the deterministic motion would converge towards a local minimum of <span class="math inline" markdown="0">\(V\)</span>, but the interactions modeled by the random noise perturb this gradient flow and may lead the particle away from these local minima. This model actually turns out to be useful in a wide range of applications, such as for instance molecular dynamics <sup id="fnref-md"><a href="#fn-md">1</a></sup><sup id="fnref-lelievre_acta"><a href="#fn-lelievre_acta">2</a></sup>, computational chemistry<sup id="fnref-lelievre_book"><a href="#fn-lelievre_book">3</a></sup>, neurobiology<sup id="fnref-tns"><a href="#fn-tns">4</a></sup>, and most notably machine learning, where these dynamics can describe the behaviour of stochastic gradient descent under certain scaling limits<sup id="fnref-yeewhye"><a href="#fn-yeewhye">5</a></sup><sup id="fnref-qianxiaoli"><a href="#fn-qianxiaoli">6</a></sup>, and can even be used to sample from distributions of interest<sup id="fnref-barbour"><a href="#fn-barbour">7</a></sup>.  

In the sampling-focused literature, it is common (and pretty much needed) to require <span class="math inline" markdown="0">\(V\)</span> to be a *confining* and *non-explosive* potential, which guarantees existence and uniqueness of a stationary distribution to which the law of <span class="math inline" markdown="0">\(X_t\)</span> converges in total variation<sup id="fnref-tweedie"><a href="#fn-tweedie">8</a></sup>. That stationary distribution is known as the **Gibbs law** and takes the form <span class="math inline" markdown="0">\(\pi\propto e^{-V(x)/\varepsilon}\)</span>. For reasons that I will briefly outline below, these extra conditions won't be necessary for us, and simply assuming that <span class="math inline" markdown="0">\(V\)</span> is sufficiently smooth will be enough for the rest of the current discussion.

## First and mean exit times

Define, for <span class="math inline" markdown="0">\(x \in \Omega\)</span>, the <em>first escape time starting from <span class="math inline" markdown="0">\(x\)</span></em> as the (<span class="math inline" markdown="0">\(x\)</span>-dependent) random variable



<div class="math display" markdown="0">
\[

T(x):= \inf \{t\ge 0: X_t^x\in\Omega^c\} = \inf \{t\ge 0: X_t^x\in\partial\Omega\},

\]
</div>



where <span class="math inline" markdown="0">\((X_t^x)_{t\ge0}\)</span> denotes a solution of <span class="math inline" markdown="0">\((1)\)</span> with initial condition <span class="math inline" markdown="0">\(X_0=x\)</span>. The smoothness of <span class="math inline" markdown="0">\(\partial\Omega\)</span> together with the continuity of <span class="math inline" markdown="0">\((X_t)_{t\ge0}\)</span> ensure that the rightmost equality holds. Computing, for all <span class="math inline" markdown="0">\(x\)</span>, a good approximation of the distribution of <span class="math inline" markdown="0">\(T(x)\)</span>, or at least its moments, is quite an important problem appearing regularly in the applications listed earlier. This is the reason why I've spent a non-negligible amount of my PhD working on computing the so-called <em>mean escape time</em>, which, as the name suggests, is defined as... the mean of the first escape time (duh). More precisely, for all <span class="math inline" markdown="0">\(x\in \Omega\)</span>, we denote by <span class="math inline" markdown="0">\(\tau(x)\)</span> and call mean escape time (MET) the quantity

<div class="math display" markdown="0">
\[

\mathbb E[T(x)] \equiv \mathbb E\left[\inf \{t\ge 0: X_t^x\in\Omega^c\}\right] =: \tau(x) \tag{2}.

\]
</div>



Note that, as written, this definition is a bit careless. Indeed, it is entirely possible a priori that the event <span class="math inline" markdown="0">\(T(x)=\infty\)</span> happens on a positive measure set, and so the expectation <span class="math inline" markdown="0">\((2)\)</span> may be infinite. Thankfully, our smoothness assumptions on <span class="math inline" markdown="0">\(V\)</span>, together with <span class="math inline" markdown="0">\(\varepsilon&gt;0\)</span> and boundedness of <span class="math inline" markdown="0">\(\Omega\)</span>, prevent this pathological scenario from happening. Here's a sketch of why: let <span class="math inline" markdown="0">\(t_0&gt;0\)</span> be any positive timescale, put <span class="math inline" markdown="0">\(R=\operatorname{diam}(\Omega)\)</span>, and let <span class="math inline" markdown="0">\(C=t_0\sup_{\bar\Omega}\&#124;\nabla V\&#124;&lt;\infty\)</span>. On the event that <span class="math inline" markdown="0">\((X_r^x)_{t\le r\le t+t_0}\)</span> stays in <span class="math inline" markdown="0">\(\Omega\)</span>, <span class="math inline" markdown="0">\((1)\)</span> gives <span class="math inline" markdown="0">\(\sqrt{2\varepsilon}\&#124;W_{t+t_0}-W_t\&#124;\le R+C\)</span>. The probability of exiting somewhere between <span class="math inline" markdown="0">\(t\)</span> and <span class="math inline" markdown="0">\(t_0\)</span> is thus at least

<div class="math display" markdown="0">
\[
\mathbb P\!\left(\sqrt{2\varepsilon}\&#124;W_{t_0}\&#124;&gt;R+C\right)=:p\in(0,1).
\]
</div>

Here, we used some of the well-known properties of Brownian motion. The Markov property then gives for all <span class="math inline" markdown="0">\(x\in\Omega\)</span> and <span class="math inline" markdown="0">\(n\in\mathbb N\)</span> that <span class="math inline" markdown="0">\(\mathbb P(T(x)&gt; n t_0)\le (1-p)^n\)</span>, from which we deduce almost sure finiteness of <span class="math inline" markdown="0">\(T(x)\)</span> and existence of exponential moments of <span class="math inline" markdown="0">\(T(x)\)</span> (and hence of its mean). The MET <span class="math inline" markdown="0">\((2)\)</span> is therefore well defined, and we may attempt to compute it. Great! 



## Computing the mean exit time

### Computing the mean exit time: the naïve approach

Estimating <span class="math inline" markdown="0">\(\tau(x)\)</span> looks like a textbook job for a vanilla Monte Carlo implementation, to the point that I can already hear an attentive reader objecting <em>"but Nath, what's so special about this problem? Can't you just simulate a bunch of independent <span class="math inline" markdown="0">\(T_i(x)\)</span>'s by [Euler-Maruyama](https://en.wikipedia.org/wiki/Euler%E2%80%93Maruyama_method), take the average, and call it a day?"</em>. This is indeed what I first thought when looking at this problem as well, but I very quickly realized, after implementing this idea naively, that this approach was pretty much doomed to fail in many (most?) regimes of interest. Indeed, in a metastable well where Kramers' law applies, when the noise level <span class="math inline" markdown="0">\(\varepsilon\)</span> is vanishingly small, it is known that the MET <span class="math inline" markdown="0">\(\tau(x)\)</span> is approximately equal to <span class="math inline" markdown="0">\(Ce^{\Delta V/\varepsilon}\)</span> for starting points <span class="math inline" markdown="0">\(x\)</span> in the well outside of a vanishingly thin *boundary layer* near <span class="math inline" markdown="0">\(\partial \Omega\)</span>, where <span class="math inline" markdown="0">\(\Delta V\)</span> denotes the height of the energy barrier (I refer the interested reader to this [beautiful review by Nils Berglund](https://arxiv.org/abs/1106.5799) for further details on this). Taking <span class="math inline" markdown="0">\(C=\Delta V=1\)</span> and <span class="math inline" markdown="0">\(\varepsilon = 0.01\)</span> for simplicity, this means that for "most" <span class="math inline" markdown="0">\(x\)</span>'s in <span class="math inline" markdown="0">\(\Omega\)</span>, a decent Euler-Maruyama estimation (say, timestep <span class="math inline" markdown="0">\(\Delta t = 0.1\)</span>) of a *single* exit from <span class="math inline" markdown="0">\(\Omega\)</span> started from <span class="math inline" markdown="0">\(x\)</span> would require "on average" about <span class="math inline" markdown="0">\(e^{100}/0.1\approx2.7\times10^{44}\)</span> iterations to simulate! [As we discussed before](https://n2pakbong.github.io/blog/2026/02/27/infinity/), this is a pretty *damn* large number. And since this is all sequential, parallelization and GPUs can't save us either. Clearly, we need a better way.



<figure style="max-width: 520px; margin: 1.5rem auto; text-align: center;">
  <img src="{{ '/assets/girsanov/brownian_vs_ou.gif' | relative_url }}" width="400" style="display: block; max-width: 100%; height: auto; margin: 0 auto;" alt="Animation of standard Brownian motion exiting the unit disk while a low-noise OU process remains near the origin">
  <figcaption style="font-size: 0.9em; line-height: 1.45; margin-top: 0.6rem;">Standard Brownian motion (teal) and an OU process (purple) with drift −x and noise intensity ε = 0.01, both started at the origin and driven by the same Brownian increments. The cross marks the Brownian exit, while the OU process continues on the same clock. As we can see, that poor boy is not escaping anytime soon!</figcaption>
</figure>

### Computing the mean exit time with a slightly less naïve approach: speed!

Okay, so vanilla Monte Carlo doesn't work because the dynamics take too long to escape the domain. So what can we do instead? Well, a very natural thing to try would be to *speed up* the dynamics: perform the change of variables <span class="math inline" markdown="0">\(t' = \alpha t\)</span> for sufficiently large <span class="math inline" markdown="0">\(\alpha&gt;0\)</span>, which would lead to a process <span class="math inline" markdown="0">\((X_t^\alpha)_{t\ge0}\)</span> solving the SDE

<div class="math display" markdown="0">
\[

dX_t^\alpha
=\alpha b(\alpha t,X_t^\alpha)\,dt
+\sqrt{\alpha}\,\sigma(\alpha t,X_t^\alpha)\,dW_t^\alpha,
\quad
W_t^\alpha:=\alpha^{-1/2}W_{\alpha t}.\tag{1'} 
\]
</div>

For the overdamped Langevin formulation, this equivalently reads:

<div class="math display" markdown="0">
\[


dX_t^\alpha
=-\alpha\nabla V(X_t^\alpha)\,dt
+\sqrt{2\alpha\varepsilon}\,dW_t^\alpha.
\tag{1''}

\]
</div>

It is easy to check that the "sped-up" SDE is well-posed without any need for additional assumptions. The resulting Euler-Maruyama scheme is easy to implement, and its step-size <span class="math inline" markdown="0">\(h&gt;0\)</span> implementation takes the following expression, with <span class="math inline" markdown="0">\(t_k=kh\)</span> and <span class="math inline" markdown="0">\(X_0^{\alpha,h}=x\)</span>:

<div class="math display" markdown="0">
\[
X_{k+1}^{\alpha,h}
=X_k^{\alpha,h}+\alpha b(\alpha t_k,X_k^{\alpha,h})h
+\sqrt{\alpha h}\,\sigma(\alpha t_k,X_k^{\alpha,h})\xi_{k+1},
\qquad \xi_{k+1}\stackrel{\mathrm{iid}}{\sim}N(0,I_d).
\]
</div>

Given any starting point <span class="math inline" markdown="0">\(x\)</span> in <span class="math inline" markdown="0">\(\Omega\)</span>, its Euler-Maruyama scheme repeated <span class="math inline" markdown="0">\(n\)</span> times could then give us <span class="math inline" markdown="0">\(n\)</span> i.i.d. (approximate) trajectories of <span class="math inline" markdown="0">\((X_t^\alpha)_{t\ge0}\)</span>, which we could simulate until exit from <span class="math inline" markdown="0">\(\Omega\)</span> (which, for <span class="math inline" markdown="0">\(\alpha\)</span> large enough, would take a reasonable time), yielding <span class="math inline" markdown="0">\(n\)</span> i.i.d. <span class="math inline" markdown="0">\(T_1^\alpha(x),\ldots,T_n^\alpha(x)\)</span> (computed as <span class="math inline" markdown="0">\(h\times \text{(steps before exit)}\)</span>) which approximate the law of <span class="math inline" markdown="0">\(T^\alpha(x):=T(x)/\alpha\)</span>. We can then average them to get the following "sped-up Monte Carlo" estimator of the mean escape time: 
<div class="math display" markdown="0">
\[

\hat{\tau}^{\alpha}(x) := \frac\alpha n\sum_{i=1}^n T_i^\alpha(x).

\]
</div>

So, we're good right? We've fixed the slow simulation time of the escape time, and we've got a nice estimator for <span class="math inline" markdown="0">\(\tau\)</span>, right? Well, of course not, otherwise I wouldn't be writing this blog post. It's in fact incredibly simple to see why: any escaping trajectory needs at least one step to exit <span class="math inline" markdown="0">\(\Omega\)</span>, which means that <span class="math inline" markdown="0">\(T_i^{\alpha}(x)\ge h\)</span> almost surely, which in turn gives the lower bound <span class="math inline" markdown="0">\(\mathbb E[\hat{\tau}^{\alpha}(x)] - \tau(x) \ge \alpha h - \tau(x)\)</span> on the bias, which of course blows up as <span class="math inline" markdown="0">\(\alpha\to\infty\)</span> whenever <span class="math inline" markdown="0">\(h\)</span> is fixed. As the variance is of course always non-negative, it follows from the good old [bias-variance decomposition](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff#Bias%E2%80%93variance_decomposition_of_mean_squared_error) that the [mean squared error](https://en.wikipedia.org/wiki/Mean_squared_error) <span class="math inline" markdown="0">\(MSE^\alpha\)</span> of <span class="math inline" markdown="0">\(\hat\tau^\alpha\)</span> satisfies <span class="math inline" markdown="0">\(MSE^\alpha\gtrsim(\alpha h)^2\)</span> as <span class="math inline" markdown="0">\(\alpha\to\infty\)</span> at fixed <span class="math inline" markdown="0">\(h\)</span>, which of course blows up terribly as <span class="math inline" markdown="0">\(\alpha\)</span> gets large. In fact, what we've learned from this is that the only way for the speed-up to be helpful is to decrease the Euler-Maruyama step-size by a factor at least as large as the speed up, which makes any "speed gain" completely worthless.

### A "smarter" way to modify the dynamics

So vanilla Monte Carlo doesn't work, and the naïve idea of speeding up the dynamics makes things even worse. What else can we try? Looking back at our failed previous attempt in further generality, what we were really trying to do is to *modify the dynamics* <span class="math inline" markdown="0">\((X_t)_{t\ge 0}\)</span> into "altered dynamics" <span class="math inline" markdown="0">\((\widetilde{X_t})_{t\ge0}\)</span> satisfying the SDE

<div class="math display" markdown="0">
\[
d\widetilde{X_t}=\widetilde b(t, \widetilde{X_t})\,dt+
\widetilde\sigma(t,\widetilde{X_t})\,dW_t, \tag{ALT} 
\]
</div>

such that: (a) the corresponding first exit time <span class="math inline" markdown="0">\(\widetilde T(x)=\inf\{t\geq0:(\widetilde{X_t}^x)\notin\Omega\}\)</span>, together with its mean, is substantially easier to simulate numerically, (b) there is a simple, numerically tractable relationship between our quantity of interest <span class="math inline" markdown="0">\(\mathbb E[T(x)]\)</span> and its altered version <span class="math inline" markdown="0">\(\mathbb E[\widetilde T(x)]\)</span>, quite likely depending on the whole altered dynamics <span class="math inline" markdown="0">\((\widetilde{X_t})_{t\ge0}\)</span> as well, (c) the resulting estimator is *good*, in the sense that it has *reasonably small* bias and variance (obviously allowing myself to be extremely loose with the meaning of *good* and *small* here). What we saw for the "accelerated dynamics" <span class="math inline" markdown="0">\((1')\)</span> is that those satisfy items (a) and (b), but unfortunately fail miserably on item (c).  

So, do such *good* altered dynamics exist? Conditional on the meaning of *good*, this is a difficult question in general, and, as far as my limited knowledge goes, too open-ended to have a fully satisfying answer<sup id="fnref-caveat"><a href="#fn-caveat">9</a></sup>. A satisfying and practically useful partial answer can nonetheless be given thanks to the beautiful *[Girsanov theorem](https://en.wikipedia.org/wiki/Girsanov_theorem)* which I now state:


> **Girsanov theorem [(Øksendal, Theorem 8.6.4)](https://link.springer.com/book/10.1007/978-3-642-14394-6).** Denote by <span class="math inline" markdown="0">\(\mathbb P\)</span> our original probability measure, and <span class="math inline" markdown="0">\((\mathcal F_t)_{t\ge0}\)</span> a filtration satisfying the usual conditions with respect to which <span class="math inline" markdown="0">\((W_t)_{t\ge0}\)</span> is Brownian. Let <span class="math inline" markdown="0">\(H&gt;0\)</span> be a **deterministic** time horizon. Let <span class="math inline" markdown="0">\((\eta_t)_{t\ge0}\)</span> be an <span class="math inline" markdown="0">\(\mathbb R^d\)</span>-valued progressively measurable process satisfying *Novikov's condition*:
>
> <div class="math display" markdown="0">
> \[
> \mathbb E_{\mathbb P}\!\left[
> \exp\!\left(\frac12\int_0^H&#124;\eta_r&#124;^2\,dr\right)
> \right]&lt;\infty.
> \]
> </div>
>
> For <span class="math inline" markdown="0">\(0\le t\le H\)</span>, define <span class="math inline" markdown="0">\( Z_t:=\exp\!\left( \int_0^t\eta_r\cdot dW_r  -\frac12\int_0^t&#124;\eta_r&#124;^2\,dr \right)\)</span>. Then, <span class="math inline" markdown="0">\((Z_t)_{0\le t\le H}\)</span> is a strictly positive, mean-one <span class="math inline" markdown="0">\(\mathbb P\)</span>-martingale. Furthermore, the formula
>
> <div class="math display" markdown="0">
> \[
> \mathbb Q_H(A)=\mathbb E_{\mathbb P}[Z_H\mathbf1_A],
> \quad A\in\mathcal F_H,
> \tag{3}
> \]
> </div>
>
> defines a probability measure on <span class="math inline" markdown="0">\(\mathcal F_H\)</span>, equivalent to <span class="math inline" markdown="0">\(\mathbb P\)</span> on that sigma-field. Finally, the process <span class="math inline" markdown="0">\((B_t)_{0\le t\le H}\)</span> defined by
>
> <div class="math display" markdown="0">
> \[
> B_t=W_t-\int_0^t\eta_r\,dr,\qquad 0\le t\le H,
> \tag{4}
> \]
> </div>
>
> is a standard Brownian motion under <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>.

The power of Girsanov's theorem, at least in the context of trying to compute the MET, is that it provides us a way to express expectations under <span class="math inline" markdown="0">\(\mathbb P\)</span> as expectations under <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>, with *a lot* of freedom in the choice of the measure <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>, through the weight <span class="math inline" markdown="0">\(Z_H\)</span>. It is indeed easily seen (by monotone convergence, say) that, for every nonnegative <span class="math inline" markdown="0">\(\mathcal F_H\)</span>-measurable random variable <span class="math inline" markdown="0">\(F\)</span>, we have
 
<div class="math display" markdown="0">
\[
\mathbb E_{\mathbb Q_H}[F]=\mathbb E_{\mathbb P}[Z_HF],\text{ and }
 \mathbb E_{\mathbb P}[F]=\mathbb E_{\mathbb Q_H}[Z_H^{-1}F].
\tag{5}
\]
</div>

For instance, if <span class="math inline" markdown="0">\(F\)</span> is the indicator of the event <span class="math inline" markdown="0">\(\{T(x)\le H\}\)</span>, we may use the second equality to compute the <span class="math inline" markdown="0">\(\mathbb P\)</span>-probability of that exit event through the (hopefully friendlier) <span class="math inline" markdown="0">\(\mathbb Q_H\)</span> measure. For the mean escape time, we will be directly integrating the exit time <span class="math inline" markdown="0">\(T(x)\)</span> itself on trajectories that have exited by the time horizon <span class="math inline" markdown="0">\(H\)</span> (and will then send <span class="math inline" markdown="0">\(H\to\infty\)</span>).

<figure style="max-width: 520px; margin: 1.5rem auto; text-align: center;">
  <img src="{{ '/assets/girsanov/Rabbit-or-duck-illusion-008.jpg' | relative_url }}" width="240" style="display: block; max-width: 100%; height: auto; margin: 0 auto;" alt="Duck–rabbit optical illusion">
  <figcaption style="font-size: 0.9em; line-height: 1.45; margin-top: 0.6rem;">Sometimes, all we need is a change of perspective!</figcaption>
</figure>

#### Constructing the altered dynamics

Okay, so now the main question becomes that of choosing a "good" altered measure <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>. We still haven't defined what "good" means of course, but remembering our primary concern of the dynamics being insanely slow to exit under <span class="math inline" markdown="0">\(\mathbb P\)</span>, we will first be looking for a measure <span class="math inline" markdown="0">\(\mathbb Q_H\)</span> which ensures faster dynamics, and worry about the rest later. 

To do that, let's go back to the theorem's statement: by Equation <span class="math inline" markdown="0">\((4)\)</span>, we have that <span class="math inline" markdown="0">\(dW_t=dB_t+\eta_t\,dt\)</span>. Consequently, under <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>, the same process <span class="math inline" markdown="0">\((X_t^x)_{0\le t\le H}\)</span> satisfies

<div class="math display" markdown="0">
\[
dX_t^x
=
\bigl[b(t,X_t^x)+\sigma(t,X_t^x)\eta_t\bigr]\,dt
+\sigma(t,X_t^x)\,dB_t.
\tag{6}
\]
</div>

We therefore see that the original SDE under <span class="math inline" markdown="0">\(\mathbb P\)</span> <span class="math inline" markdown="0">\((1)\)</span> becomes, under <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>, the same SDE with the drift <span class="math inline" markdown="0">\(b\)</span> replaced by <span class="math inline" markdown="0">\(b+\sigma\eta\)</span>, and the noise term essentially unchanged. For us, this means that we can generate trajectories under <span class="math inline" markdown="0">\(\mathbb Q_H\)</span> simply by simulating the original SDE with this altered drift, which is straightforward to do. Cool, so what we want now is to pick the altered drift term <span class="math inline" markdown="0">\(\widetilde b \equiv b+\sigma\eta\)</span> in a way that ensures fast escape. There are of course many choices of <span class="math inline" markdown="0">\(\widetilde b\)</span> that fit the bill, but the only "free variable" here is <span class="math inline" markdown="0">\(\eta\)</span>. So really, by Equation <span class="math inline" markdown="0">\((6)\)</span>, we're looking for a function <span class="math inline" markdown="0">\(u(t,y)\in\mathbb R^d\)</span> such that

<div class="math display" markdown="0">
\[
\widetilde b(t,y)=b(t,y)+\sigma(t,y)u(t,y).
\]
</div>

Once such a <span class="math inline" markdown="0">\(u\)</span> is found, we can then take <span class="math inline" markdown="0">\(\eta_t\equiv u(t,X_t^x)\)</span>. If <span class="math inline" markdown="0">\(\sigma(t,y)\)</span> has the courtesy to be invertible, we can start from any <span class="math inline" markdown="0">\(\widetilde b\)</span> we like and compute directly

<div class="math display" markdown="0">
\[
u(t,y)=\sigma(t,y)^{-1}\bigl(\widetilde b(t,y)-b(t,y)\bigr),
\]
</div>

provided the theorem's integrability conditions hold. For the overdamped Langevin coefficients <span class="math inline" markdown="0">\(b=-\nabla V\)</span> and <span class="math inline" markdown="0">\(\sigma=\sqrt{2\varepsilon}\,I_{d\times d}\)</span>, this becomes <span class="math inline" markdown="0">\( u(t,y)=(\sqrt{2\varepsilon})^{-1}(\widetilde b(t,y)+\nabla V(y))\)</span>. The potential's restoring force <span class="math inline" markdown="0">\(-\nabla V\)</span> is the reason why low-noise dynamics get stuck in the well for so long. Choosing <span class="math inline" markdown="0">\(\widetilde b\)</span> to weaken or cancel that force therefore gives us candidates for quicker exits. For example, setting <span class="math inline" markdown="0">\(\widetilde b=0\)</span> removes the force altogether and turns the dynamics into a Brownian motion with small noise intensity. One could of course imagine other choices, such as <span class="math inline" markdown="0">\(\widetilde b\)</span> pointing directly towards <span class="math inline" markdown="0">\(\partial\Omega\)</span>. For any such choice, Girsanov's theorem then provides a correction-weight for the change of drift and allows us to compute the expectation of interest under the faster dynamics.

#### Application to the mean exit time

Remember that our ultimate goal is to compute <span class="math inline" markdown="0">\(\mathbb E[T(x)]\)</span>, which depends only on the path up to its first exit from <span class="math inline" markdown="0">\(\Omega\)</span>. We can therefore "truncate" the extra drift term up to exits of <span class="math inline" markdown="0">\(\Omega\)</span>. Practically, this means we can choose <span class="math inline" markdown="0">\((\eta_t)_{t\ge0}\)</span> as

<div class="math display" markdown="0">
\[
\eta_t=u(t,X_t^x)\mathbf1_{\{t&lt;T(x)\}}.
\tag{7}
\]
</div>

To ensure that we can apply Girsanov's theorem, and furthermore extend the horizon <span class="math inline" markdown="0">\(H\)</span> to infinity, we impose some technical conditions on <span class="math inline" markdown="0">\(u\)</span>: we will assume that <span class="math inline" markdown="0">\(u\)</span> is Borel, and satisfies <span class="math inline" markdown="0">\(&#124;u(t,y)&#124;\le K\)</span> uniformly in <span class="math inline" markdown="0">\(t\)</span> for all <span class="math inline" markdown="0">\(y\in\Omega\)</span>. Lastly, we require the resulting altered SDE to have a unique strong solution up to exit, as we did for the original SDE <span class="math inline" markdown="0">\((1)\)</span>. Since <span class="math inline" markdown="0">\((X_t^x)_{t\ge0}\)</span> is continuous and adapted, <span class="math inline" markdown="0">\((\eta_t)_{t\ge0}\)</span> is progressively measurable. By our boundedness assumption, it follows that Novikov's condition is also satisfied for every deterministic <span class="math inline" markdown="0">\(H&gt;0\)</span>. Under the resulting measure <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>, the altered SDE <span class="math inline" markdown="0">\((6)\)</span> has drift <span class="math inline" markdown="0">\(\widetilde b\)</span> until exit.

Now, denote by <span class="math inline" markdown="0">\(\mathbb Q\)</span> the probability measure under which we generate a standard Brownian input <span class="math inline" markdown="0">\((B_t)_{t\ge0}\)</span>, and, starting from <span class="math inline" markdown="0">\(\widetilde X_0^x=x\)</span>, solve the SDE

<div class="math display" markdown="0">
\[
d\widetilde X_t^x
=
\widetilde b(t,\widetilde X_t^x)\,dt
+\sigma(t,\widetilde X_t^x)\,dB_t.
\tag{8}
\]
</div>

Finally, denote by <span class="math inline" markdown="0">\(\widetilde T(x)=\inf\{t\ge0:\widetilde X_t^x\notin\Omega\}\)</span> the first exit time starting from <span class="math inline" markdown="0">\(x\)</span> under these dynamics. By uniqueness of the strong solution, the joint law of <span class="math inline" markdown="0">\((\widetilde X_{t\wedge\widetilde T(x)}^x)_{0\le t\le H}\)</span> and <span class="math inline" markdown="0">\((B_t)_{0\le t\le H}\)</span> under <span class="math inline" markdown="0">\(\mathbb Q\)</span> agrees with that of <span class="math inline" markdown="0">\((X_{t\wedge T(x)}^x)_{0\le t\le H}\)</span> and <span class="math inline" markdown="0">\((B_t)_{0\le t\le H}\)</span> under <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>.

To use the expectation identity from Girsanov's theorem, we need to express <span class="math inline" markdown="0">\(Z_H^{-1}\)</span> in terms of the Brownian motion <span class="math inline" markdown="0">\((B_t)_{0\le t\le H}\)</span> under <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>. Before exit, we have <span class="math inline" markdown="0">\(dW_t=dB_t+u(t,X_t^x)\,dt\)</span>. Substituting this relation into the definition of <span class="math inline" markdown="0">\(Z_H\)</span> and using <span class="math inline" markdown="0">\(\eta_t=0\)</span> for <span class="math inline" markdown="0">\(t\ge T(x)\)</span> gives

<div class="math display" markdown="0">
\[
Z_H^{-1}
=
\exp\!\left(
-\int_0^{T(x)\wedge H}u(t,X_t^x)\cdot dB_t
-\frac12\int_0^{T(x)\wedge H}&#124;u(t,X_t^x)&#124;^2\,dt
\right).
\]
</div>

On a path that exits by time <span class="math inline" markdown="0">\(H\)</span>, both integrals end at <span class="math inline" markdown="0">\(T(x)\)</span>. Under <span class="math inline" markdown="0">\(\mathbb Q\)</span>, we simulate <span class="math inline" markdown="0">\((\widetilde X_t^x)_{t\ge0}\)</span> together with its driving Brownian motion <span class="math inline" markdown="0">\((B_t)_{t\ge0}\)</span>. These have the same joint law up to exit as <span class="math inline" markdown="0">\((X_t^x)_{t\ge0}\)</span> and <span class="math inline" markdown="0">\((B_t)_{t\ge0}\)</span> under <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>. We can therefore compute the weight on a simulated path that exits by <span class="math inline" markdown="0">\(H\)</span> as follows:

<div class="math display" markdown="0">
\[
Z_{\widetilde T(x)}^{-1}
=
\exp\!\left(
-\int_0^{\widetilde T(x)}
u(t,\widetilde X_t^x)\cdot dB_t
-\frac12\int_0^{\widetilde T(x)}
&#124;u(t,\widetilde X_t^x)&#124;^2\,dt
\right).
\tag{9}
\]
</div>

So we're now in pretty good shape and almost ready to apply the theorem to get a computationally tractable estimator of the mean exit time. There is just one little technical hurdle that remains: Girsanov's theorem is only stated here for deterministic time horizons <span class="math inline" markdown="0">\(H&gt;0\)</span>, but we've seen that an exit time can be arbitrarily large. To overcome this, denote by <span class="math inline" markdown="0">\(F_H:=T(x)\mathbf1_{\{T(x)\le H\}}\)</span> the "truncation" of <span class="math inline" markdown="0">\(T(x)\)</span> to trajectories that have exited by time <span class="math inline" markdown="0">\(H\)</span>. Clearly, <span class="math inline" markdown="0">\(F_H\)</span> is <span class="math inline" markdown="0">\(\mathcal F_H\)</span>-measurable and so the expectation identity <span class="math inline" markdown="0">\((5)\)</span> applies. Since <span class="math inline" markdown="0">\(\eta_t=0\)</span> after <span class="math inline" markdown="0">\(T(x)\)</span>, <span class="math inline" markdown="0">\(Z_H^{-1}=Z_{T(x)}^{-1}\)</span> on <span class="math inline" markdown="0">\(\{T(x)\le H\}\)</span>. The identity <span class="math inline" markdown="0">\((5)\)</span> therefore gives

<div class="math display" markdown="0">
\[
\mathbb E_{\mathbb P}\!\left[
T(x)\mathbf1_{\{T(x)\le H\}}
\right]
=
\mathbb E_{\mathbb Q}\!\left[
\widetilde T(x)Z_{\widetilde T(x)}^{-1}
\mathbf1_{\{\widetilde T(x)\le H\}}
\right].
\tag{10}
\]
</div>

In particular, we can now establish the following proposition, whose proof we defer to the appendix:

> **Proposition.** Suppose <span class="math inline" markdown="0">\(u\)</span> is bounded before exit, both the original and altered SDEs have unique strong solutions up to exit, both exit times are almost surely finite, and <span class="math inline" markdown="0">\(\mathbb E_{\mathbb P}[T(x)]&lt;\infty\)</span>. Then
>
> <div class="math display" markdown="0">
> \[
> \tau(x)=\mathbb E_{\mathbb P}[T(x)]
> =
> \mathbb E_{\mathbb Q}\!\left[
> \widetilde T(x)Z_{\widetilde T(x)}^{-1}
> \right].
> \tag{11}
> \]
> </div>

So, we've *finally* found the altered dynamics <span class="math inline" markdown="0">\((\mathrm{ALT})\)</span> we were looking for. Awesome! After <span class="math inline" markdown="0">\(n\)</span> independent Euler-Maruyama simulations of the altered dynamics run until exit, the resulting estimator of <span class="math inline" markdown="0">\(\tau(x)\)</span> is given by

<div class="math display" markdown="0">
\[
\widehat\tau_n(x)
:=
\frac1n\sum_{j=1}^n
\widetilde T^{(j)}(x)
\bigl(Z_{\widetilde T^{(j)}(x)}^{(j)}\bigr)^{-1}.
\tag{12}
\]
</div>


Hooray! Let's see how we can apply this to our... wait. There is something important that we've left out. How can we check how *good* the estimator is, and choose the *optimal* altered dynamics and corresponding drift <span class="math inline" markdown="0">\(\widetilde b\)</span>? Well, as we did earlier for the sped up dynamics, we can say that the estimator is *good* if its variance is *small*. Now the question of finding the *optimal control* <span class="math inline" markdown="0">\(u^\star\)</span> which minimizes the variance is a deep one that has been (and still is) the subject of much research. I will thus happily sweep that under the rug for today and direct the interested reader to [these excellent notes](https://alexxthiery.github.io/notes/doob_transforms/doob.html) by Alexandre Thiéry instead for a short introduction to that fascinating world. 


For the sake of illustration, let's still see how this looks like for the Ornstein-Uhlenbeck process I mentioned at the start: let <span class="math inline" markdown="0">\(V(y)=&#124;y&#124;^2/2\)</span> on the unit disk in <span class="math inline" markdown="0">\(\mathbb R^2\)</span>, fix the starting point <span class="math inline" markdown="0">\(x=0\)</span>, and set <span class="math inline" markdown="0">\(\varepsilon=0.08\)</span>. The corresponding mean exit time is equal to <span class="math inline" markdown="0">\(\tau(0)\approx51.14\)</span>, as we show in Appendix C. For the altered dynamics, we use the bounded control <span class="math inline" markdown="0">\(u\)</span> given by<sup id="fnref-disclaimer"><a href="#fn-disclaimer">10</a></sup>

<div class="math display" markdown="0">
\[
a(y):=\exp\!\left(\frac{&#124;y&#124;^2-1}{2\varepsilon}\right),
\text{ and } \
u(y):=-\sqrt{\frac{2}{\varepsilon}}\,\frac{a(y)}{2-a(y)}\,y.
\]
</div>

With these choices, the altered drift is given <span class="math inline" markdown="0">\(\widetilde b(y)=-\frac{2+a(y)}{2-a(y)}y\)</span>. Despite leading to *longer* escape trajectories, this choice of altered drift reduces the variability of the weighted exit times and results in a stronger estimator overall. In the below plot, we found that Girsanov reweighting reduces the sample standard error from 3.08 to 2.13. The Girsanov trajectories however use about 1.87 times as many Euler steps. Hence with everything taken into account, the variance for a given Euler-step budget is about 10% lower only. We could definitely do *a lot* better if we tried, but that will be for another day!

<figure style="max-width: 560px; margin: 1.5rem auto; text-align: center;">
  <img src="{{ '/assets/girsanov/ou_importance_sampling.png' | relative_url }}" width="520" style="display: block; max-width: 100%; height: auto; margin: 0 auto;" alt="Running mean exit-time estimates with narrower sampling bands under Girsanov weighting, compared with the exact mean">
  <figcaption style="font-size: 0.9em; line-height: 1.45; margin-top: 0.6rem;">Running averages of exit times under the original dynamics and weighted exit times under the altered dynamics, with 256 independent paths per method. Shading extends two sample standard errors around each average. The dashed line marks the exact continuous-time mean, 51.1371. Here ε = 0.08 and the Euler step-size is h = 0.00025.</figcaption>
</figure>


## References

- Alexandre Thiéry, [Girsanov and Importance Sampling](https://alexxthiery.github.io/notes/girsanov/girsanov.html) notes, which were a huge source of inspiration in writing this blog post.
- Djalil Chafaï, [About diffusions leaving invariant a given law](https://djalil.chafai.net/blog/2017/11/24/about-diffusions-leaving-invariant-a-given-law/) blog post

### Footnotes

1. <a id="fn-md"></a>Leimkuhler, Ben, and Charles Matthews. Molecular dynamics: with deterministic and stochastic numerical methods. Vol. 39. Cham: Springer, 2015. <a href="#fnref-md" aria-label="Back to footnote 1">↩</a>

2. <a id="fn-lelievre_acta"></a>Lelievre, Tony, and Gabriel Stoltz. "Partial differential equations and stochastic methods in molecular dynamics." Acta Numerica 25 (2016): 681. <a href="#fnref-lelievre_acta" aria-label="Back to footnote 2">↩</a>

3. <a id="fn-lelievre_book"></a>Lelievre, Tony, Mathias Rousset, and Gabriel Stoltz. Free energy computations. Imperial College Press, London, 2010. <a href="#fnref-lelievre_book" aria-label="Back to footnote 3">↩</a>

4. <a id="fn-tns"></a>Dayan, Peter, and Laurence F. Abbott. Theoretical neuroscience. MIT Press, 2005. <a href="#fnref-tns" aria-label="Back to footnote 4">↩</a>

5. <a id="fn-yeewhye"></a>Welling, Max, and Yee W. Teh. "Bayesian learning via stochastic gradient Langevin dynamics." Proceedings of the 28th international conference on machine learning (ICML-11). 2011. <a href="#fnref-yeewhye" aria-label="Back to footnote 5">↩</a>

6. <a id="fn-qianxiaoli"></a>Li, Qianxiao, Cheng Tai, and Weinan E. "Stochastic modified equations and dynamics of stochastic gradient algorithms i: Mathematical foundations." Journal of Machine Learning Research 20.40 (2019): 1-47. <a href="#fnref-qianxiaoli" aria-label="Back to footnote 6">↩</a>

7. <a id="fn-barbour"></a>Barbour, Andrew D. "Stein's method for diffusion approximations." Probability theory and related fields 84.3 (1990): 297-322. <a href="#fnref-barbour" aria-label="Back to footnote 7">↩</a>

8. <a id="fn-tweedie"></a>Roberts, Gareth O., and Richard L. Tweedie. "Exponential convergence of Langevin distributions and their discrete approximations." (1996): 341-363. <a href="#fnref-tweedie" aria-label="Back to footnote 8">↩</a>

9. <a id="fn-caveat"></a>I am in fact merely showcasing the depth of my ignorance, as I don't know the state-of-the-art regarding this question much further than what I present in this note. I also must point out that Girsanov's theorem is not *necessary* in any sense beyond the fact that it gives a concrete starting point to start defining *good* and thus answering the question. I must also of course point out that there are in fact a myriad of other ways one could go about computing the mean escape time that do not rely at all on Monte Carlo approximations, such as approaches based on singular perturbation theory (discussed in [Berglund's review](https://arxiv.org/abs/1106.5799)) or on PDE representation, as was done in [previous work](https://arxiv.org/abs/2607.19167) of [mine](https://arxiv.org/abs/2606.00643). <a href="#fnref-caveat" aria-label="Back to footnote 9">↩</a>



10. <a id="fn-disclaimer"></a>as the astute reader may have noticed, this drift does NOT push particles away from <span class="math inline" markdown="0">\(\Omega\)</span> faster, quite the opposite! This alternate control was found by trial and error to have decent variance reduction, despite *longer* escape trajectories, and thus resulting in a better estimator! Again, the choice of *optimal controls* is a delicate mathematical question that I may or may not revisit later on this blog. <a href="#fnref-disclaimer" aria-label="Back to footnote 10">↩</a>

## Appendix

### A. Proof of the proposition

Fix <span class="math inline" markdown="0">\(H&gt;0\)</span> and set <span class="math inline" markdown="0">\(F_H=T(x)\mathbf1_{\{T(x)\le H\}}\)</span>. Since <span class="math inline" markdown="0">\(F_H\)</span> is <span class="math inline" markdown="0">\(\mathcal F_H\)</span>-measurable, Girsanov's expectation identity gives

<div class="math display" markdown="0">
\[
\mathbb E_{\mathbb P}[F_H]
=\mathbb E_{\mathbb Q_H}[Z_H^{-1}F_H].
\]
</div>

Since <span class="math inline" markdown="0">\(\eta_t=0\)</span> after <span class="math inline" markdown="0">\(T(x)\)</span>, we have <span class="math inline" markdown="0">\(Z_H^{-1}=Z_{T(x)}^{-1}\)</span> whenever <span class="math inline" markdown="0">\(T(x)\le H\)</span>. Under <span class="math inline" markdown="0">\(\mathbb Q_H\)</span>, the process <span class="math inline" markdown="0">\((X_t^x)_{0\le t\le H}\)</span> satisfies the altered SDE up to exit, driven by the Brownian motion <span class="math inline" markdown="0">\((B_t)_{0\le t\le H}\)</span>. By uniqueness of the strong solution, its path up to exit and the Brownian motion have the same joint law as the simulated pair <span class="math inline" markdown="0">\((\widetilde X_t^x)_{0\le t\le H}\)</span> and <span class="math inline" markdown="0">\((B_t)_{0\le t\le H}\)</span> under <span class="math inline" markdown="0">\(\mathbb Q\)</span>. We can therefore use the simulated exit time and its weight to write

<div class="math display" markdown="0">
\[
\mathbb E_{\mathbb P}\!\left[T(x)\mathbf1_{\{T(x)\le H\}}\right]
=
\mathbb E_{\mathbb Q}\!\left[
\widetilde T(x)Z_{\widetilde T(x)}^{-1}\mathbf1_{\{\widetilde T(x)\le H\}}
\right].
\]
</div>

The boundedness of <span class="math inline" markdown="0">\(u\)</span> and the almost sure finiteness of <span class="math inline" markdown="0">\(\widetilde T(x)\)</span> ensure that this weight is well defined. As <span class="math inline" markdown="0">\(H\to\infty\)</span>, both indicators increase to one almost surely. Monotone convergence on each side gives

<div class="math display" markdown="0">
\[
\mathbb E_{\mathbb P}[T(x)]
=
\mathbb E_{\mathbb Q}\!\left[\widetilde T(x)Z_{\widetilde T(x)}^{-1}\right].
\]
</div>

This is the desired formula for <span class="math inline" markdown="0">\(\tau(x)\)</span>.

### B. Source code for the simulations and plots

The examples use NumPy and Matplotlib. The animation also requires Pillow.

#### Brownian motion and a low-noise OU process

```python
from pathlib import Path
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation, PillowWriter

epsilon, lam, h, duration, radius = 0.01, 1.0, 0.005, 5.0, 1.0
rng = np.random.default_rng(17)
t = np.arange(round(duration / h) + 1) * h
dW = np.sqrt(h) * rng.standard_normal((len(t) - 1, 2))
brownian = np.vstack((np.zeros(2), np.cumsum(dW, axis=0)))
ou = np.zeros_like(brownian)
for k in range(len(dW)):
    ou[k + 1] = ou[k] - lam * ou[k] * h + np.sqrt(2 * epsilon) * dW[k]

exit_index = np.flatnonzero(np.linalg.norm(brownian, axis=1) >= radius)[0]
fig, ax = plt.subplots(figsize=(4.2, 4.7))
fig.subplots_adjust(left=0.02, right=0.98, bottom=0.13, top=0.88)
ax.add_patch(plt.Circle((0, 0), radius, facecolor="#f3f6f7", edgecolor="#687780", ls="--"))
ax.set(xlim=(-1.12, 1.12), ylim=(-1.12, 1.12), aspect="equal")
ax.axis("off")
bm_line, = ax.plot([], [], color="#007f86", lw=1.6, label="Brownian motion")
ou_line, = ax.plot([], [], color="#9c4f96", lw=1.6, label="OU process")
bm_dot, = ax.plot([], [], "o", color="#007f86", ms=6)
ou_dot, = ax.plot([], [], "o", color="#9c4f96", ms=6)
clock = fig.text(0.5, 0.93, "", ha="center", va="center", fontsize=10)
fig.legend(handles=[bm_line, ou_line], loc="lower center", ncol=2, frameon=False, fontsize=9)

def update(k):
    j = min(k, exit_index)  # Stop Brownian motion at its first grid exit.
    bm_line.set_data(brownian[:j + 1, 0], brownian[:j + 1, 1])
    ou_line.set_data(ou[:k + 1, 0], ou[:k + 1, 1])
    bm_dot.set_data([brownian[j, 0]], [brownian[j, 1]])
    bm_dot.set_marker("X" if k >= exit_index else "o")
    ou_dot.set_data([ou[k, 0]], [ou[k, 1]])
    clock.set_text(f"t = {t[k]:.2f}")

animation = FuncAnimation(fig, update, frames=np.arange(0, len(t), 5), interval=50)
out = Path(__file__).resolve().parents[1] / "assets" / "girsanov"
out.mkdir(parents=True, exist_ok=True)
animation.save(out / "brownian_vs_ou.gif", writer=PillowWriter(fps=20), dpi=100)
print(f"Brownian exit: {t[exit_index]:.3f}; OU maximum radius: {np.linalg.norm(ou, axis=1).max():.3f}")
plt.close(fig)
```

#### Mean exit time with an altered OU drift

```python
from pathlib import Path
import numpy as np
import matplotlib.pyplot as plt

n, h, epsilon = 256, 0.00025, 0.08

def simulate(control, seed):
    rng = np.random.default_rng(seed)
    x, log_weight, steps = np.zeros((n, 2)), np.zeros(n), np.zeros(n, dtype=int)
    alive, block = np.arange(n), 80
    power, offset = (1 - h) ** np.arange(1, block + 1), np.arange(block)
    while len(alive):
        a = np.exp((np.sum(x[alive] ** 2, axis=1) - 1) / (2 * epsilon))
        u = -np.sqrt(2 / epsilon) * (a / (2 - a))[:, None] * x[alive] if control else np.zeros((len(alive), 2))
        xi = rng.standard_normal((len(alive), block, 2))
        path = power[None, :, None] * (x[alive, None, :] + np.sqrt(2 * epsilon * h) * np.cumsum(xi / power[None, :, None], axis=1))
        path += np.sqrt(2 * epsilon) * u[:, None, :] * (1 - power[None, :, None])
        outside = np.sum(path ** 2, axis=2) >= 1
        exited = outside.any(axis=1)
        length = np.where(exited, outside.argmax(axis=1) + 1, block)
        increment = -np.sqrt(h) * np.sum(u[:, None, :] * xi, axis=2) - h / 2 * np.sum(u ** 2, axis=1)[:, None]
        log_weight[alive] += np.sum(np.where(offset[None, :] < length[:, None], increment, 0), axis=1)
        steps[alive] += length
        x[alive] = path[np.arange(len(alive)), length - 1]
        alive = alive[~exited]
    return h * steps * np.exp(log_weight), steps

def draw(results):
    z, term, reference = 1 / (2 * epsilon), 1 / (2 * epsilon), 1 / (4 * epsilon)
    for k in range(2, 200):
        term *= z * (k - 1) / k ** 2
        reference += term / 2
        if term < 1e-14 * reference:
            break
    fig, ax = plt.subplots(figsize=(5.6, 3.8), layout="constrained")
    count = np.arange(1, n + 1)
    shown = np.arange(15, n)
    for (value, steps), label, color in zip(results, ["Original dynamics", "Altered dynamics + weights"], ["#007f86", "#9c4f96"]):
        mean = np.cumsum(value) / count
        variance = np.maximum((np.cumsum(value ** 2) - count * mean ** 2) / np.maximum(count - 1, 1), 0)
        error = 2 * np.sqrt(variance / count)
        ax.plot(count[shown], mean[shown], color=color, lw=1.7, label=label)
        ax.fill_between(count[shown], (mean - error)[shown], (mean + error)[shown], color=color, alpha=0.12)
    ax.axhline(reference, color="#3d4750", ls="--", lw=1.2, label=f"Exact MET = {reference:.4f}")
    ax.set(xlabel="Number of simulated trajectories", ylabel="Mean exit-time estimate", xlim=(0, n))
    ax.spines[["top", "right"]].set_visible(False)
    ax.grid(axis="y", alpha=0.18)
    ax.legend(frameon=False, fontsize=9)
    out = Path(__file__).resolve().parents[1] / "assets" / "girsanov"
    out.mkdir(parents=True, exist_ok=True)
    for extension in ["png", "svg"]:
        fig.savefig(out / f"ou_importance_sampling.{extension}", dpi=180)
    plt.close(fig)

if __name__ == "__main__":
    results = [simulate(control, 20261028 + control) for control in [0, 1]]
    for name, (value, steps) in zip(["direct", "controlled"], results):
        np.savez(Path(__file__).with_name(f"ou_{name}_paths.npz"), value=value, steps=steps, n=n, h=h, epsilon=epsilon)
    draw(results)
```

### C. Reference mean for the OU example

We can check the mean exit time from the unit disk directly using Itô's formula. For the two-dimensional process <span class="math inline" markdown="0">\(dX_t=-X_t\,dt+\sqrt{2\varepsilon}\,dW_t\)</span>, define, for <span class="math inline" markdown="0">\(0\le r\le1\)</span>,

<div class="math display" markdown="0">
\[
f(r):=\frac12\int_{r^2/(2\varepsilon)}^{1/(2\varepsilon)}\frac{e^v-1}{v}\,dv.
\]
</div>

The integrand is understood to equal <span class="math inline" markdown="0">\(1\)</span> at <span class="math inline" markdown="0">\(v=0\)</span>. The function <span class="math inline" markdown="0">\(y\mapsto f(&#124;y&#124;)\)</span> is twice continuously differentiable on the closed disk and equals zero on its boundary. For <span class="math inline" markdown="0">\(r&gt;0\)</span>, differentiation gives

<div class="math display" markdown="0">
\[
f'(r)=-\frac{e^{r^2/(2\varepsilon)}-1}{r}
\]
</div>

Thus <span class="math inline" markdown="0">\(-rf'(r)=e^{r^2/(2\varepsilon)}-1\)</span>, while <span class="math inline" markdown="0">\(\varepsilon(f''(r)+f'(r)/r)=-e^{r^2/(2\varepsilon)}\)</span>. Their sum is <span class="math inline" markdown="0">\(-1\)</span>.

For the radial function <span class="math inline" markdown="0">\(y\mapsto f(&#124;y&#124;)\)</span>, this sum is the drift term in Itô's formula. The same holds at <span class="math inline" markdown="0">\(r=0\)</span> by continuity. Stopping the process at <span class="math inline" markdown="0">\(t\wedge T(0)\)</span> and taking expectations gives

<div class="math display" markdown="0">
\[
\mathbb E[f(&#124;X_{t\wedge T(0)}&#124;)]
=
f(0)-\mathbb E[t\wedge T(0)].
\]
</div>

The stochastic integral has mean zero: the gradient of <span class="math inline" markdown="0">\(f(&#124;y&#124;)\)</span> is bounded on the disk and the stopping time is at most <span class="math inline" markdown="0">\(t\)</span>. As <span class="math inline" markdown="0">\(t\to\infty\)</span>, the path reaches the boundary almost surely, where <span class="math inline" markdown="0">\(f=0\)</span>. Bounded convergence on the left and monotone convergence on the right give <span class="math inline" markdown="0">\(\tau(0)=f(0)\)</span>. Expanding the exponential in the integral and integrating term by term then yields

<div class="math display" markdown="0">
\[
\tau(0)
=\frac12\sum_{k=1}^{\infty}\frac{(2\varepsilon)^{-k}}{k\,k!}.
\]
</div>

For <span class="math inline" markdown="0">\(\varepsilon=0.08\)</span>, this gives <span class="math inline" markdown="0">\(\tau(0)\approx51.137134971\)</span>.
