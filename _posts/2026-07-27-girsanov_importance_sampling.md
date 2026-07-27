## Importance sampling on path space with Girsanov theorem



I have recently-ish gone through the exercise of writing my PhD dissertation (hooray), and to nobody's surprise, it got quite a lot longer than I had initially envisioned (especially the acknowledgement section!!), to the point that a good chunk of the material I had initially planned to include ended up having to be left out (in no small part due to <s>poor planning on my end</s> a lack of time as well). The purpose of this post is to record, a bit informally, one such thesis chunk which did not make the final cut, but which I nonetheless find important and interesting.



For $d\\ge 2$, let $\\Omega\\subset\\mathbb R^d$ be a bounded open connected set with smooth enough boundary, and consider a continuous-time $\\mathbb R^d$-valued valued stochastic process $(X\_t)\_{t\\ge 0}$ solving the stochastic differential equation

$$

dX\_t = b(t, X\_t)dt + \\sigma(t,X\_t)dW\_t, \\quad X\_0= x \\tag1,

$$



where $x\\in\\Omega$, $(W\_t)$ is a standard Brownian motion on $\\mathbb R^d$, and the coefficients $b$ and $\\sigma$, which are respectively $\\mathbb R^d$ and $\\mathbb R^{d\\times d}$-valued, are assumed to be sufficiently regular for $(1)$ to have a unique strong (and continuous) solution. We will not bother with the technicalities here, but one can refer to, e.g., \[C2020, Chapter 8], for details on what such conditions could look like. In the case which was (and still is) of interest to me and many people, the coefficients can respectively be taken as $b(t,X\_t)\\equiv -\\nabla V(X\_t)$ and $\\sigma(t,X\_t)\\equiv \\sqrt{2\\varepsilon} I\_{d\\times d}$, where $\\varepsilon>0$ is a <em>noise parameter</em>, and $V:\\mathbb R^d\\to\\mathbb R$ is a <em>potential field</em>, which for the purposes of this post, we may think of as taking the form $V(x)\\propto \\exp(-|x|^2)$. The rough interpretation behind \[this model](https://en.wikipedia.org/wiki/Langevin\_dynamics#Overdamped\_Langevin\_dynamics), originating from statistical physics, is that it represents the dynamics of a particle $X\_t$ evolving inside a potential field described by $V$, while being excited by a random noise of intensity $\\varepsilon$ (as you can probably tell, I am not a physicist). This model actually turns out to be useful in a wide range of domains beyond physics, such as for instance molecular dynamics, computational chemistry, neurobiology, and most notably machine learning, where these dynamics can describe the behaviour of stochastic gradient descent under certain scaling limits, and can even be used to sample from distributions of interest \[TODO: sources!!]. 



Define, for $x \\in \\Omega$, the <em>first escape time starting from $x$</em> as the ($x$-dependent) random variable



$$

T(x):= \\inf \\{t\\ge 0: X\_t^x\\in\\Omega^c\\} = \\inf \\{t\\ge 0: X\_t^x\\in\\partial\\Omega\\},

$$



where $X\_t^x$ denotes a solution of $(1)$ with initial condition $X\_0=x$. The smoothness of $\\partial\\Omega$ together with the continuity of $(X\_t)$ ensure that the righmost equality holds. Computing, for all $x$, a good approximation of the distribution of $T(x)$, or at least its moments, is quite an important problem. This is the reason I have spent a non-negligible amount of my PhD working on computing the so-called <em>mean escape time</em>, which as the name suggested is defined as... the mean of the first escape time (duh). More precisely, for all $x\\in \\Omega$, we denote $\\tau(x)$ and call mean escape time (MET) the quantity

$$

\\mathbb E\[T(x)] \\equiv \\mathbb E\\left\[\\inf \\{t\\ge 0: X\_t^x\\in\\Omega^c\\}\\right] =: \\tau(x) \\tag2.

$$



The expectation $(2)$ looks like a textbook job for a vanilla Monte Carlo implementation, to the point that I can already hear an attentive reader objecting <em>"but Nath, what's so special about this problem? Can't you just simulate a bunch of independent $T\_i$'s, take the average, and call it a day?"</em>. This is indeed what I first thought when looking at this problem as well, but I very quickly realized, after implementing this idea naively, that this approach was pretty much doomed to fail in many (most?) regimes of interest. Indeed, when the potential $V$ is \*confining\*, and the noise level $\\epsilon$ is vanishingly small, it is known that the MET $\\tau(x)$ is approximately equal to $Ce^{c/\\varepsilon^2}$ for all $x$ outside of a vanishingly thin \*boundary-layer\* near $\\partial\\Omega$ \[TODO: sources!!!]. Taking $C=c=1$ and $\\varepsilon = 0.01$ for simplicity, this means that for most $x$'s in $\\Omega$, a decent Euler-Maruyama estimation (say, timestep $\\Delta t = 0.1$s) of a \*single\* exit from $\\Omega$ started from $x$ would require about $\\approx 2.7\\times10^{44}$ iterations to simulate! And since this is all sequential, parallelization and GPUs can't save us either. Clearly, we need a better way.



\[TODO: plot/animation]



Okay, so vanilla Monte Carlo doesn't work because the dynamics take too long to escape the domain. So what can we do instead? Well, a very natural thing to try would be to \*speed up\* the dynamics: perform the change of variables $t' = \\alpha t$ for sufficiently large $\\alpha>0$, which would lead to a process $X\_t^\\alpha$ solving the SDE

$$

(1')

$$

or equivalently

$$

(1'')

$$

it is easy to check that this SDE is well-posed without any need for additional assumptions, and the resulting Euler-Maruyama scheme is easy to implement. So we can just run that, compute the average escape time over a sufficiently large amount of run, and use that as an unbiased estimator of the MET, can't we? Well, yes, we \*can\*. If we actually do it however, we will realize that there is a very serious issue with this: the resulting estimator has ridiculously large variance, so much so that we are in fact better off with running vanilla Monte Carlo (it is a good exercise to check this explicitly).



So vanilla Monte Carlo doesn't work, and speeding up the dynamics is even worse. What else can we try? Well, the idea of \*speeding up the dynamics\* was definitely not a bad one in spirit, it only had the very unfortunate drawback that it also deteriorated the variance of the resulting estimator beyond repair. What if we could indeed \*modify the dynamics\* in a way that made escapes happen in a reasonable amount of time, all the while yielding an estimator with reasonable variance? More precisely, assume we can efficiently simulate solutions of the (well-behaved) SDE

$$

dY\_t = rzr

$$

can we then use this process to compute $\\tau$? A positive answer is given by the celebrated \[Girsanov theorem](https://en.wikipedia.org/wiki/Girsanov\_theorem).



\[precise theorem statement]



\[image: sometimes it's all a matter of perspective!]



this means, if we take Y like this, we have the identity. note that \[diffusion coefficients needa be equal. future blog post material for sure]. This is nice, but this is not \*quite\* what we're looking for, as $\\tau$  blabla. Luckily for us, it is a martingale \[proof in appendix], so we can apply the \[optional stopping theorem](https://en.wikipedia.org/wiki/Optional\_stopping\_theorem) to deduce, as we had hoped that

$$ 

good\\ \\ news!

$$





Hooray! Let's see how we can apply this to our process of interest



\[code + plots]



not too bad, huh?



\[appendix: proof of martingale property plus girsanov optional stopping]

