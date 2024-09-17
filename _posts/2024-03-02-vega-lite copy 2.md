---
layout: post
title: Minimum number of lines from non-collinear points 
date: 2024-09-16 16:42:00
description:
tags: combinatorics
categories: math
featured: false
---

Recently, I've been reading more about algebraic methods within combinatorics, and have been fascinated by the appearances of topics like linear algebra in the subject. In this blog post, I'll share one of my favorite problems I've come across lately that is really just inspired by some incredibly simple ideas.

<b>Problem 1.</b> What is the minimum number of lines $$n$$ non-collinear points form if all lines are drawn between points?

It is easy to see that we can achieve $$n$$ lines by simply leaving $$n - 1$$ collinear points on a line and the last point off the line. Indeed, this is the best case possible.

To demonstrate why, we can assign variables $$x$$ to each of the points, and define a system of equations as:

$$\sum_{j\text{ }: x_j \in l_i} x_j = 0.$$

That is, the sum of all the variables on line $$i$$ is equal to 0. Assuming that there is a placement of $$n$$ points that produces less than $$n$$ lines, we will have at most $$n - 1$$ equations and $$n$$ variables, meaning that there exists a nontrivial solution (this is the key idea of the proof). Since the square of non-zero numbers is strictly larger than 0, it would make sense to introduce squares, so let's square each equation and add them all up, like this:

$$\sum_{i = 1}^{m}\left(\sum_{j\text{ }: x_j \in l_i} x_j\right)^2 = \sum_{i = 1}^n a_i x_i^2 + 2\sum_{1 \leq i < j \leq n} a_{i, j} x_i x_j = 0$$

where $$m$$ is the number of lines, $$a_i$$ is the number of lines that $$x_i$$ shows up on, and $$a_{i, j}$$ is the number of lines $$x_i$$ and $$x_j$$ show up on. Since our points are not all on a single line, $$a_i > 1$$, and since all lines are drawn between points and two points define a line, we have that $$a_{i, j}$$ equals exactly 1. Thus,

$$0 = \sum_{i = 1}^n a_i x_i^2 + 2\sum_{1 \leq i < j \leq n} a_{i, j} x_i x_j = \sum_{i = 1}^n (a_i - 1) x_i^2 + \left(\sum_{i = 1}^n x_i \right)^2.$$

Because we have a non-trivial solution,

$$0 = \sum_{i = 1}^n (a_i - 1) x_i^2 + \left(\sum_{i = 1}^n x_i \right)^2 > 0$$

and we have a contradiction.