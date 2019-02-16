# Stochastic process in discrete time

Let $$(\Omega, \mathcal{F}, \mathbb{P})$$ be a probability space.

### Filtration
#### Definition
A collection of random variables $$X:=(X_n)_{n \ge 0}$$ is called a (discrete time) stochastic process.


#### Definition
A non­decreasing sequence of $$\sigma$$-algebras $$\mathcal{F}_0 \subseteq \mathcal{F}_1 \subseteq ...\subseteq  \mathcal{F}_n \subseteq ... \subseteq \mathcal{F}$$ is called a *filtration*.

Let $$X:=(X_n)_{n \ge 0}$$ be a sequence of r.v.s. Let $$\mathcal{F}_n=\sigma(X_0, X_1, ..., X_n)$$, $$n \ge 0$$. The filtration $$(\mathcal{F_n})_{n \ge 0}$$ is called the natural filtration of $$X$$.


#### Definition
The probability space $$(\Omega, \mathcal{F}, \mathbb{P})$$ equipped with a filtration $$(\mathcal{F_n})_{n \ge 0}$$ denoted $$(\Omega, \mathcal{F_n}, \mathbb{P})$$ by *filtered probability space*.

#### Definition
A stochastic process $$X$$ is called adapted to the filtration $$(\mathcal{F_n})_{n \ge 0}$$ if $$X_n$$ is a $$\mathcal{F_n}$$ - measurable for every $$n \ge 0$$.

### Martingales
Let $$(\Omega, \mathcal{F_n}, \mathbb{P})$$ be a filtered probability space. A stochastic process $$X$$ is called a martingale(submartingale, supermartingale) with respect to the filtration $$(\mathcal{F_n})_{n \ge 0}$$ if it satisfies
- $$\mathbb{E} \vert X_n \vert < \infty, \forall n \ge 0$$.
- $$X$$ is adapted to $$(\mathcal{F_n})_{n \ge 0}$$.
- $$\mathbb{E} [ X_{n+1} \vert \mathcal{F_n} ]=(\ge, \le)X_n, \forall n \ge 0$$. 