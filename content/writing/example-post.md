---
title: "Placeholder Post: Writing Math on This Site"
date: 2026-01-20
description: Placeholder post showing how math is written on this site.
---

This is a placeholder post, and doubles as a reference for writing math. Inline math goes between single dollar signs, so `$e^{i\pi} + 1 = 0$` becomes $e^{i\pi} + 1 = 0$. Underscores and asterisks are safe inside it: $a_1 * b_2 + a_3 * b_4.$ Put punctuation that follows math inside the dollar signs, as that period is, so it can't wrap onto the next line alone.

Display math goes between double dollar signs:

$$
\hat{f}(\xi) = \int_{-\infty}^{\infty} f(x)\, e^{-2\pi i x \xi} \, dx
$$

Environments work too:

$$
\begin{aligned}
(a + b)^2 &= a^2 + 2ab + b^2 \\
(a - b)^2 &= a^2 - 2ab + b^2
\end{aligned}
$$

For a literal dollar sign, escape it with a backslash: the book costs \$20 and the course costs \$30.

Math in a code span or code block is left alone:

```python
print("$x$ stays as written")
```

A wide equation scrolls inside its own box on a small screen:

$$
\zeta(s) = \sum_{n=1}^{\infty} \frac{1}{n^s} = \prod_{p \text{ prime}} \frac{1}{1 - p^{-s}} = 1 + \frac{1}{2^s} + \frac{1}{3^s} + \frac{1}{4^s} + \frac{1}{5^s} + \frac{1}{6^s} + \frac{1}{7^s} + \frac{1}{8^s} + \cdots
$$
