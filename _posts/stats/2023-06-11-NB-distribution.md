---
title:  "Negative Bionominal Distribution"
date:   2023-06-11 -0500
categories: statistics
tags: Statistics
author: Pei Tian
header:
    teaser: /assets/img/distributions.png
---
Basic Introduction to Negative Binomial Distribution.

## Introduction

$$ X\sim NB(r, p) $$

### Formula 1

Description:

When applying Bernoulli trails, the **total trail times** *k* within *r* times success conform to NB distribution. The success probability is *p*.

Possibility mass function, pmf:

$$ f(k;r,p) = P(X = k) = \binom{k-1}{r-1}p^r(1-p)^{(k-r)}, k=r, r+1, \cdots $$

Expectation:

$$ E(X) = \frac{r}{p} $$

Variation:

$$ Var(X) = \frac{r}{p^2} $$

### Formula 2

Description:

When applying Bernoulli trails, the **failure times** *k* conform to NB distribution while there are *r* success times. The success probability is *p*.

Possibility mass function, pmf:

$$ f(k;r,p) = P(X = k) = \binom{k+r-1}{r-1}p^r(1-p)^{k}, k=0, 1, \cdots $$

Expectation:

$$ E(X) = \frac{r(1-p)}{p} $$

Variation:

$$ Var(X) = \frac{r(1-p)}{p^2} $$

## Property Reasoning

### About negative

$$ \begin{equation*}\begin{split}\binom{y}{k} &= \frac{y(y-1)\cdots (y-(k-1))}{k!}\end{split}\end{equation*} $$

$$ \begin{equation*} \begin{split} \binom{r+k-1}{k} &= \frac{(r+k-1)!}{k!(r-1)!} \\ &= \frac{(r+k-1)(r+k-2)\cdots(r+1)r}{k!} \\ &= (-1)^k \cdot \frac{(-r-(k-1))(-r-(k-2))\cdots(-r-1)(-r)}{k!} \\ &= (-1)^k\cdot\binom{-r}{k} \end{split} \end{equation*} $$

### Expectation

Supplemental formula: (About Taylor Expansion)

$$

\begin{equation*}      \begin{split} (1-x)^{-r} &= \sum^{\infty}*{k=0}\frac{r(r+1)\cdots(r+k-1)}{k!}\cdot x^k\\ &= \sum^{\infty}*{k=0}\binom{k+r-1}{k}x^k\\         \end{split}\end{equation*} $$

$$ \begin{equation*}\begin{split}E(X) &= \sum^{\infty}*{k=0}k\binom{k+r-1}{k}p^r(1-p)^k\\ &= r(1-p)p^r\sum^{\infty}*{k=0}\binom{k+r-1}{k-1}(1-p)^{k-1} \\ &= r(1-p)p^r \cdot [1-(1-p)]^{-(r+1)} \\ &= r(1-p)p^r \cdot p^{-(r+1)} \\ &= \frac{r(1-p)}{p}\end{split}\end{equation*} $$

### Variance

$$ \begin{equation*}\begin{split}E(X^2) &= \sum^{\infty}*{k=0}k^2\binom{k+r-1}{k}p^r(1-p)^k \\ &= \sum^{\infty}*{k=0}k(k-1) \binom{k+r-1}{k}p^r(1-p)^k + \sum^{\infty}*{k=0}k \binom{k+r-1}{k}p^r(1-p)^k \\ &=  \sum^{\infty}*{k=0}r(r+1) \binom{k+r-1}{k-2}p^r(1-p)^k + E(X) \\ &= r(r+1)p^r(1-p)^2 \sum^{\infty}_{k=0}\binom{k+r-1}{k-2}(1-p)^{k-2} + \frac{r(1-p)}{p} \\ &= r(r+1)p^r(1-p)^2 \cdot (1-(1-p))^{-(r+2)} + \frac{r(1-p)}{p} \\  &= \frac{r(r+1)(1-p)^2}{p^2} + \frac{r(1-p)}{p} \end{split}\end{equation*} $$

$$ \begin{equation*}\begin{split}Var(X) &= E(X^2) -E(X)^2 \\ &= \frac{r}{p}(\frac{(r+1)(1-p)^2}{p}+1-p-\frac{r(1-p)^2}{p}) \\ &= \frac{r(1-p)}{p^2}\end{split}\end{equation*} $$

### Another form

mean & variance: while $X \sim NB(r, p)$

$$ p = \frac{\mu}{\sigma^2}~~~~~~~r = \frac{\mu^2}{\sigma^2 - \mu} $$

dispersion form: $\alpha = r^{-1}$ is dispersion parameter