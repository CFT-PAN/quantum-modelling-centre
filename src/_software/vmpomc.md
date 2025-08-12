---
title: "VMPOMC.jl"
author: dhryniuk
header: 
    teaser: /assets/images/method_drawing.svg
github: "dhryniuk/VMPOMC"
layout: single
classes: wide
excerpt: "A Julia package for solving many-body Lindblad master equations of dissipative spin chains via variational Monte Carlo optimization of matrix product operators. Keywords: Open Quantum Systems, Variational Monte Carlo, Tensor Networks, Spins"
---

{% if page.author %}
  {% assign author_id = page.author %}
  {% assign author = site.data.authors[author_id] %}
  <p class="page__meta" style="margin-top: 0.5em; margin-bottom: 2.0em; line-height: 1.2; color: grey; font-size: 1.0em; font-style: italic;">
    By {{ author.name }}
  </p>
{% endif %}

A Julia package for solving many-body Lindblad master equations of dissipative spin chains via variational Monte Carlo optimization of matrix product operators.

Keywords: Open Quantum Systems, Variational Monte Carlo, Tensor Networks, Spins

# Description:

This package is a Julia implementation of a variational matrix product operator Monte Carlo method (VMPOMC). The method targets the non-equilibrium steady state, i.e. it finds the null-eigenvector or fixed point of the many-body Lindblad master equation, via variational Monte Carlo (VMC) optimization of a matrix product operator (MPO) trial state for the many-body density operator. It supports simple stochastic gradient descent (SGD) as well as more advanced stochastic reconfiguration (SR) or natural gradient minimization routines. It is suitable for simulating finite, periodic, translationally-invariant, interacting spin chains in contact with an external Markovian environment, including spins with long-range Ising interactions. 

# Installation:

Simply clone the repository via
```
git clone https://github.com/dhryniuk/VMPOMC.git
```

# Example Problems:

[Dissipative Ising Model]({{ site.baseurl }}/problems/dissipative_ising)

# Further reading:
[Hryniuk, D. A.; Szymańska, M. H. Tensor-network-based variational Monte Carlo approach to the non-equilibrium steady state of open quantum systems. Quantum 2024, 8, No.1475.](https://doi.org/10.22331/q-2024-09-17-1475)