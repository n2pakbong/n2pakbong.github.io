---
title: "Importance sampling, mean exit times, and Girsanov theorem"
date: 2026-10-28 00:00:00 +0800
---

<!-- Last updated: 2026-10-28 05:24 +08:00. -->



I have recently-ish gone through the exercise of writing my PhD dissertation (hooray!), which, to nobody's surprise, ended up quite a lot longer than I had initially envisioned (especially the acknowledgement section!!). As a consequence, a good chunk of the material I had initially planned to include therein ended up having to be left out. I therefore decided to write this post to record, a bit informally, one such thesis chunk which did not make the final cut, but which I nonetheless find important and interesting.

## The overdamped Langevin SDE

For $d\ge 2$, let $\Omega\subset\mathbb R^d$ be a bounded open connected set with smooth enough boundary, and consider a continuous-time $\mathbb R^d$-valued stochastic process <span markdown="0">$(X_t)_{t\ge 0}$</span> solving the stochastic differential equation

$$

dX_t = b(t, X_t)dt + \sigma(t,X_t)dW_t, \quad X_0= x \tag{1},

$$



where $x\in\Omega$, <span markdown="0">$(W_t)_{t\ge0}$</span> is a standard Brownian motion on $\mathbb R^d$, and the coefficients $b$ and $\sigma$, which are respectively $\mathbb R^d$ and $\mathbb R^{d\times d}$-valued, are assumed to be sufficiently regular for $(1)$ to have a unique strong (and continuous) solution. We will not bother with the technicalities here, but one can refer to, e.g., chapter 8 of [these excellent lecture notes by Djalil Chafaï](https://djalil.chafai.net/docs/M2/m2-stochastic-calculus-course-2020-2021.pdf) for details on what such conditions would typically look like.  

In the case which was (and still is) of interest to me and many people, the coefficients can respectively be taken as $b(t,X_t)\equiv -\nabla V(X_t)$ and $\sigma(t,X_t)\equiv \sqrt{2\varepsilon} I_{d\times d}$, where <span markdown="0">$\varepsilon&gt;0$</span> is a <em>noise parameter</em>, and $V:\mathbb R^d\to\mathbb R$ is a <em>potential field</em>. The rough interpretation behind [this model](https://en.wikipedia.org/wiki/Langevin_dynamics#Overdamped_Langevin_dynamics), originating from statistical physics, is that it represents the dynamics of a particle $X_t$ evolving inside a potential field described by $V$, while being excited by a random noise of intensity $\varepsilon$. In the absence of noise, the deterministic motion would converge towards a local minimum of $V$, but the interactions modeled by the random noise perturb this gradient flow and may lead the particle away from these local minima. This model actually turns out to be useful in a wide range of applications, such as for instance molecular dynamics <sup id="fnref-md"><a href="#fn-md">1</a></sup><sup id="fnref-lelievre_acta"><a href="#fn-lelievre_acta">2</a></sup>, computational chemistry<sup id="fnref-lelievre_book"><a href="#fn-lelievre_book">3</a></sup>, neurobiology<sup id="fnref-tns"><a href="#fn-tns">4</a></sup>, and most notably machine learning, where these dynamics can describe the behaviour of stochastic gradient descent under certain scaling limits<sup id="fnref-yeewhye"><a href="#fn-yeewhye">5</a></sup><sup id="fnref-qianxiaoli"><a href="#fn-qianxiaoli">6</a></sup>, and can even be used to sample from distributions of interest<sup id="fnref-barbour"><a href="#fn-barbour">7</a></sup>.  

In the sampling-focused literature, it is common (and pretty much needed) to require $V$ to be a *confining* and *non-explosive* potential, which guarantees existence and uniqueness of a stationary distribution to which the law of $X_t$ converges in total variation<sup id="fnref-tweedie"><a href="#fn-tweedie">8</a></sup>. That stationary distribution is known as the **Gibbs law** and takes the form $\pi\propto e^{-V(x)/\varepsilon}$. For reasons that I will briefly outline below, these extra conditions won't be necessary for us, and simply assuming that $V$ is sufficiently smooth will be enough for the rest of the current discussion.

## First and mean exit times

Define, for $x \in \Omega$, the <em>first escape time starting from $x$</em> as the ($x$-dependent) random variable



$$

T(x):= \inf \{t\ge 0: X_t^x\in\Omega^c\} = \inf \{t\ge 0: X_t^x\in\partial\Omega\},

$$



where <span markdown="0">$(X_t^x)_{t\ge0}$</span> denotes a solution of $(1)$ with initial condition $X_0=x$. The smoothness of $\partial\Omega$ together with the continuity of <span markdown="0">$(X_t)_{t\ge0}$</span> ensure that the rightmost equality holds. Computing, for all $x$, a good approximation of the distribution of $T(x)$, or at least its moments, is quite an important problem appearing regularly in the applications listed earlier. This is the reason why I've spent a non-negligible amount of my PhD working on computing the so-called <em>mean escape time</em>, which, as the name suggests, is defined as... the mean of the first escape time (duh). More precisely, for all $x\in \Omega$, we denote by $\tau(x)$ and call mean escape time (MET) the quantity

$$

\mathbb E[T(x)] \equiv \mathbb E\left[\inf \{t\ge 0: X_t^x\in\Omega^c\}\right] =: \tau(x) \tag{2}.

$$



Note that, as written, this definition is a bit careless. Indeed, it is entirely possible a priori that the event $T(x)=\infty$ happens on a positive measure set, and so the expectation $(2)$ may be infinite. Thankfully, our smoothness assumptions on $V$, together with <span markdown="0">$\varepsilon&gt;0$</span> and boundedness of $\Omega$, prevent this pathological scenario from happening. Here's a sketch of why: let <span markdown="0">$t_0&gt;0$</span> be any positive timescale, put $R=\operatorname{diam}(\Omega)$, and let <span markdown="0">$C=t_0\sup_{\bar\Omega}\&#124;\nabla V\&#124;&lt;\infty$</span>. On the event that <span markdown="0">$(X_r^x)_{t\le r\le t+t_0}$</span> stays in $\Omega$, $(1)$ gives <span markdown="0">$\sqrt{2\varepsilon}\&#124;W_{t+t_0}-W_t\&#124;\le R+C$</span>. The probability of exiting somewhere between $t$ and $t_0$ is thus at least

$$
\mathbb P\!\left(\sqrt{2\varepsilon}\|W_{t_0}\|>R+C\right)=:p\in(0,1).
$$

Here, we used some of the well-known properties of Brownian motion. The Markov property then gives for all $x\in\Omega$ and $n\in\mathbb N$ that <span markdown="0">$\mathbb P(T(x)&gt; n t_0)\le (1-p)^n$</span>, from which we deduce almost sure finiteness of $T(x)$ and existence of exponential moments of $T(x)$ (and hence of its mean). The MET $(2)$ is therefore well defined, and we may attempt to compute it. Great! 



## Computing the mean exit time

### Computing the mean exit time: the naïve approach

Estimating $\tau(x)$ looks like a textbook job for a vanilla Monte Carlo implementation, to the point that I can already hear an attentive reader objecting <em>"but Nath, what's so special about this problem? Can't you just simulate a bunch of independent $T_i(x)$'s by [Euler-Maruyama](https://en.wikipedia.org/wiki/Euler%E2%80%93Maruyama_method), take the average, and call it a day?"</em>. This is indeed what I first thought when looking at this problem as well, but I very quickly realized, after implementing this idea naively, that this approach was pretty much doomed to fail in many (most?) regimes of interest. Indeed, in a metastable well where Kramers' law applies, when the noise level $\varepsilon$ is vanishingly small, it is known that the MET $\tau(x)$ is approximately equal to $Ce^{\Delta V/\varepsilon}$ for starting points $x$ in the well outside of a vanishingly thin *boundary layer* near $\partial \Omega$, where $\Delta V$ denotes the height of the energy barrier (I refer the interested reader to this [beautiful review by Nils Berglund](https://arxiv.org/abs/1106.5799) for further details on this). Taking $C=\Delta V=1$ and $\varepsilon = 0.01$ for simplicity, this means that for "most" $x$'s in $\Omega$, a decent Euler-Maruyama estimation (say, timestep $\Delta t = 0.1$) of a *single* exit from $\Omega$ started from $x$ would require "on average" about $e^{100}/0.1\approx2.7\times10^{44}$ iterations to simulate! [As we discussed before](https://n2pakbong.github.io/blog/2026/02/27/infinity/), this is a pretty *damn* large number. And since this is all sequential, parallelization and GPUs can't save us either. Clearly, we need a better way.



<figure style="max-width: 520px; margin: 1.5rem auto; text-align: center;">
  <img src="/assets/girsanov/brownian_vs_ou.gif" width="400" style="display: block; max-width: 100%; height: auto; margin: 0 auto;" alt="Animation of standard Brownian motion exiting the unit disk while a low-noise OU process remains near the origin">
  <figcaption style="font-size: 0.9em; line-height: 1.45; margin-top: 0.6rem;">Standard Brownian motion (teal) and an OU process (purple) with drift −x and noise intensity ε = 0.01, both started at the origin and driven by the same Brownian increments. The cross marks the Brownian exit, while the OU process continues on the same clock. As we can see, that poor boy is not escaping anytime soon!</figcaption>
</figure>

### Computing the mean exit time with a slightly less naïve approach: speed!

Okay, so vanilla Monte Carlo doesn't work because the dynamics take too long to escape the domain. So what can we do instead? Well, a very natural thing to try would be to *speed up* the dynamics: perform the change of variables <span markdown="0">$t' = \alpha t$</span> for sufficiently large <span markdown="0">$\alpha&gt;0$</span>, which would lead to a process <span markdown="0">$(X_t^\alpha)_{t\ge0}$</span> solving the SDE

$$

dX_t^\alpha
=\alpha b(\alpha t,X_t^\alpha)\,dt
+\sqrt{\alpha}\,\sigma(\alpha t,X_t^\alpha)\,dW_t^\alpha,
\quad
W_t^\alpha:=\alpha^{-1/2}W_{\alpha t}.\tag{1'} 
$$

For the overdamped Langevin formulation, this equivalently reads:

$$


dX_t^\alpha
=-\alpha\nabla V(X_t^\alpha)\,dt
+\sqrt{2\alpha\varepsilon}\,dW_t^\alpha.
\tag{1''}

$$

It is easy to check that the "sped-up" SDE is well-posed without any need for additional assumptions. The resulting Euler-Maruyama scheme is easy to implement, and its step-size <span markdown="0">$h&gt;0$</span> implementation takes the following expression, with $t_k=kh$ and $X_0^{\alpha,h}=x$:

$$
X_{k+1}^{\alpha,h}
=X_k^{\alpha,h}+\alpha b(\alpha t_k,X_k^{\alpha,h})h
+\sqrt{\alpha h}\,\sigma(\alpha t_k,X_k^{\alpha,h})\xi_{k+1},
\qquad \xi_{k+1}\stackrel{\mathrm{iid}}{\sim}N(0,I_d).
$$

Given any starting point $x$ in $\Omega$, its Euler-Maruyama scheme repeated $n$ times could then give us $n$ i.i.d. (approximate) trajectories of <span markdown="0">$(X_t^\alpha)_{t\ge0}$</span>, which we could simulate until exit from $\Omega$ (which, for $\alpha$ large enough, would take a reasonable time), yielding $n$ i.i.d. $T_1^\alpha(x),\ldots,T_n^\alpha(x)$ (computed as $h\times \text{(steps before exit)}$) which approximate the law of $T^\alpha(x):=T(x)/\alpha$. We can then average them to get the following "sped-up Monte Carlo" estimator of the mean escape time: 

$$ 

\hat{\tau}^{\alpha}(x) := \frac\alpha n\sum_{i=1}^n T_i^\alpha(x).

$$

So, we're good right? We've fixed the slow simulation time of the escape time, and we've got a nice estimator for $\tau$, right? Well, of course not, otherwise I wouldn't be writing this blog post. It's in fact incredibly simple to see why: any escaping trajectory needs at least one step to exit $\Omega$, which means that $T_i^{\alpha}(x)\ge h$ almost surely, which in turn gives the lower bound <span markdown="0">$\mathbb E[\hat{\tau}^{\alpha}(x)] - \tau(x) \ge \alpha h - \tau(x)$</span> on the bias, which of course blows up as $\alpha\to\infty$ whenever $h$ is fixed. As the variance is of course always non-negative, it follows from the good old [bias-variance decomposition](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff#Bias%E2%80%93variance_decomposition_of_mean_squared_error) that the [mean squared error](https://en.wikipedia.org/wiki/Mean_squared_error) $MSE^\alpha$ of $\hat\tau^\alpha$ satisfies $MSE^\alpha\gtrsim(\alpha h)^2$ as $\alpha\to\infty$ at fixed $h$, which of course blows up terribly as $\alpha$ gets large. In fact, what we've learned from this is that the only way for the speed-up to be helpful is to decrease the Euler-Maruyama step-size by a factor at least as large as the speed up, which makes any "speed gain" completely worthless.

### A "smarter" way to modify the dynamics

So vanilla Monte Carlo doesn't work, and the naïve idea of speeding up the dynamics makes things even worse. What else can we try? Looking back at our failed previous attempt in further generality, what we were really trying to do is to *modify the dynamics* <span markdown="0">$(X_t)_{t\ge 0}$</span> into "altered dynamics" <span markdown="0">$(\widetilde{X_t})_{t\ge0}$</span> satisfying the SDE

$$ 
d\widetilde{X_t}=\widetilde b(t, \widetilde{X_t})\,dt+
\widetilde\sigma(t,\widetilde{X_t})\,dW_t, \tag{ALT} 
$$

such that: (a) the corresponding first exit time <span markdown="0">$\widetilde T(x)=\inf\{t\geq0:(\widetilde{X_t}^x)\notin\Omega\}$</span>, together with its mean, is substantially easier to simulate numerically, (b) there is a simple, numerically tractable relationship between our quantity of interest <span markdown="0">$\mathbb E[T(x)]$</span> and its altered version <span markdown="0">$\mathbb E[\widetilde T(x)]$</span>, quite likely depending on the whole altered dynamics <span markdown="0">$(\widetilde{X_t})_{t\ge0}$</span> as well, (c) the resulting estimator is *good*, in the sense that it has *reasonably small* bias and variance (obviously allowing myself to be extremely loose with the meaning of *good* and *small* here). What we saw for the "accelerated dynamics" <span markdown="0">$(1')$</span> is that those satisfy items (a) and (b), but unfortunately fail miserably on item (c).  

So, do such *good* altered dynamics exist? Conditional on the meaning of *good*, this is a difficult question in general, and, as far as my limited knowledge goes, too open-ended to have a fully satisfying answer<sup id="fnref-caveat"><a href="#fn-caveat">9</a></sup>. A satisfying and practically useful partial answer can nonetheless be given thanks to the beautiful *[Girsanov theorem](https://en.wikipedia.org/wiki/Girsanov_theorem)* which I now state:


> **Girsanov theorem [(Øksendal, Theorem 8.6.4)](https://link.springer.com/book/10.1007/978-3-642-14394-6).** Denote by $\mathbb P$ our original probability measure, and <span markdown="0">$(\mathcal F_t)_{t\ge0}$</span> a filtration satisfying the usual conditions with respect to which <span markdown="0">$(W_t)_{t\ge0}$</span> is Brownian. Let <span markdown="0">$H&gt;0$</span> be a **deterministic** time horizon. Let <span markdown="0">$(\eta_t)_{t\ge0}$</span> be an $\mathbb R^d$-valued progressively measurable process satisfying *Novikov's condition*:
>
> $$
> \mathbb E_{\mathbb P}\!\left[
> \exp\!\left(\frac12\int_0^H|\eta_r|^2\,dr\right)
> \right]<\infty.
> $$
>
> For $0\le t\le H$, define <span markdown="0">$ Z_t:=\exp\!\left( \int_0^t\eta_r\cdot dW_r  -\frac12\int_0^t&#124;\eta_r&#124;^2\,dr \right)$</span>. Then, <span markdown="0">$(Z_t)_{0\le t\le H}$</span> is a strictly positive, mean-one $\mathbb P$-martingale. Furthermore, the formula
>
> $$
> \mathbb Q_H(A)=\mathbb E_{\mathbb P}[Z_H\mathbf1_A],
> \quad A\in\mathcal F_H,
> \tag{3}
> $$
>
> defines a probability measure on $\mathcal F_H$, equivalent to $\mathbb P$ on that sigma-field. Finally, the process <span markdown="0">$(B_t)_{0\le t\le H}$</span> defined by
>
> $$
> B_t=W_t-\int_0^t\eta_r\,dr,\qquad 0\le t\le H,
> \tag{4}
> $$
>
> is a standard Brownian motion under $\mathbb Q_H$.

The power of Girsanov's theorem, at least in the context of trying to compute the MET, is that it provides us a way to express expectations under $\mathbb P$ as expectations under $\mathbb Q_H$, with *a lot* of freedom in the choice of the measure $\mathbb Q_H$, through the weight $Z_H$. It is indeed easily seen (by monotone convergence, say) that, for every nonnegative $\mathcal F_H$-measurable random variable $F$, we have
 
$$
\mathbb E_{\mathbb Q_H}[F]=\mathbb E_{\mathbb P}[Z_HF],\text{ and }
 \mathbb E_{\mathbb P}[F]=\mathbb E_{\mathbb Q_H}[Z_H^{-1}F].
\tag{5}
$$

For instance, if $F$ is the indicator of the event <span markdown="0">$\{T(x)\le H\}$</span>, we may use the second equality to compute the $\mathbb P$-probability of that exit event through the (hopefully friendlier) $\mathbb Q_H$ measure. For the mean escape time, we will be directly integrating the exit time $T(x)$ itself on trajectories that have exited by the time horizon $H$ (and will then send $H\to\infty$).

<figure style="max-width: 520px; margin: 1.5rem auto; text-align: center;">
  <img src="/assets/girsanov/Rabbit-or-duck-illusion-008.jpg" width="240" style="display: block; max-width: 100%; height: auto; margin: 0 auto;" alt="Duck–rabbit optical illusion">
  <figcaption style="font-size: 0.9em; line-height: 1.45; margin-top: 0.6rem;">Sometimes, all we need is a change of perspective!</figcaption>
</figure>

#### Constructing the altered dynamics

Okay, so now the main question becomes that of choosing a "good" altered measure $\mathbb Q_H$. We still haven't defined what "good" means of course, but remembering our primary concern of the dynamics being insanely slow to exit under $\mathbb P$, we will first be looking for a measure $\mathbb Q_H$ which ensures faster dynamics, and worry about the rest later. 

To do that, let's go back to the theorem's statement: by Equation $(4)$, we have that <span markdown="0">$dW_t=dB_t+\eta_t\,dt$</span>. Consequently, under $\mathbb Q_H$, the same process <span markdown="0">$(X_t^x)_{0\le t\le H}$</span> satisfies

$$
dX_t^x
=
\bigl[b(t,X_t^x)+\sigma(t,X_t^x)\eta_t\bigr]\,dt
+\sigma(t,X_t^x)\,dB_t.
\tag{6}
$$

We therefore see that the original SDE under $\mathbb P$ $(1)$ becomes, under $\mathbb Q_H$, the same SDE with the drift $b$ replaced by $b+\sigma\eta$, and the noise term essentially unchanged. For us, this means that we can generate trajectories under $\mathbb Q_H$ simply by simulating the original SDE with this altered drift, which is straightforward to do. Cool, so what we want now is to pick the altered drift term $\widetilde b \equiv b+\sigma\eta$ in a way that ensures fast escape. There are of course many choices of $\widetilde b$ that fit the bill, but the only "free variable" here is $\eta$. So really, by Equation $(6)$, we're looking for a function $u(t,y)\in\mathbb R^d$ such that

$$
\widetilde b(t,y)=b(t,y)+\sigma(t,y)u(t,y).
$$

Once such a $u$ is found, we can then take $\eta_t\equiv u(t,X_t^x)$. If $\sigma(t,y)$ has the courtesy to be invertible, we can start from any $\widetilde b$ we like and compute directly

$$
u(t,y)=\sigma(t,y)^{-1}\bigl(\widetilde b(t,y)-b(t,y)\bigr),
$$

provided the theorem's integrability conditions hold. For the overdamped Langevin coefficients $b=-\nabla V$ and <span markdown="0">$\sigma=\sqrt{2\varepsilon}\,I_{d\times d}$</span>, this becomes $ u(t,y)=(\sqrt{2\varepsilon})^{-1}(\widetilde b(t,y)+\nabla V(y))$. The potential's restoring force $-\nabla V$ is the reason why low-noise dynamics get stuck in the well for so long. Choosing $\widetilde b$ to weaken or cancel that force therefore gives us candidates for quicker exits. For example, setting $\widetilde b=0$ removes the force altogether and turns the dynamics into a Brownian motion with small noise intensity. One could of course imagine other choices, such as $\widetilde b$ pointing directly towards $\partial\Omega$. For any such choice, Girsanov's theorem then provides a correction-weight for the change of drift and allows us to compute the expectation of interest under the faster dynamics.

#### Application to the mean exit time

Remember that our ultimate goal is to compute <span markdown="0">$\mathbb E[T(x)]$</span>, which depends only on the path up to its first exit from $\Omega$. We can therefore "truncate" the extra drift term up to exits of $\Omega$. Practically, this means we can choose <span markdown="0">$(\eta_t)_{t\ge0}$</span> as

$$
\eta_t=u(t,X_t^x)\mathbf1_{\{t<T(x)\}}.
\tag{7}
$$

To ensure that we can apply Girsanov's theorem, and furthermore extend the horizon $H$ to infinity, we impose some technical conditions on $u$: we will assume that $u$ is Borel, and satisfies <span markdown="0">$&#124;u(t,y)&#124;\le K$</span> uniformly in $t$ for all $y\in\Omega$. Lastly, we require the resulting altered SDE to have a unique strong solution up to exit, as we did for the original SDE $(1)$. Since <span markdown="0">$(X_t^x)_{t\ge0}$</span> is continuous and adapted, <span markdown="0">$(\eta_t)_{t\ge0}$</span> is progressively measurable. By our boundedness assumption, it follows that Novikov's condition is also satisfied for every deterministic <span markdown="0">$H&gt;0$</span>. Under the resulting measure $\mathbb Q_H$, the altered SDE $(6)$ has drift $\widetilde b$ until exit.

Now, denote by $\mathbb Q$ the probability measure under which we generate a standard Brownian input <span markdown="0">$(B_t)_{t\ge0}$</span>, and, starting from $\widetilde X_0^x=x$, solve the SDE

$$
d\widetilde X_t^x
=
\widetilde b(t,\widetilde X_t^x)\,dt
+\sigma(t,\widetilde X_t^x)\,dB_t.
\tag{8}
$$

Finally, denote by <span markdown="0">$\widetilde T(x)=\inf\{t\ge0:\widetilde X_t^x\notin\Omega\}$</span> the first exit time starting from $x$ under these dynamics. By uniqueness of the strong solution, the joint law of <span markdown="0">$(\widetilde X_{t\wedge\widetilde T(x)}^x)_{0\le t\le H}$</span> and <span markdown="0">$(B_t)_{0\le t\le H}$</span> under $\mathbb Q$ agrees with that of <span markdown="0">$(X_{t\wedge T(x)}^x)_{0\le t\le H}$</span> and <span markdown="0">$(B_t)_{0\le t\le H}$</span> under $\mathbb Q_H$.

To use the expectation identity from Girsanov's theorem, we need to express $Z_H^{-1}$ in terms of the Brownian motion <span markdown="0">$(B_t)_{0\le t\le H}$</span> under $\mathbb Q_H$. Before exit, we have <span markdown="0">$dW_t=dB_t+u(t,X_t^x)\,dt$</span>. Substituting this relation into the definition of $Z_H$ and using $\eta_t=0$ for $t\ge T(x)$ gives

$$
Z_H^{-1}
=
\exp\!\left(
-\int_0^{T(x)\wedge H}u(t,X_t^x)\cdot dB_t
-\frac12\int_0^{T(x)\wedge H}|u(t,X_t^x)|^2\,dt
\right).
$$

On a path that exits by time $H$, both integrals end at $T(x)$. Under $\mathbb Q$, we simulate <span markdown="0">$(\widetilde X_t^x)_{t\ge0}$</span> together with its driving Brownian motion <span markdown="0">$(B_t)_{t\ge0}$</span>. These have the same joint law up to exit as <span markdown="0">$(X_t^x)_{t\ge0}$</span> and <span markdown="0">$(B_t)_{t\ge0}$</span> under $\mathbb Q_H$. We can therefore compute the weight on a simulated path that exits by $H$ as follows:

$$
Z_{\widetilde T(x)}^{-1}
=
\exp\!\left(
-\int_0^{\widetilde T(x)}
u(t,\widetilde X_t^x)\cdot dB_t
-\frac12\int_0^{\widetilde T(x)}
|u(t,\widetilde X_t^x)|^2\,dt
\right).
\tag{9}
$$

So we're now in pretty good shape and almost ready to apply the theorem to get a computationally tractable estimator of the mean exit time. There is just one little technical hurdle that remains: Girsanov's theorem is only stated here for deterministic time horizons <span markdown="0">$H&gt;0$</span>, but we've seen that an exit time can be arbitrarily large. To overcome this, denote by <span markdown="0">$F_H:=T(x)\mathbf1_{\{T(x)\le H\}}$</span> the "truncation" of $T(x)$ to trajectories that have exited by time $H$. Clearly, $F_H$ is $\mathcal F_H$-measurable and so the expectation identity $(5)$ applies. Since $\eta_t=0$ after $T(x)$, $Z_H^{-1}=Z_{T(x)}^{-1}$ on <span markdown="0">$\{T(x)\le H\}$</span>. The identity $(5)$ therefore gives

$$
\mathbb E_{\mathbb P}\!\left[
T(x)\mathbf1_{\{T(x)\le H\}}
\right]
=
\mathbb E_{\mathbb Q}\!\left[
\widetilde T(x)Z_{\widetilde T(x)}^{-1}
\mathbf1_{\{\widetilde T(x)\le H\}}
\right].
\tag{10}
$$

In particular, we can now establish the following proposition, whose proof we defer to the appendix:

> **Proposition.** Suppose $u$ is bounded before exit, both the original and altered SDEs have unique strong solutions up to exit, both exit times are almost surely finite, and <span markdown="0">$\mathbb E_{\mathbb P}[T(x)]&lt;\infty$</span>. Then
>
> $$
> \tau(x)=\mathbb E_{\mathbb P}[T(x)]
> =
> \mathbb E_{\mathbb Q}\!\left[
> \widetilde T(x)Z_{\widetilde T(x)}^{-1}
> \right].
> \tag{11}
> $$

So, we've *finally* found the altered dynamics $(\mathrm{ALT})$ we were looking for. Awesome! After $n$ independent Euler-Maruyama simulations of the altered dynamics run until exit, the resulting estimator of $\tau(x)$ is given by

$$
\widehat\tau_n(x)
:=
\frac1n\sum_{j=1}^n
\widetilde T^{(j)}(x)
\bigl(Z_{\widetilde T^{(j)}(x)}^{(j)}\bigr)^{-1}.
\tag{12}
$$


Hooray! Let's see how we can apply this to our... wait. There is something important that we've left out. How can we check how *good* the estimator is, and choose the *optimal* altered dynamics and corresponding drift $\widetilde b$? Well, as we did earlier for the sped up dynamics, we can say that the estimator is *good* if its variance is *small*. Now the question of finding the *optimal control* $u^\star$ which minimizes the variance is a deep one that has been (and still is) the subject of much research. I will thus happily sweep that under the rug for today and direct the interested reader to [these excellent notes](https://alexxthiery.github.io/notes/doob_transforms/doob.html) by Alexandre Thiéry instead for a short introduction to that fascinating world. 


For the sake of illustration, let's still see how this looks like for the Ornstein-Uhlenbeck process I mentioned at the start: let <span markdown="0">$V(y)=&#124;y&#124;^2/2$</span> on the unit disk in $\mathbb R^2$, fix the starting point $x=0$, and set $\varepsilon=0.08$. The corresponding mean exit time is equal to $\tau(0)\approx51.14$, as we show in Appendix C. For the altered dynamics, we use the bounded control $u$ given by<sup id="fnref-disclaimer"><a href="#fn-disclaimer">10</a></sup>

$$
a(y):=\exp\!\left(\frac{|y|^2-1}{2\varepsilon}\right),
\text{ and } \
u(y):=-\sqrt{\frac{2}{\varepsilon}}\,\frac{a(y)}{2-a(y)}\,y.
$$

With these choices, the altered drift is given $\widetilde b(y)=-\frac{2+a(y)}{2-a(y)}y$. Despite leading to *longer* escape trajectories, this choice of altered drift reduces the variability of the weighted exit times and results in a stronger estimator overall. In the below plot, we found that Girsanov reweighting reduces the sample standard error from 3.08 to 2.13. The Girsanov trajectories however use about 1.87 times as many Euler steps. Hence with everything taken into account, the variance for a given Euler-step budget is about 10% lower only. We could definitely do *a lot* better if we tried, but that will be for another day!

<figure style="max-width: 560px; margin: 1.5rem auto; text-align: center;">
  <img src="/assets/girsanov/ou_importance_sampling.png" width="520" style="display: block; max-width: 100%; height: auto; margin: 0 auto;" alt="Running mean exit-time estimates with narrower sampling bands under Girsanov weighting, compared with the exact mean">
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



10. <a id="fn-disclaimer"></a>as the astute reader may have noticed, this drift does NOT push particles away from $\Omega$ faster, quite the opposite! This alternate control was found by trial and error to have decent variance reduction, despite *longer* escape trajectories, and thus resulting in a better estimator! Again, the choice of *optimal controls* is a delicate mathematical question that I may or may not revisit later on this blog. <a href="#fnref-disclaimer" aria-label="Back to footnote 10">↩</a>

## Appendix

### A. Proof of the proposition

Fix <span markdown="0">$H&gt;0$</span> and set <span markdown="0">$F_H=T(x)\mathbf1_{\{T(x)\le H\}}$</span>. Since $F_H$ is $\mathcal F_H$-measurable, Girsanov's expectation identity gives

$$
\mathbb E_{\mathbb P}[F_H]
=\mathbb E_{\mathbb Q_H}[Z_H^{-1}F_H].
$$

Since $\eta_t=0$ after $T(x)$, we have $Z_H^{-1}=Z_{T(x)}^{-1}$ whenever $T(x)\le H$. Under $\mathbb Q_H$, the process <span markdown="0">$(X_t^x)_{0\le t\le H}$</span> satisfies the altered SDE up to exit, driven by the Brownian motion <span markdown="0">$(B_t)_{0\le t\le H}$</span>. By uniqueness of the strong solution, its path up to exit and the Brownian motion have the same joint law as the simulated pair <span markdown="0">$(\widetilde X_t^x)_{0\le t\le H}$</span> and <span markdown="0">$(B_t)_{0\le t\le H}$</span> under $\mathbb Q$. We can therefore use the simulated exit time and its weight to write

$$
\mathbb E_{\mathbb P}\!\left[T(x)\mathbf1_{\{T(x)\le H\}}\right]
=
\mathbb E_{\mathbb Q}\!\left[
\widetilde T(x)Z_{\widetilde T(x)}^{-1}\mathbf1_{\{\widetilde T(x)\le H\}}
\right].
$$

The boundedness of $u$ and the almost sure finiteness of $\widetilde T(x)$ ensure that this weight is well defined. As $H\to\infty$, both indicators increase to one almost surely. Monotone convergence on each side gives

$$
\mathbb E_{\mathbb P}[T(x)]
=
\mathbb E_{\mathbb Q}\!\left[\widetilde T(x)Z_{\widetilde T(x)}^{-1}\right].
$$

This is the desired formula for $\tau(x)$.

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

We can check the mean exit time from the unit disk directly using Itô's formula. For the two-dimensional process <span markdown="0">$dX_t=-X_t\,dt+\sqrt{2\varepsilon}\,dW_t$</span>, define, for $0\le r\le1$,

$$
f(r):=\frac12\int_{r^2/(2\varepsilon)}^{1/(2\varepsilon)}\frac{e^v-1}{v}\,dv.
$$

The integrand is understood to equal $1$ at $v=0$. The function <span markdown="0">$y\mapsto f(&#124;y&#124;)$</span> is twice continuously differentiable on the closed disk and equals zero on its boundary. For <span markdown="0">$r&gt;0$</span>, differentiation gives

$$
f'(r)=-\frac{e^{r^2/(2\varepsilon)}-1}{r}
$$

Thus <span markdown="0">$-rf'(r)=e^{r^2/(2\varepsilon)}-1$</span>, while <span markdown="0">$\varepsilon(f''(r)+f'(r)/r)=-e^{r^2/(2\varepsilon)}$</span>. Their sum is $-1$.

For the radial function <span markdown="0">$y\mapsto f(&#124;y&#124;)$</span>, this sum is the drift term in Itô's formula. The same holds at $r=0$ by continuity. Stopping the process at $t\wedge T(0)$ and taking expectations gives

$$
\mathbb E[f(|X_{t\wedge T(0)}|)]
=
f(0)-\mathbb E[t\wedge T(0)].
$$

The stochastic integral has mean zero: the gradient of <span markdown="0">$f(&#124;y&#124;)$</span> is bounded on the disk and the stopping time is at most $t$. As $t\to\infty$, the path reaches the boundary almost surely, where $f=0$. Bounded convergence on the left and monotone convergence on the right give $\tau(0)=f(0)$. Expanding the exponential in the integral and integrating term by term then yields

$$
\tau(0)
=\frac12\sum_{k=1}^{\infty}\frac{(2\varepsilon)^{-k}}{k\,k!}.
$$

For $\varepsilon=0.08$, this gives $\tau(0)\approx51.137134971$.
