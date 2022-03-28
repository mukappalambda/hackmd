# Solving Minimum Norm Problem From the Perspective of SVD

In this note, I will present how to derive the solution to the Minimum Norm Problem from the perspective of the Singular Value Decomposition (SVD).

---

## Minimum Norm Problem

The statement of the Minimum Norm Problem is provided here.

Let $A\in\mathbb{R}^{m\times n}$ be a matrix, $b\in\mathbb{R}^m$ be a column vector. Suppose that the system $Ax=b$ of linear equations is consistent (means that $Ax=b$ has a solution), and we consider the following optimization problem $$\min \|x\|_2\quad\text{subject to }Ax=b.$$ This is called the **Minimum Norm Problem**.

---

## The Minimizer to the Minimum Norm Problem

It is well known from the theory of linear algebra that, the minimizer to the Minimum Norm Problem is $A^+b$, where $A^+$ denotes the pseudo-inverse of the matrix $A$.

The purpose of this note is to give a derivation of this minimizer in terms of the SVD.

---

## Derivation

Let $$A = U_r\Sigma V_r^T\tag{2}$$ be the rank $r$-truncated SVD of $A$ where we assume that $A$ is of rank $r$. It means that the matrix $U_r$ is $m\times r$ (the first $r$ columns of the matrix $U$), $\Sigma$ is $r\times r$, and $V_r$ is $n\times r$ (the first $r$ columns of $V$). 

Let $\{u_j\}_{0\le j \le m}$ and $\{v_j\}_{0\le j\le n}$ be the column vectors of $U$ and $V$, respectively. We also let $\{\sigma_j\}_{0\le j\le r}$ be the diagonal entries of $\Sigma$. Now we express $(2)$ into the following vector forms: $$A = \sum_{j\le r} \sigma_j u_jv_j^T$$ What does this expression has something to do with our problem? For any vector $x\in\mathbb{R}^n$, we want to express this $x$ in terms of the so-called "good" basis, and these $v_j$'s just play that role.

We write $x$ as follows: $$x=\sum_{j\le n}c_j v_j$$ Using the constraint $Ax = b$, we are going to investigate under which condition the minimum of $\|x\|_2$ can be achieved.

Since $b$ is in the range of $A$, we write $b$ as follows: $$b=\sum_{j\le r} b_j u_j$$ Then $Ax = b$ tells us that $$\Big(\sum_{j\le r} \sigma_j u_jv_j^T\Big)\Big(\sum_{j\le n}c_j v_j\Big)=\sum_{j\le r} b_j u_j,$$ Expanding the LHS of the above equation, we obtain $$\sum_{j\le r}\sigma_jc_ju_j = \sum_{j\le r} b_j u_j,$$ which simply says that the first $r$ coefficients $c_j$ of $x$ is complelely determined by the above equation, that is, $\sigma_j c_j = b_j$ for all $j\le r$. In order to achieve the minimum norm $\|x\|_2$ of $x$, we just force the remaining coefficients $c_j$ of $x$ equal to $0$ for $j > r$.

Now we have a better understanding of how the minimizer $\hat{x}$ should look like, we turn back to this guy $A^+b$.

We can write down the pseudo-inverse $A^+$ explicitly in terms of $u_j$ and $v_j$: $$A^+=V\Sigma^{-1}U^T$$ and hence $$A^+=\sum_{j\le r}\frac{1}{\sigma_j}v_ju_j^T$$ So $A^+b$ is $$\Big(\sum_{j\le r}\frac{1}{\sigma_j}v_ju_j^T\Big)\Big(\sum_{j\le m} b_j u_j\Big)$$ i.e. $$\sum_{j\le r}\frac{b_j}{\sigma_j}v_j,$$ which is nothing but the minimizer $\hat{x}$ we just deduced earlier.

Now, the derivation of the minimizer to the Minimum Norm Problem is explained.

###### tags: `Singular Value Decomposition`