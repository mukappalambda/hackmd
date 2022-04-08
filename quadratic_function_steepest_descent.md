# Quadratic Function With the Steepest Descent Method

Let $Q$ be an $n\times n$ real positive definite matrix and $b\in\mathbb{R}^n$. Let $f$ be the following quadratic function: $$f(x) \triangleq \frac{1}{2}x^TQx-b^Tx$$

We know a useful fact: the total differential $Df(x)$ is  $$Df(x)=x^TQ-b^T$$ In other words, $\nabla f(x)=Qx-b$. The quadratic function admits a global minimizer.

In this post, I am about to explain why the update rule of the Steepest Descent Method is the following: $$x^{(k+1)}=x^{(k)}-\alpha^{(k)}g^{(k)}$$ where $g^{(k)}=\nabla{f(x^{(k)})}$ and $$\alpha^{(k)}=\frac{{g^{(k)}}^Tg^{(k)}}{{g^{(k)}}^TQg^{(k)}}\tag{1}$$

---

## Explanation

By the definition of the Steepest Descent Method, the step size $\alpha^{(k)}$ is the scalar that minimizes $f(x^{(k)}-\alpha g^{(k)})$, that is to say, $$\alpha^{(k)}\triangleq \mathop{\arg\min}\limits_{\alpha\ge 0}f(x^{(k)}-\alpha g^{(k)})$$ If we let $\varphi_k(\alpha)\triangleq f(x^{(k)}-\alpha g^{(k)})$, then $$\varphi_k'(\alpha^{(k)})=0\tag{2}$$

From the chain rule, we obtain $$\begin{align}\varphi_k'(\alpha^{(k)})&=D\varphi(\alpha^{(k)})\\&=Df(x^{(k)}-\alpha^{(k)} g^{(k)})(-g^{(k)})\\&={\nabla f(x^{(k)}-\alpha^{(k)} g^{(k)})}^T(-g^{(k)})\\&={\Big(Q\big(x^{(k)}-\alpha^{(k)}g^{(k)}\big)-b\Big)}^T(-g^{(k)})\end{align}\tag{3}$$

Hence, $(2)$ and $(3)$ gives us $$\alpha^{(k)}{g^{(k)}}^TQg^{(k)}=\Big({x^{(k)}}^TQ-b^T\Big)g^{(k)}={g^{(k)}}^Tg^{(k)}$$ And $(1)$ is derived.

---

## Implementation Code

Here I write a Python code to demonstrate the above argument.

```python=
import numpy as np
from sklearn.datasets import make_spd_matrix

if __name__ == "__main__":
  n = 13
  Q = make_spd_matrix(n)
  b = np.random.randn(n)
  assert Q.shape == (n, n)
  assert b.shape == (n,)

  x = np.zeros_like(b)
  zero_vec = np.zeros_like(b)
  grad = Q @ x - b
  cnt = 1

  while not np.allclose(grad, zero_vec):
    grad = Q @ x - b
    alpha = (grad.T @ grad) / (grad.T @ Q @ grad)
    x = x - alpha * grad

    if cnt % 20 == 0:
      print(f"Iteration: {cnt}")

    cnt = cnt + 1
    
  x_true = np.linalg.pinv(Q) @ b
  assert np.allclose(x_true, x), "The steepest descent method does not converge yet."
  print(f"x_true: {x_true}")
  print(f"x: {x}")
```


###### tags: `Optimization Theory` `Gradient Descent` `Steepest Descent`