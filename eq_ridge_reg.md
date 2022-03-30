# Thoughts Behind the Equation $(A^TA+\lambda I_n)^{-1}A^T=A^T(AA^T+\lambda I_m)^{-1}$

We often encounter one fact in the context of the [Ridge Regression](https://en.wikipedia.org/wiki/Tikhonov_regularization):

Let $A\in\mathbb{R}^{m\times n}$ be a matrix and $\lambda\in\mathbb{R}$ be a non-zero scalar. Then $$(A^TA+\lambda I_n)^{-1}A^T=A^T(AA^T+\lambda I_m)^{-1}\tag{1},$$ where $I_n$ and $I_m$ denote the identity matrices in $\mathbb{R}^n$ and $\mathbb{R}^m$, respectively.

Its proof is trivial, by showing that $$A^T(AA^T+\lambda I_m)=(A^TA+\lambda I_n)A^T.$$

And both $AA^T+\lambda I_m$ and $A^TA+\lambda I_n$ are invertible matrices (but, why?).

In this note, I would like to go into this equation another way, with the help of the Singular Value Decomposition (SVD).

---

## Facts

Let me first build up some facts that would be useful later.

Let $M\in\mathbb{R}^{m\times n}$ be any matrix, we write down the SVD of $M$ both in the matrix form and in the vector form: $$M=U\Sigma V^T = \sum_{j\le r} \sigma_j u_j v_j^T$$ where $\sigma_j>0$ for each $j\le r$, $\{u_j\}_{j\le m}$ an orthonormal basis in $\mathbb{R}^m$ (i.e., the columns of $U$), and $\{v_j\}_{j\le n}$ an orthonormal basis in $\mathbb{R}^n$ (i.e., the columns of $V$).

Then the transpose $M^T$ and the inverse $M^{-1}$ (which exists when $m=n=r$) are $$M^T=\sum_{j\le r} \sigma_j v_ju_j^T\tag{2}$$ and $$M^{-1}=\sum_{j\le r} \frac{1}{\sigma_j}v_ju_j^T\tag{3}$$ These results will be used in the next section.

---

## Dissect

Now, for arbitrary matrix $A\in\mathbb{R}^{m\times n}$, which is of rank $r$, and any but fixed positive scalar $\lambda\in\mathbb{R_{+}}$. We express the SVD of the matrix $A$ by: $$ A=\sum_{j\le r}\sigma_ju_jv_j^T$$.

First, We start by computing the term $(A^TA+\lambda I_n)^{-1}A^T$.

Using the observations built in the previous section, we found that $$A^TA=\sum_{j\le r} \sigma_j^2v_jv_j^T.$$ 

Hence $$A^TA+\lambda I_n = \sum_{j\le r}(\sigma_j^2+\lambda)v_jv_j^T+\sum_{j>r}\lambda v_jv_j^T.$$ 

Furthermore, we have $$(A^TA+\lambda I_n)^{-1} = \sum_{j\le r}{1\over \sigma_j^2+\lambda}v_jv_j^T+\sum_{j>r}{1\over \lambda}v_jv_j^T$$ 

and $$(A^TA+\lambda I_n)^{-1}A^T = \sum_{j\le r}{\sigma_j\over \sigma_j^2+\lambda}v_j u_j^T$$

It completes the computation of $(A^TA+\lambda I_n)^{-1}A^T$.

To derive $A^T(AA^T+\lambda I_m)^{-1}$, we use the same argument as above, and the results are written below: $$AA^T=\sum_{j\le r} \sigma_j^2u_ju_j^T.$$ $$AA^T+\lambda I_m = \sum_{j\le r}(\sigma_j^2+\lambda)u_ju_j^T+\sum_{j>r}\lambda u_ju_j^T.$$ $$(AA^T+\lambda I_m)^{-1} = \sum_{j\le r}{1\over \sigma_j^2+\lambda}u_ju_j^T+\sum_{j>r}{1\over \lambda}u_ju_j^T$$ $$A^T(AA^T+\lambda I_m)^{-1} = \sum_{j\le r}{\sigma_j\over \sigma_j^2+\lambda}v_j u_j^T.$$


---

## A Weapon to Reduce Computation Cost

In the Ridge Regression, the optimization problem we seek to solve is that: $$\text{minimize}\quad\|Aw-y\|_2^2+\lambda\|w\|_2^2,$$ Its optimal solution $\hat{w}$ is $(A^TA+\lambda I_n)^{-1}A^Ty$. We can directly derive this fact by setting the gradient of the objective function to be zero. 

However, when the number of columns of $A$ is much larger than that of rows of $A$ (e.g., the number of features is much larger than the number of samples), the time complexity of evaluating $(A^TA+\lambda I_n)^{-1}A^T$ is higher (for intance, in order to obtain $A^TA$, it requires about $mn^2$ operations). In that case, replacing the $\hat{w}$ with $\hat{w}=A^T(AA^T+\lambda I_m)^{-1}y$ in terms of the equation $(1)$ would give rise to the lower time complexity (it requires about $m^2n$ to get $AA^T$).

To validate the above argument, here is a simple experiment using Python.

```python=
import time

import numpy as np

if __name__ == "__main__":
  n_samples = 123
  n_features = 4567
  lambda_ = 89
  
  A = np.random.randn(n_samples, n_features)

  start = time.time()
  LHS = np.linalg.inv(A.T @ A + lambda_ * np.identity(n_features)) @ A.T
  end = time.time()
  print(f"Elapsed time of computing LHS: {end - start:.4f}s.")

  start = time.time()
  RHS = A.T @ np.linalg.inv(A @ A.T + lambda_ * np.identity(n_samples))
  end = time.time()
  print(f"Elapsed time of computing RHS: {end - start:.4f}s.")

  assert LHS.shape == (n_features, n_samples)
  assert RHS.shape == (n_features, n_samples)
  assert np.allclose(LHS, RHS)
```

My output is something as follows:

```bash=
Elapsed time of computing LHS: 3.1054s.
Elapsed time of computing RHS: 0.0052s.
```

Additionally, in the ridge regression, we can discover that the feature function is a linear combination of inner products through the expression $\hat{w}=A^T(AA^T+\lambda I_m)^{-1}y$, which introduces the motivation for the so-called kernel tricks.

After examing the equation $(1)$ through these interpretations, it more or less becomes a vivid object instead of the merely rigid mathematical notation.

###### tags: `Singular Value Decomposition` `Linear Algebra` `Tikhonov Regularization`