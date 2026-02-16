---
layout: section
title: "Research"
permalink: /research/
---

## Research interests

I am mostly interested in [Statistical Learning Theory](https://en.wikipedia.org/wiki/Statistical_learning_theory) and applying it to obtain mathematical guarantees (convergence rates, error bounds...) for Deep Learning-based solutions of PDEs arising from the study of rare events, such as computation of [equilibrium distributions](https://en.wikipedia.org/wiki/Fokker%E2%80%93Planck_equation), [first exit times](https://en.wikipedia.org/wiki/Narrow_escape_problem) or [committor functions](https://link.springer.com/article/10.1007/s10955-005-9003-9). For problems of this kind, it is almost always necessary to exploit the extra mathematical structure in order to obtain tractable algorithms.  

Below is a brief overview of the kind of problems that have kept me busy throughout my PhD years.

---

## 1. Physics-informed neural networks for rare-event PDEs

One of my main research directions concerns [**physics-informed neural networks (PINNs)**](https://en.wikipedia.org/wiki/Physics-informed_neural_networks) for solving PDEs that arise in the analysis of **rare events** and **metastable stochastic dynamics**. These include boundary value problems associated with exit times, transition probabilities, and related quantities. Such PDEs are often high-dimensional and may exhibit sharp boundary layers or stiffness, which makes them challenging for classical numerical schemes.

In this direction, I am interested not only in designing PINN architectures and training schemes for these problems, but also in using tools from **statistical learning theory** to derive **error estimates and generalization guarantees**. The goal is to understand how the sample complexity and optimization difficulty depend on domain geometry, solution regularity, and the choice of network architecture and training procedure.

**Keywords:** physics-informed neural networks, rare events, high-dimensional PDEs, generalization bounds.

---

## 2. Operator learning for committor functions

In a closely related direction, I have been investigating [**neural operator learning**](https://en.wikipedia.org/wiki/Neural_operators) approaches to learning **committor functions**, which encode the probability that a stochastic system transitions from one metastable set to another. Committor functions are the central object of study in [transition path theory](https://link.springer.com/chapter/10.1007/3-540-35273-2_13) and rare-event analysis, but they are typically difficult (read: intractable) to compute in complex, high-dimensional systems.

The operator learning approach views the mapping from the (infinite-dimensional) parameter space (e.g., the potential landscape) to the associated committor function as an **operator** to be learned from relevant input-output pairs. I am interested in developing operator-learning architectures that are tailored to this structure, and in establishing **theoretical guarantees**—for example, sample-complexity bounds and rates of convergence—that quantify when and why these methods can reliably approximate committors in realistic settings.

**Keywords:** operator learning, committor function, transition path theory, sampling.

---

## 3. Fast rates under low-noise conditions for deep networks

In a much more classical flavor, I've also been interested in **rates of convergence for classification** with deep neural networks (DNNs) under (Tsybakov) **low-noise conditions**, which encode how "hard" a classification problem is. In supervised learning, such conditions are known to yield **fast rates** (faster than the standard $n^{-1/2}$ rate) for certain estimators, but the extent to which these phenomena carry over to modern deep neural networks is not yet fully understood.

I have thus been looking into how these low-noise conditions (perhaps coupled with further distributional assumptions) could lead to improved statistical guarantees for modern DNNs trained by [Empirical Risk Minimization (ERM)](https://en.wikipedia.org/wiki/Empirical_risk_minimization). The aim is to understand how these low-noise conditions intertwine with DNN architecture choices and training procedures to yield regimes where deep models can provably achieve **sharper excess-risk bounds** than those predicted by the classical "worst-case" theory.

**Keywords:** statistical learning theory, Tsybakov low-noise condition, fast rates, deep neural networks, excess risk bounds.
