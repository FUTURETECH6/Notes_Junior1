# Common DFA/ReL

## ReL

原始定义：在交并差补、concatenation、kleene star下封闭，一般构造FA来证明

$a^{*} b^{*}, a^{*} \cup b^{*}, a(a^{*} \cup b^{*}), (a^{*} \cup b^{*}) a(a^{*} \cup b^{*}), aaaaa^*$

strings of *a, b* with even number of *b*s?

## nReL

一般用不符合泵定理来证明语言不正则

$\{a^nb^n, n\geqslant0\}$

 $\{a^n: n为质数\}$

$L = \{w\in\{a, b\}^*: w\text{ has an equal number of }a\text{'s and }b\text{'s}\}$

ww^R^

ww

ww^bar^

# Common NFA

# Common PDA/CFL

## CF

Closed under: K-star, concatenation, union, intersection(with regex) 交并

**Theorem**: The CFL are ==**not closed**== under <u>intersection</u> or <u>complementation</u>

 

w=w^R^

{a^n^b^n^}

{a^n^b^m^: n≠m}

{\#a=\#b}: R = 
$$
\begin{align}
  S &\to \epsilon \\
  S &\to aSbS \\
  S &\to bSaS \enspace.
\end{align}
$$
{\#a=2\#b}: R = {$S \to a S b S b \mid b S a S b \mid b S b S a \mid S S \mid \varepsilon$}

## nCF

{ww}

$\{a^{n^2}\}$

{www}

{w \in \{a, b, c\}, \#a = \#b = \#c}

# Undecidablity

|                | recursive                       | RE                          | co-RE                                                |
| -------------- | ------------------------------- | --------------------------- | ---------------------------------------------------- |
| Idendification | decide                          | semi-decide                 | undecidable yet semi-decidable language's complement |
| Closure        | K-star, concatenation, 交并差补 | K-star, concatenation, 交并 |                                                      |
| Ex.            | Hbar                            | H                           |                                                      |

少于...：递归

至少...：递归可枚举但不递归（找不到就会永不停机

至多...：co-RE（非递归可枚举

恰好.. .：既不是RE也不是co-RE