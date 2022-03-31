# Sherman-Morrison Formula

The Sherman-Morrison Formula commonly appears in the Recursive Least Squares. Here I collect its proof and a way to recall its correct formula in the future.

The statement of the Sherman-Morrison Formula says that, when the square matrix $A^{-1}$ exists, then given column vectors $u$ and $v$,  $\big(A+uv^T\big)^{-1}$ exists if and only if $1+v^TA^{-1}u\ne 0$. In that case, $\big(A+uv^T\big)^{-1}$ has the explicit formula: $$(A+uv^T)^{-1} = A^{-1}-\frac{A^{-1}uv^TA^{-1}}{1+v^TA^{-1}u}\tag{1}$$

To obtain $(1)$, we often need to prepare a fact: $$\Big(I+uv^T\Big)^{-1} = I-\frac{uv^T}{1+v^Tu}\tag{2}$$

---

The proof of $(2)$ will be presented in this section.

For any $b$, consider the equation $$(I+uv^T)x=b.\tag{3}$$ The LHS of $(3)$ gives us that $$x+uv^Tx = b\tag{4}$$ Since $v^Tx$ is a scalar, we let it be $k$, then $(4)$ becomes $$x+ku = b.\tag{5}$$ 

Left-multiplying $v^T$ on both sides of $(5)$ leads to $$k + kv^Tu = v^Tb\tag{6}$$ Therefore, it follows that $$k=\frac{v^Tb}{1+v^Tu}.\tag{7}$$ From $(5)$ and $(7)$ we find that $$x=b-ku = b-\frac{(v^Tb)u}{1+v^Tu} = \Big(I-\frac{uv^T}{1+v^Tu}\Big)b$$ 

Since the vector $b$ is arbitrary, we have claimed that $$\Big(I+uv^T\Big)^{-1}=I-\frac{uv^T}{1+v^Tu}$$

Now, the proof of $(2)$ is completed. 

However, We tend to forget about the details of the proof in general. So here comes the question: how to recall this formula? I like to think it in the special case: when $u$ and $v$ are just of the forms: $$u=(0\ \cdots\  u_j\ \cdots 0)^T$$ and $$v=(0\ \cdots\  v_j\ \cdots 0)^T.$$

So the matrix $uv^T$ is simply a square matrix whose $j$-th entry is $u_jv_j$ and zero elsewhere. The matrix $\Big(I+uv^T\Big)^{-1}$ looks like: $$\Big(I+uv^T\Big)^{-1}=\pmatrix{1 & 0 & \cdots &&&\cdots& 0 \\ 0 &1 & 0 & \cdots &&&\vdots\\ 0& & \ddots \\ 0& & & \frac{1}{1+u_jv_j} \\ \vdots& & & & & \ddots & \vdots \\ 0&\cdots&&&&\cdots&1}\tag{8}$$

Once we have $(8)$, it would not be a hard thing to get the RHS of $(2)$ with only simple algebra.

This is how I usually recall this equation.

---

Using $(2)$, the Sherman-Morrison formula can be easily derived: $$\begin{align}\Big(A+uv^T\Big)^{-1} &= \Big(A\big(I+A^{-1}uv^T\big)\Big)^{-1}\\&=\Big(I+A^{-1}uv^T\Big)^{-1}A^{-1}\\&=\Big(I-\frac{A^{-1}uv^T}{1+v^TA^{-1}u}\Big)A^{-1}\\&=A^{-1}-\frac{A^{-1}uv^TA^{-1}}{1+v^TA^{-1}u}\end{align}$$

###### tags: `Linear Algebra` `Shermann-Morrison Formula` `Recursive Least Squares`