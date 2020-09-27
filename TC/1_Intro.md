* Set
    * an unordered collection of elements empty set $\emptyset$
- Subsets and proper subsets
  - Subset notation: $\subseteq$, $S \subseteq T \Leftrightarrow(\forall x \in S \Rightarrow x \in T)$
  - Proper Subset: $\subset$
  - Two sets are equal iff they contain the same elements $S=T \Leftrightarrow(S \subseteq T) \wedge(T \subseteq S)$



## Func

* Ordered Pair and Binary Relation
    * Ordered Pair: $(a, b)$
        $(a, b)=(c, d) \Leftrightarrow(a=c) \wedge(b=d)$
    * Cartesian Product: $A \times B$
        $A \times B=\{(a, b) \mid a \in A \wedge b \in B\}$
    * Binary Relation on $A$ and $B:$
        $R \subseteq A \times B$
    * Inverse
        $R \subseteq A \times B \Rightarrow R^{-1} \subseteq B \times A$
* Ordered Tuple and n-ary Relation (Omitted)



Definition: A function $f: A \rightarrow B$ must satisfy:
$f \subseteq A \times B$
$ \forall a \in A, \exists$ exactly one $b \in B$ with $(a, b) \in f$
Note: We write $(a, b) \in f$ as $f(a)=b$
Domain, range



* one-to-one function:
    $\forall a, b \in A \wedge a \neq b \Rightarrow f(a) \neq f(b)$
* onto function: $f(A) = B$:
    $\forall b \in B, \exists a \in A$ such that $f(a)=b$
* bijection function:
    (one-to-one correspondence)
    one-to-one $+$ onto



Representation of Relations
- Directed graph: node, edge, path
- Matrix: Adjacency matrix

Properties of Relations $(R \subseteq A \times A)$

* reflexive: $\forall a \in A \Rightarrow(a, a) \in R$
* symmetric: $(a, b) \in R \wedge a \neq b \Rightarrow(b, a) \in R$
* antisymmetric: $(a, b) \in R \Rightarrow(b, a) \notin R$
* transitive: $(a, b) \in R,(b, c) \in R \Rightarrow(a, c) \in R$



Equivalence Relation

- reflexive, symmetric, transitive
- equivalence classes
    $[a]=\{b \mid(a, b) \in R\}$

Theorem: Let $R$ be an equivalence relation on a nonempty set $A$. Then the equivalence classes of $R$ constitute a partition of $A$



Partial Order

* reflexive, antisymmetric, transitive
* total order
* minimal element and maximal element



## 有限集无限集

**Equinumerous**

* Sets $A$ and $B$ equinumerous $\Leftrightarrow \exists$ bijection $f: A \rightarrow B$
* Cardinality and generalized Cardinality
* Finite and Infinite Sets

**Countable and Uncountable Infinite**

* A set is said to be <u>countably infinite</u> $\Leftrightarrow$ it is equinumerous with $\mathbb{N}$.
* $S$ is an <u>uncountable</u> set $\Leftrightarrow|S|>|\mathbb{N}|$

**Continuum Hypothesis**

$$
\begin{aligned}
&|\mathbb{N}|=\aleph_{0} \quad|\mathbb{R}|=\aleph_{1}\\
&\aleph_{0}<\aleph_{1}\\
&\exists ? w, \text { such that } \aleph_{0}<w<\aleph_{1}
\end{aligned}
$$

## 3

**The Principle of Mathematical Induction**

> Let $A$ be a set of natural numbers such that
> 1) $0 \in A,$ and
> 2) for each natural number $n,$ if $\{0,1,2, \cdots, n\} \in A,$ then $n+1 \in A$

**The Pigeonhole Principle**

> If $A$ and $B$ are finite sets and $|A|>|B|,$ then there is no one-to-one function from $A$ to $B$.
>

**The Diagonalization Principle**

> Let $R$ be a binary relation on a set $A,$ and let $D,$ the diagonal set for $R,$ be $\{a: a \in A \wedge(a, a) \notin R\}$. For each $a \in A,$ let
> $$
> R_{a}=\{b: b \in A \wedge(a, b) \in R\}
> $$
> Then $D$ is distinct from each $R_{x}$（目的就是要构造与$R_x$不同的）
>
>**Example**: Let us consider the relation
>
>$R=\{(a, b),(a, d),(b, b), (b, c),(c, c),(d, b),(d, c),(d, e),(d, f),\\(e, e),(e, f),(f, a),(f, c)， (f, d),(f, e)\}$
>
>Notice that:
> $$
> \begin{array}{ll}
> R_{a}=\{b, d\}, & R_{b}=\{b, c\},\\
> R_{c}=\{c\}, & R_{d}=\{b, c, e, f\}, \\
> R_{e}=\{e, f\}, & R_{f}=\{a, c, d, e\}
> \end{array}
> $$
> The corresponds to the diagonal set is (R中不存在(a,a),(d,d),(f,f))
> $$
> D=\{a, d, f\}
> $$

**Ex2**. 利用Diag原理证明(0,1)为不可数集

## Closure

 Reflexive, transitive closure of R is usually denoted R*.



# Ther

## Alphabet

