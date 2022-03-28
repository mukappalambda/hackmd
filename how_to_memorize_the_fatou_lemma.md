# How to Memorize the Fatou's Lemma?

Given a sequence of non-negative measurable functions $\{f_n\}$, the statement of [Fatou's lemma](https://en.wikipedia.org/wiki/Fatou%27s_lemma) tells us that $$\int \liminf_{n \to \infty} f_n \ d\mu \leq \liminf_{n \to \infty} \left(\int f_n \ d\mu\right).\tag{1}$$


Sometimes it can be a bit tricky to figure out which inequality is correct: $\leq$ or $\geq$. Is there a more intuitive way to memorize this formula?

In this note, I will try to present such an approach.

---

First, we review the meaning of the notation $\liminf_{n\to\infty} f_n$.

## Lower Limit of a Sequence of Numbers

If $\{a_{n}\}$ is a bounded sequence in $\mathbb{R}$. For each positive integer $k$, define $b_k := \inf \{ a_k, a_{k+1}, a_{k+2}, \ldots \}$. According to the property of the greatest lower bound, we easily have the following: $$b_1\leq b_2\leq\cdots\leq b_k \leq b_{k+1} \leq b_{k+2} \leq \cdots.$$ The sequence $\{b_k\}$ is monotone increasing and bounded from above, therefore $\lim_{n\to\infty}b_k$ exists  and $\lim_{n\to\infty}b_k = \sup b_k$. We define $$\liminf_{n\to\infty} a_n:=\sup b_k$$ and $\liminf_{n\to\infty} a_n$ is said to be the **lower limit** of the sequence $\{a_n\}$.

---

## Lower Limit of a Sequence of Functions

If $\{f_n\}$ is a sequence of real-valued functions, we define $\liminf_{n\to\infty} f_n$ to be $$(\liminf_{n\to\infty} f_n) (x) := \liminf_{n\to\infty} f_n(x)$$ In other words, we create a new function $\liminf_{n\to\infty} f_n$, whose value at $x$ is defined to be the lower limit of the sequence $\{f_n(x)\}$. In that case, $\liminf_{n\to\infty} f_n$ is called the **lower limit** of the sequence of functions $\{f_n\}$.

---

## Method

After review the definition of lower limits of sequences (either of numbers or of functions), we back to address our primary problem: how to memorize the Fatou's lemma?

We consider one special situation: $$f_n: f_1, f_2, f_1, f_2, \ldots$$

where the sequence of functions $\{f_n\}$ is actually consisting of two non-negative functions $f_1$ and $f_2$ that appear alternately. Now what is lower limit $\liminf_{n\to\infty} f_n(x)$ of this sequence of functions? According to the definition of the lower limit of a sequence of functions, $$\liminf f_n(x) = \liminf_{n\to\infty} \{f_1(x),f_2(x),f_1(x),...\}=\min \{f_{1}(x), f_{2}(x)\}$$ Therefore the LHS of $(1)$ is $$\int \liminf_{n\to\infty} f_n \ d\mu = \int \min \{f_{1}, f_{2}\} \ d\mu.$$

Using the similar argument, we know that the RHS of $(1)$ is $$\liminf_{n\to\infty} \left(\int f_n \ d\mu\right) = \min \left\{ \int f_1 \ d\mu, \int f_2 \ d\mu\right\}.$$

From the geometric point of view:

- $\int \min \{f_1, f_2\} \ d\mu$ denotes the intersection of areas enclosed by $f_{1}$ and by $f_{2}$, respectively.
- $\min \{ \int f_1 \ d\mu, \int f_2 \ d\mu\}$ denotes the smaller one between areas enclosed by $f_1$ and by $f_2$.

Hence, it is naturally to deduce that $$\int \min \{f_{1}, f_{2}\} \ d\mu\leq\min \left\{ \int f_1 \ d\mu, \int f_2 \ d\mu\right\}.$$ 

And the formula $(1)$ holds in this special case (notice that both $f_1$ and $f_2$ are non-negative functions.)

Next time we bump into the Fatou's lemma, just let this naive example guide us.

###### tags: `Real Analysis` `Fatou's Lemma`
