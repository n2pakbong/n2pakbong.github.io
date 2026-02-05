---
layout: section
title: "Research"
permalink: /research/
---

## Research interests

I am mostly interested in [Statistical Learning Theory](https://en.wikipedia.org/wiki/Statistical_learning_theory) and applying it to obtain mathematical guarantees (convergence rates, error bounds...) for Deep Learning-based solutions of PDEs arising from the study of rare events, such as computation of [equilibrium distributions](https://en.wikipedia.org/wiki/Fokker%E2%80%93Planck_equation), [first exit times](https://en.wikipedia.org/wiki/Narrow_escape_problem) or [committor functions](https://link.springer.com/article/10.1007/s10955-005-9003-9). For problems of this kind, whose solutions exhibit highly localized behavior, it is necessary to exploit the structural information in order to obtain tractable algorithms.  

Below is a brief overview of the kind of problems I've been keeping myself busy with over my PhD years.

---

## 1. Physics-informed neural networks for rare-event PDEs

One line of work studies **physics-informed neural networks (PINNs)** for solving PDEs that arise in the analysis of **rare events** and **metastable stochastic dynamics**. These include boundary value problems associated with exit times, transition probabilities, and related quantities. Such PDEs are often high-dimensional and may exhibit sharp boundary layers or stiffness, which makes them challenging for classical numerical schemes.

In this direction, I am interested not only in designing PINN architectures suited to these problems, but also in using tools from **statistical learning theory** to derive **error estimates and generalization guarantees**. The goal is to understand how the approximation error and sample complexity depend on the geometry of the metastable sets, the regularity of the solution, and the choice of network architecture and training procedure.

**Keywords:** physics-informed neural networks, rare events, metastability, high-dimensional PDEs, generalization bounds.

---

## 2. Operator learning for committor functions

A second direction focuses on **operator learning** approaches to approximating **committor functions**, which encode the probability that a stochastic system transitions from one metastable set to another before returning. Committor functions are central objects in transition path theory and rare-event analysis, but they are typically difficult to compute in complex, high-dimensional systems.

Here, I view the map from problem data (e.g., the potential landscape or system parameters) to the committor function as an **operator** to be learned from data. I am interested in developing operator-learning architectures that are tailored to this structure, and in establishing **theoretical guarantees**—for example, sample-complexity bounds and rates of convergence—that quantify when and why these methods can reliably approximate committors in realistic settings.

**Keywords:** operator learning, committor function, transition path theory, stochastic dynamics, sample complexity.

---

## 3. Fast rates under low-noise conditions for deep networks

A third line of work is more classical in flavor and concerns **rates of convergence for deep neural networks** under **low-noise conditions** in the sense of **Tsybakov’s margin assumption**. In supervised learning, such conditions are known to yield **fast rates** (faster than the standard \(n^{-1/2}\) rate) for certain estimators, but the extent to which these phenomena carry over to modern deep architectures is not yet fully understood.

In this project, I study how assumptions on the data distribution and the noise level can lead to improved statistical guarantees for deep networks. The aim is to connect the rich theory of margin / low-noise conditions with concrete learning algorithms used in practice, and to identify regimes where deep models can provably achieve **sharper excess-risk bounds** than those predicted by “worst-case” theory.

**Keywords:** statistical learning theory, Tsybakov low-noise condition, fast rates, deep neural networks, excess risk bounds.
