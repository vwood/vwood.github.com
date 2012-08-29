---
layout: post
title: linear equations
tags: [compressed sensing, linear regression]
published: false
---

{{ page.title }}
================
<p class="meta">17 August 2012</p>

Least Squares gives an approximate solution to an over determined system.
Compressed Sensing gives an approximate solution to an under determined system.

These systems of equations are given in matrix form.

Ax = s

Both involve minimised residuals (errors).



Least Squares
-------------
Adjust the parameters of the model function to fit a dataset.

n points, (x_i, y_i), i = 1...n
x_i is independent, y_i is a dependent variable.

Least squares minimises the sum of squared residuals:

S = SUM^n_{i=1} r_i^2

Solving:


