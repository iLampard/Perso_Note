# Topology concepts


### Metric space

#### Definition
A *metric* spaces is a pair $$(E,d)$$ where $$E$$ is a set and $$d: E \times E \Rightarrow R_+$$ such that 
- $$d(x, y) = 0 \Longleftrightarrow x = y$$
- $$d(x,y) \le d(x,z) + d(z,y)$$
- $$d(x,y)=d(y,x)$$

#### Remark
$$E$$ is a *vector space* on $$R$$ if there is an operation
- $$\forall x \in E, y \in E, x + y = y + x$$
- $$\lambda \in R, x \in E, \lambda x \in E$$
- $$0 \in E. \forall x \in E, x + 0 = x, \lambda 0 = 0$$


#### Definition
A *normed space* is a pair $$(E, \Vert \cdot \Vert)$$ where $$E$$ is a vector space and $$\Vert \cdot \Vert: E \Rightarrow R_+$$ such that 
- $$\Vert x \Vert = 0 \Rightarrow x = 0$$
- $$\Vert \lambda x \Vert = \vert \lambda \vert \Vert x \Vert, \forall \lambda \in R, x \in E$$
- $$\Vert x + y \Vert \le \Vert x \Vert + \Vert y \Vert$$


### Convergence

#### Definition
A sequence $$\{x_n\}$$ converges to $$x$$ in a metric space $$(E, d)$$ if $$\lim \limits_{n \rightarrow \infty} d(x_n,x)=0$$, that is, $$\forall \epsilon, \exists N, \forall n \ge N, d(x_n, x) \le \epsilon$$.


#### Remark
The definition of convergence depends on the distance function defined in the metric space.

#### Definition
Two distances $$d_1, d_2$$ are equivalent if $$\exists \alpha, \beta >0 s.t. \forall x, y, \alpha d_1(x,y) \le d_2(x,y) \le \beta d_1(x,y)$$.

Two norms $$\Vert \cdot \Vert_1, \Vert \cdot \Vert_2$$ are equivalent if $$\exists \alpha, \beta >0 s.t. \forall x, y, \alpha \Vert x \Vert_1 \le \Vert x \Vert_2 \le \beta \Vert x \Vert_1$$.

#### Property
Two equivalent distances induce the same converging sequences and the same limits.

#### Theorem
On a vector space of finite dimentions, all norms are equivalent.


### Open sets

#### Definition 

Given a metric space $$(E, d)$$, $$x \in E, r \ge 0$$, 
- $$B(x,r) = \{y: d(x, y) < r \}$$ is called the  *open ball* with center $$x$$ and radius $$r$$.
- $$B(x,r) = \{y: d(x, y) \le r \}$$ is called the  *closed ball* with center $$x$$ and radius $$r$$.
  
Given a metric space $$(E, d)$$, set $$U \subset E$$ is open if $$\forall x \in U, \exists r >0, B(x,r) \in U$$. 







