---
title: Lagrangian Duality
date: 2025-10-23
mathjax: true
categories:
  - Math
---

We consider an optimization problem over domain $\mathcal{D}\neq\emptyset$ in standard form, i.e. $$\begin{align}
\text{minimize } f_{0}(x)\quad \text{s.t.} \quad f_{i}(x)&\leq 0 \\
h_{i}(x)&=0
\end{align}$$which we do *not* assume to be convex. We assume the optimal value to be $p^{*}$.

The Lagrangian associated with this problem is defined as $$\mathcal{L}(x,\lambda,\nu):=f_{0}(x)+\sum\limits\lambda_{i}f_{i}(x)+\sum\limits\nu_{i}h_{i}(x)$$where the vectors $\lambda$ and $\nu$ are the dual variables. In fact, the primal problem is equivalent to solving $$p^{*}=\inf_{x} \sup_{\lambda\geq0, \nu} \mathcal{L}(x,\lambda,\nu)$$as can be seen by noting that the supremum $$\sup_{\lambda\geq0, \nu}\mathcal{L}(x,\lambda,\nu)=f_{0}(x)$$for feasible $x$, and is $+\infty$ for infeasible ones.

---

The dual function is defined as $$g(\lambda,\nu):=\inf_{x\in\mathcal{D}}\mathcal{L}(x,\lambda,\nu)$$The dual function is *always* concave, even when the primal is *not* convex. We would refer to the set $\{(\lambda,\nu)\mid g(\lambda,\nu)>-\infty\}$ as $\mathrm{dom}g$, because restrictedly working on this set would not cause any loss of generality.

**Proposition**. $\forall \lambda\geq0, \nu$, we have $$g(\lambda,\nu)\leq p^{*}$$which means the dual function always yields lower bounds for the primal.

**Proof**. If $x^{*}$ is feasible, then $\forall \lambda\geq0$, $$\sum\limits \lambda_{i}f_{i}(x^{*})+\sum\limits \nu_{i}h_{i}(x^{*})\leq 0$$thus $\mathcal{L}(x^{*},\lambda,\nu)\leq f_{0}(x^{*})$. Thus, $$g(\lambda,\nu)= \inf_{x\in\mathcal{D}}\mathcal{L}(x,\lambda,\nu)\leq \mathcal{L}(x^{*},\lambda,\nu)\leq f_{0}(x^{*})\quad \square$$

The **dual problem** is asking about the tightest of such lower bounds, i.e. $$\text{maximize } g(\lambda, \nu) \quad \text{s.t.} \quad \lambda\geq 0, \quad\nu \text{ free}$$the solution of which we denote $$d^{*}=\sup_{\lambda\geq0, \nu}\inf_{x}  \mathcal{L}(x,\lambda,\nu)$$which is primal with max/min inverted.

---

We have already shown:

**Theorem (Weak Lagrangian Duality)**. $$d^{*}=\sup_{\lambda\geq0, \nu}\inf_{x}  \mathcal{L}(x,\lambda,\nu)\leq \inf_{x} \sup_{\lambda\geq0, \nu} \mathcal{L}(x,\lambda,\nu)=p^{*}$$which is in satisfying symmetric form. This in fact does not depend on any property of $\mathcal{L}$, because it's an instance of the general max-min theorem.

Strong duality, however, does not hold in general. But if the primal problem is *convex*, with minimal additional condition, we do have strong duality. One of these is **Slater's condition**:

**Theorem (Strong Lagrangian Duality)**. For a standard form convex optimization problem, namely $$\begin{align}
\text{minimize } f_{0}(x)\quad \text{s.t.} \quad f_{i}(x)&\leq 0 \\
Ax&=b
\end{align}$$which satisfies Slater's condition: $\exists x\in\mathrm{relint}\mathcal{D}$ such that $f_{i}(x)<0,\forall i$ and $Ax=b$ (i.e. at least one feasible point exists for all inequalities to hold strictly), we have $d^{*}=p^{*}$.[^1] Further, the optimals can be attained by some primal and dual variables (so the $\inf$/$\sup$ can be replaced by $\max$/$\min$).[^2]

**Proof**. cf. Boyd, p235.

## KKT Optimality Conditions

