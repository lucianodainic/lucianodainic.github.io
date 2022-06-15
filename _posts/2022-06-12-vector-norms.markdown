---
layout: post
title: "Vector norms."
excerpt: "Listing of some vector norms formulas."
---

In mathematics, a norm is a function
$ || \cdot || : V \rightarrow \mathbb{R} $,
where $ V $ is a vector space and satisfies:

1. $$ ||u||>0 \hspace{100px} u \in V $$

2. $$ ||\alpha u|| = |\alpha| ||u|| \hspace{70px}  \alpha \in \mathbb{R}, \forall u \in V $$

3. $$ ||u + v|| \leq||u|| + ||v|| \hspace{43px}  u \in V, \forall v \in V $$

### Manhattan norm

$$||x-y||_1 = \sum_{i=1}^n{(x_i-y_i)}  \hspace{20px} x \in \mathbb{R}^n, y \in \mathbb{R}^n $$

### Euclidean norm

$$ ||x-y||_2 = \sqrt{\sum_{i=1}^n{x_i^2}} \hspace{20px} x \in \mathbb{R}^n, y \in \mathbb{R}^n $$

### Infinite norm

$$ ||x-y||_\infty = \max_{1\leq i \leq n} |x_i - y_i| \hspace{20px} x \in \mathbb{R}^n, y \in \mathbb{R}^n $$

### Cosine norm

$$ ||x-y||_{cos} = 1- \frac{x \cdot y }{||x||_2 * ||y||_2} \hspace{20px} x \in \mathbb{R}^n, y \in \mathbb{R}^n $$
