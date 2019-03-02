# Topology concepts


## Metric space

### Matric space
A *metric* spaces is a pair $$(E,d)$$ where $$E$$ is a set and $$d: E \times E \Rightarrow R_+$$ such that 
- $$d(x, y) = 0 \Longleftrightarrow x = y$$
- $$d(x,y) \le d(x,z) + d(z,y)$$
- $$d(x,y)=d(y,x)$$

### Vector space
$$E$$ is a *vector space* on $$R$$ if there is an operation
- $$\forall x \in E, y \in E, x + y = y + x$$
- $$\lambda \in R, x \in E, \lambda x \in E$$
- $$0 \in E. \forall x \in E, x + 0 = x, \lambda 0 = 0$$


### Normed space
A *normed space* is a pair $$(E, \Vert \cdot \Vert)$$ where $$E$$ is a vector space and $$\Vert \cdot \Vert: E \Rightarrow R_+$$ such that 
- $$\Vert x \Vert = 0 \Rightarrow x = 0$$
- $$\Vert \lambda x \Vert = \vert \lambda \vert \Vert x \Vert, \forall \lambda \in R, x \in E$$
- $$\Vert x + y \Vert \le \Vert x \Vert + \Vert y \Vert$$


## Convergence

### Convergence of sequence
A sequence $$\{x_n\}$$ converges to $$x$$ in a metric space $$(E, d)$$ if $$\lim \limits_{n \rightarrow \infty} d(x_n,x)=0$$, that is, $$\forall \epsilon, \exists N, \forall n \ge N, d(x_n, x) \le \epsilon$$.


#### Remark
The definition of convergence depends on the distance function defined in the metric space.

### Equivalence of distance and norms
Two distances $$d_1, d_2$$ are equivalent if $$\exists \alpha, \beta >0 s.t. \forall x, y, \alpha d_1(x,y) \le d_2(x,y) \le \beta d_1(x,y)$$.

Two norms $$\Vert \cdot \Vert_1, \Vert \cdot \Vert_2$$ are equivalent if $$\exists \alpha, \beta >0 s.t. \forall x, y, \alpha \Vert x \Vert_1 \le \Vert x \Vert_2 \le \beta \Vert x \Vert_1$$.

### Property
Two equivalent distances induce the same converging sequences and the same limits.

### Theorem
On a vector space of finite dimentions, all norms are equivalent.


## Open sets

### Balls 

Given a metric space $$(E, d)$$, $$x \in E, r \ge 0$$, 
- $$B(x,r) = \{y: d(x, y) < r \}$$ is called the  *open ball* with center $$x$$ and radius $$r$$.
- $$B(x,r) = \{y: d(x, y) \le r \}$$ is called the  *closed ball* with center $$x$$ and radius $$r$$.
  
### Open set  
Given a metric space $$(E, d)$$, set $$U \subset E$$ is open if $$\forall x \in U, \exists r >0, B(x,r) \in U$$. 

#### Properties of open set
- $$\varnothing$$ and $$E$$ are open sets.
- Any union of open sets is open.
- Any finite intersection of open sets is open.

##### Proof of the second property

Define $$V= \bigcup \limits_{i \in I} U_i=\{x \in E: \exists i \in I, x \in U_i \}$$.

Then $$\forall x \in V, \exists i \in I, x \in U_i$$.

Then by definition $$\exists r > 0, B(x,r) \subset U_i \subset V$$.  


##### Proof of the third property

Define $$W= \bigcap^n \limits_{i=1} U_i=\{x \in E: \forall i=1,2,..n, x \in U_i \}$$.

Then $$\forall x \in W, x \in U_i \forall i =1,2,..n$$. 

Then by definition $$\forall i, \exists r_i > 0, B(x,r_i) \subset U_i$$.

Set $$r=\min{r_1, r_2,..., r_n}$$, then $$B(x,r) \subset B(x, r_i) \subset U_i, \forall i$$. Therefore $$B(x, r) \subset W$$.



#### Remark: in $$R$$ an open ball is an open interval.


### Interior point
For $$A \subset E, x \in A$$, $$x$$ is an *interior point* if $$\exists r > 0$$, such that $$B(x,r) \subset A$$. The set of interior point is the interiors of A.

#### Theorem of interior point
A set if open iff all its points are interior points.

### Closed set

$$U \subset E $$ is closed if $$U^c = E \backslash U$$ is open.

#### Properties of closed set
- $$\varnothing$$ and $$E$$ are closed sets.
- Any intersection of closed sets is closed.
- Any finite union of closed sets is closed. 

### Closure point

For $$A \subset E$$, $$x \in A $$ is a closure point if $$\forall r $$, $$B(x, r) \cap A \neq \varnothing $$. 

The set of closure points is the closure of $$A$$.

A set is closed iff all its closure points are closure points.

#### Properties of closure point
- $$(cl A)^c = int (A^c)$$ 
- $$cl(A)$$ is the smallest closed set containning $$A$$.

#### Theorem of closure point 
$$x \in cl(A)$$ iff there exits a sequence $$\{ x_n\} \in A$$ that converges to $$x$$.

Typical use of the theorem: to show that $$A$$ is closed, show that if $$\{ x_n\}$$ is a sequence in $$A$$ that converges to $$x$$, then $$x$$ is in $$A$$.


### Compact set 
A subset $$A$$ of a metric space is *compact* if every sequence in $$A$$ admits a subsequence that converges to some point in $$A$$.

$$[0, 1]$$ is compact while $$(0,1]$$ is not.

#### Properties of compact set
- compact $$\Rightarrow$$ bounded and closed.
- A closed subset of a compact set is compact.

### Theorem of compact set
- In finite dimention space($$R^n$$), compact $$\Longleftrightarrow$$ bounded and closed.
- In finite dimention vector space with a norm, compact $$\Longleftrightarrow$$ bounded and closed.

#### Remark
- Any finite set is compact.
- $$(E, \Vert \cdot \Vert)$$, $$\widebar{\mathcal{B}}(0,1)$$ is compact.


### Complete set

#### Cauchy sequence
A sequence is a Cauchy sequence if $$\lim_\limits{n \rightarrow \infty, \\ m \rightarrow \infty} d(x_n, x_m) = 0$$

#### Remark of Cauchy sequence 
- A Cauchy sequnce is bounded.
- A converging sequence is Cauchy.
- A Cauchy sequence that has a converging subsequence is convering.

#### Complete set 
A subset $$A$$ of a metric space  is complete if every Cauchy sequence in $$A$$ conveges to some point in $$A$$. 

#### Properties of a complete set
- Complete $$\Rightarrow$$ closed.
- Compact $$\Rightarrow$$ complete.
- A closed subset of a complete set is complete.
  