We still work without convexity assumptions. Suppose that the primal and dual optimal values are attained and equal (strong duality holds), with $x^{*}$ and $(\lambda^{*},\nu^{*})$ respectively. We have $$f_{0}(x^{*})=g(\lambda^{*},\nu^{*})\leq f_{0}(x^{*})+\sum\limits \lambda_{i}^{*}f_{i}(x^{*})+\sum\limits\nu_{i}h_{i}(x^{*})\leq f_{0}(x^{*})$$so we must have $\lambda_{i}^{*}f_{i}(x^{*})=0$. This condition is known as **complementary slackness**.

Further, if we assume the problem defining functions $f_{0}$, $f_{i}$'s and $h_{i}$'s are all differentiable (and thus $\mathcal{D}$ is open), then because $x^{*}$ minimizes $\mathcal{L}(x,\lambda^{*},\nu^{*})$ over $x\in\mathcal{D}$, its gradient must vanish, i.e. $$\nabla f_{0}(x^{*})+ \sum\limits \lambda_{i}^{*}\nabla f_{i}(x^{*}) + \sum\limits \nu_{i}^{*} \nabla h_{i}(x^{*})=0$$
These two conditions, together with the constraints themselves, are called the **Karush-Kuhn-Tucker (KKT)** conditions:

$$\begin{align}f_{i}(x^{*}) &\leq 0 ,\quad i=1,\dots,m\\
h_{i}(x^{*}) &= 0, \quad i=1,\dots,p \\
\lambda_{i}^{*} &\geq 0, \quad i=1,\dots,m \\
\lambda_{i}^{*}f_{i}(x^{*}) &= 0, \quad i=1,\dots,m \\
\nabla f_{0}(x^{*})+ \sum\limits \lambda_{i}^{*}\nabla f_{i}(x^{*}) + \sum\limits \nu_{i}^{*} \nabla h_{i}(x^{*})&=0 \end{align}$$

The next result states that, when the primal problem is *convex*, the KKT conditions are also sufficient for the points to be primal and dual optimal.

**Theorem**. For convex $f_{i}$'s and affine $h_{i}$'s, if $\tilde{x},\tilde{\lambda},\tilde{\nu}$ satisfy the KKT conditions, then they are primal and dual optimal with zero duality gap.

**Proof**. Since $\tilde{\lambda}>0$, $\mathcal{L}(x,\tilde{\lambda},\tilde{\nu})$ is convex in $x$. The gradient condition states that $\tilde{x}$ minimizes it, so $$g(\tilde{\lambda},\tilde{\nu})=\mathcal{L}(\tilde{x},\tilde{\lambda},\tilde{\nu})=f_{0}(\tilde{x})\quad \square$$**Corollary**. For a convex optimization problem, if Slater's condition holds (so strong duality holds), then $\tilde{x},\tilde{\lambda},\tilde{\nu}$ are optimal iff KKT conditions hold.

Also worth noting is that, the usual Lagrangian multiplier method for solving conditional optimals falls happily into solving KKT, too.

## Sensitivity

If we perturb the primal into $$\begin{align}
\text{minimize } f_{0}(x)\quad \text{s.t.} \quad f_{i}(x)&\leq u \\
h_{i}(x)&=v
\end{align}$$and denote the new optimum $p^{*}(u,v)$, we have:

**Theorem**. Assume that strong duality holds and the dual optimum is attained (e.g. convex problem satisfying Slater's condition). Then for all $u,v$ we have $$p^{*}(u,v)\geq p^{*}(0,0)-\lambda^{*T}u-\nu^{*T}v$$

**Proof**. Suppose $x$ is feasible to the perturbed primal. Then $$p^{*}(0,0)=g(\lambda^{*},\nu^{*})\leq f_{0}(x)+\sum\limits \lambda_{i}^{*}f_{i}(x)+\sum\limits\nu_{i}^{*}h_{i}(x)\leq f_{0}(x)+\lambda^{*T}u + \nu^{*T}v$$
and the results follows.


[^1]: In fact, Slater's condition can be further relaxed: if any of the $f_{i}$'s are in fact affine, the corresponding inequality also does not have to hold strictly.

[^2]: For a case where strong duality holds but cannot be attained, cf. Boyd Exercise 5.26, where Slater's condition does not hold, and $d^{*}$ is a limit point.
