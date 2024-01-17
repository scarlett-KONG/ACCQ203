## Algorithm 12: Berlekamp Algorithm

**Inputs:** Polynomial \( f \in \mathbb{F}_q[x] \) (product of polynomials without square factors) of degree \( n \).

**Outputs:** Factors of \( f \).

For \( 0 \leq j < n \) do
  - Calculate the coefficients of the matrix \( Q = ((q_{ij})) \in \mathbb{F}_q^{n \times n} \) such that \( \sum q_i x_i \equiv (x_j)^q \mod f \) for \( 0 \leq i, j < n \).

  - Calculate (using Gaussian elimination) a basis \([b_1, \ldots, b_k]\) for the kernel of \( Q - I \) in \( \mathbb{F}_q^{n \times n} \), where \( b \) is the vector \((1, 0, \ldots, 0)\).

  - Set \( F = \{f\} \).
  - Set \( j = 1 \).
  
  While \( \text{Length}(F) < k \) do
    - Increment \( j \).
    - For each \( f \) in \( F \) do
      - Initialize \( B = \emptyset \).
      - For each \( \alpha \) in \( \mathbb{F}_q \) do
        - Add \( a \) to \( B \), where \( a = f \land (b_j - \alpha) \).
      - The submodule \( B \) is called the Berlekamp subalgebra.

The dimension of \( \text{ker}(Q - I) \) equals the number of irreducible factors of \( f \).

**Theorem 195:** The Berlekamp algorithm is correct.

The proof is based on two observations:

**Lemma 196:** Let \( g \) be a divisor of \( f \) (polynomial without square factors), then
  - \( C = \{ f \in \mathbb{F} \;|\; \text{deg} f > 1 \} \)
  - \( a = f \land (b_j - \alpha) \), if \( \text{deg} a \geq 1 \), then
  - Update \( F = (F \cup \{f\}) \cup B \).

# Now I don't know why they designed this algorithm like this.
**Theorem 195:** The Berlekamp algorithm is correct.

The proof is based on two observations:

**Lemma 196:** Let \( g \) be a divisor of \( f \) (polynomial without square factors), then for any \( b \) in the Berlekamp subalgebra \( B \),
\[ g = \prod g(x) \land (b(x) - \alpha). \]

*Proof:* Reduced modulo \( f_i \), \( b \) is a constant in \( \mathbb{F}_q \). Thus, \( g \land (b - \alpha) \) captures all factors \( f_i \) of \( g \) such that \( b(x) \equiv \alpha \mod f_i \). By varying \( \alpha \), we ensure capturing all factors.

**Lemma 197:** For any \( i \neq i' \), there exists at least one element \( b_j \) in the basis of \( B \) such that equation (6) separates the factors \( f_i \) and \( f_{i'} \).

*Proof:* If not, all \( b_j \) would have the same reduction modulo \( f_i \) and \( f_{i'} \), generating only the subspace of \( b \in B \) such that \( b \mod f_i = b \mod f_{i'} \), which is impossible.

*Remark 198:* When \( q \) is large, finding the correct \( \alpha \in \mathbb{F}_q \) can be time-consuming. One may prefer choosing \( a \) as a random combination of \( (b_i) \) and compute the gcd of \( f \) with \( a^{(p-1)/2} - 1 \), using the same trick as in the Cantor-Zassenhaus algorithm.

*Remark 199:* With the Berlekamp algorithm, the step of factoring into distinct degrees can be omitted; only the assumption " \( f \) has no square factors " is crucial. However, retaining this assumption partially factors \( f \) at a low cost and saves time.

**Example 200:** Let \( f(x) = x^4 + x^3 + x^2 + 1 \) be the polynomial in \( \mathbb{F}_4[x] \) to factorize. To represent \( \mathbb{F}_4[x]/\langle f \rangle \), we use the monomial basis:
\[ \mathbb{F}_4[x]/\langle f \rangle = \mathbb{F}_4 \oplus \mathbb{F}_4x \oplus \mathbb{F}_4x^2 \oplus \mathbb{F}_4x^3. \]
The images of the basis of \( \mathbb{F}_4[x]/\langle f \rangle \) under the application \( \sigma : h(x) \mapsto (h(x))^4 \mod f \) are:
\[ \begin{cases} 1 \mapsto 1 \mod f \\ x \mapsto x^4 = x^3 + x^2 + 1 \mod f \\ x^2 \mapsto x = x \mod f \\ x^3 \mapsto x^{12} = x^2 + x + 1 \mod f \end{cases} \]
We deduce the matrix \( Q \) of \( \sigma \):
\[ Q = \begin{bmatrix} 1 & 1 & 0 & 1 \\ 0 & 0 & 1 & 1 \\ 0 & 1 & 0 & 1 \\ 0 & 0 & 0 & 1 \end{bmatrix} \]
Now we seek the kernel of:
\[ Q - I_4 = \begin{bmatrix} 0 & 1 & 0 & 1 \\ 0 & 1 & 1 & 1 \\ 0 & 1 & 1 & 1 \\ 0 & 0 & 1 & 1 \end{bmatrix} \]
The matrix has rank 2. Therefore, the kernel is of dimension \( r = 2 \) (indicating two irreducible factors). Its basis is:
\[ b_1 = \begin{bmatrix} 1 \\ 0 \\ 0 \\ 1 \end{bmatrix} \quad \text{and} \quad b_2 = \begin{bmatrix} 0 \\ 1 \\ 0 \\ 0 \end{bmatrix} \]
The vector \( b_1 \) corresponds to the constant polynomial \( b_1(x) = 1 \), and \( b_2 \) corresponds to the polynomial \( b_2(x) = x + x^2 \). We attempt to calculate the gcd \( f \land (x^3 + x + \alpha) \) for \( \alpha \in \mathbb{F}_4 \). (Note that searching for a factor with \( b_1 \) is unnecessary and always yields the same result). Already, for \( \alpha = 0 \): \( f \land (x^3 + x) = x + 1 \), and for \( \alpha = 1 \): \( f \land (x^3 + x + 1) = x^3 + x + 1 \). We have found two factors, and there are no others, so \( f \) is decomposed:
\[ f = (x + 1)(x^3 + x + 1) \]
In hindsight, we can analyze that \( \mathbb{F}_4[x]/\langle x^4 + x^3 + x^2 + 1 \rangle \) is isomorphic to \( \mathbb{F}_4 \oplus \mathbb{F}_{64} \) with the mapping:

\(  \mathbb{F}_4[x]/\langle x^4 + x^3 + x^2 + 1 \rangle \rightarrow (0,1) \)



### Factoring in Other Rings

We will discuss factorization in the rings Z[x] and Q[x]. The situation is less straightforward, and we'll only illustrate the techniques employed so far. A complete factorization algorithm is not expected at the end of this discussion.

**General Principles**

Factorizing a polynomial in Q[x] can be reduced to factorization in Z[x] by multiplying the polynomial by an integer that clears all denominators. We will focus on factorization in Z[x].

**Definition 203:** If \( f = f_nx^n + \ldots + f_1x + f_0 \in Z[x] \), we define the \( p \)-norm as \(\|f\|_p = \sum |f_i|^p\) and \(\|f\|_\infty = \max |f_i|\), where \(0 \leq i \leq n\).

**Lemma 204 (Mignotte's Bound):** If the polynomial \( g \in Z[x] \) divides the polynomial \( f = f_nx^n + \ldots + f_1x + f_0 \in Z[x] \), then
\[ \|g\|_\infty \leq (n+1)^{1/2n}2^{n}|f_n| \cdot \|f\|_\infty. \]

*Proof:* [Admitted]

All factorization algorithms in Z[x] follow a similar approach. Suppose \( f \) factors as \( f(x) = f_1(x)f_2(x) \ldots f_k(x) \in Z[x] \). Then, reducing the equality modulo \( m \) yields \( f(x) = f_1(x)f_2(x) \ldots f_k(x) \in Z/mZ[x] \).

For \( m \) sufficiently large (\( m \geq 2B + 1 \), where \( B \) is Mignotte's bound), when writing \( f_i \) with coefficients centered between \(\lfloor -m/2 \rfloor\) and \(\lceil m/2 \rceil\), the expression of \( f_i \) coincides with that of \( f_i \). Thus, we can recover the factorization of \( f_i \) by lifting the \( f_i \).

Is this sufficient for factorization? Unfortunately, sometimes the factorization of \( f \) is finer than that of \( f \): i.e., the factorization of \( f \) has more factors than that of \( f \).

**Example 205:** Consider
\[ f(x) = x^6 + x^5 + x^4 + x^2 + x + 1 = (x^2 + x + 1) (x^4 + 1) \in Z[x] \]
Randomly choosing large prime numbers \( m \) and factoring \( f \) in \( F_m[x] \) using the methods from the previous sections, we obtain irreducible factors:
\[ f(x) = (x + 3105)(x - 3104) (x^2 + 825)(x^2 - 825) \in F_{6421}[x] \]
\[ f(x) = (x^2+x+1)(x+225)(x+2975)(x-225)(x-2975) \in F_{7121}[x] \]
\[ f(x) = (x+480)(x-479)(x^2+1758x-1)(x^2-1758x-1) \in F_{5347}[x]. \]

As they are, these factorizations in \( F_m[x] \) do not give the factorization in \( Z[x] \). It is necessary to figure out how to recombine the obtained factors to recover \( f_1 \) and \( f_2 \).

In general, one needs to always reassemble the factors of \( f \) and test if they are indeed factors of \( f \) as polynomials in \( Z[x] \).

**Algorithm 13: Factoring Algorithm in Z**
**Inputs:** Polynomial \( f \in Z[x] \) without square factors
1. Set \( B \leftarrow (n+1)^{1/2n}2^{n}|f_n| \cdot \|f\|_\infty \) (Mignotte's bound)
2. Let \( m \) be a power of a prime such that \( m \geq 2B + 1 \) and \( p \) divides \( f_n \)
3. Factorize \( f \) in \( Z/mZ[x] \): \( f = r_1r_2 \ldots r_s \in Z/mZ[x] \)
4. For \( S \subseteq [1, s] \) do
   - \( \tilde{g} \leftarrow \prod_{i \in S} r_i \)
   - Lift \( g \) in \( Z[x] \) by centering the coefficients between \(-m/2\) and \(m/2\)
   - Test if \( g \) divides \( f \) in \( Z[x] \)

Two types of choices for \( m = p^e \) are possible:
1. Choose a large prime \( p \) (with \( e = 1 \))
2. For a fixed prime \( p \), choose a large exponent \( e \)

In the second case, factoring \( f \) is done using Hensel's lemma and lifting. This is the most efficient method and the one used in practice.

For the last step (grouping factors), one can either enumerate all possibilities (there can be an exponential number of cases to handle), or use the LLL algorithm wisely to find the grouping of factors \( S \).

**Remark 206:** If attempting to group factors by brute force, it's useful to first check if the constant coefficient of \( g \) divides that of \( f \). This saves computational effort.

### Exercises

**Exercise 207:**
1. Show that if \( f = gh \) in \( Z[x] \), then \( f = g h \).
2. How can we improve the factorization algorithm in Z[x] (consider Mignotte's bound)?

**Exercise 208:** Revisiting exercise 157, demonstrate that there exist polynomials such that, regardless of the choice of \( p \), the factor grouping step is

inevitable in the factorization algorithm in Z[x].

# How to factor in Z?

### Hensel Lifting

Let \( m \) be an integer. Hensel lifting is a technique that, given an initial factorization \( f \equiv gh \mod m \) in \( Z/mZ[x] \), extends this factorization to \( f \equiv g^\wedge h \) in \( Z/mZ[x] \). To achieve this, we assume a known Bézout relation \( sg + th \equiv 1 \mod m \). In short, if
\[ \hat{e} = f - gh, \quad \hat{g} = g + te, \quad \text{and} \quad \hat{h} = h + se, \]
then
\[ f - \hat{g}\hat{h} = (1 - sg - th)e - ste \equiv 0 \mod m. \]

The only drawback of this approach is that the degrees of \( \hat{g} \) and \( \hat{h} \) may be larger than those of \( g \) and \( h \). To overcome this issue, we need to resort to certain Euclidean divisions.

For simplicity, we assume that \( f, g, \) and \( h \) are monic polynomials.

\[ g^*, h^*, s^*, t^* \in Z[x] \]

\[ f \equiv g^*h^* \mod m^2 \quad \text{and} \quad s^*g^* + t^*h^* \equiv 1 \mod m^2 \]

We present a version that allows us to transition from

\[ \begin{cases} f \equiv gh \mod m \\ sg + th \equiv 1 \mod m \end{cases} \]

to

\[ \begin{cases} g, h, s, t \in Z[x] \\ \deg s < \deg h, \quad \deg t < \deg g, \end{cases} \]

\[ \begin{cases} g \equiv g^* \mod m, \quad h \equiv h^* \mod m \\ s \equiv s^* \mod m, \quad t \equiv t^* \mod m \end{cases} \]

**Algorithm 14: Hensel Lifting**

**Inputs:** Integer \( m \), elements \( f, g, h, s, t \in Z[x] \) such that
\[ f \equiv gh \mod m^2 \]
**Outputs:** Elements \( g^*, h^*, s^*, t^* \in Z[x] \) such that
\[ f \equiv g^*h^* \mod m^2 \]

1. \( e \leftarrow f - gh \mod m^2 \)
2. \( (q, r) \leftarrow \) Euclidean division of \( se \) by \( h \) modulo \( m^2 \)
3. \( g^* \leftarrow g + te + qg \mod m^2 \)
4. \( h^* \leftarrow h + r \mod m^2 \)
5. \( b \leftarrow sg^* + th^* - 1 \mod m^2 \)
6. \( (c, d) \leftarrow \) Euclidean division of \( sb \) by \( h^* \) modulo \( m^2 \)
7. \( s^* \leftarrow s - d \mod m^2 \)
8. \( t \leftarrow t - tb - cg \)

**Example 210:**
Consider the polynomial \( f(x) = x^4 + 78x^3 - 2556x^2 + 4389x - 722 \in Z[x] \) that needs to be factored. An agent has already determined that
\[ f(x) \equiv (x^2 + x - 1)(x^2 - x - 1) \mod 3 \]
and wishes to find a factorization modulo 3. By applying Euclid's algorithm in \( F_3 \), we can calculate
\[ s_0(x) = -x + 1, \quad t_0(x) = x + 1, \quad s_0g_0 + t_0h_0 = -2 \equiv 1 \mod 3. \]
After the first lifting step, we obtain
\[ g_1(x) = x^2 + 4x - 1, \quad h_1(x) = x^2 + 2x + 2, \]
\[ s_1(x) = -x + 1, \quad t_1(x) = x + 1, \]
and
\[ f - g_1h_1 = 72x^3 - 2565x^2 + 4383x - 720 \equiv 0 \mod 9, \]
\[ s_1g_1 + t_1h_1 = 9x + 1 \equiv 1 \mod 9 \]
After a second lifting step, we arrive at
\[ g_2 = x^2 + 22x - 19, \quad h_2 = x^2 - 25x + 38, \quad s_2 = -x + 10, \quad t_2 = x + 37, \]
and
\[ f - g_2h_2 = 81x^3 - 2025x^2 + 3078x \equiv 0 \mod 81, \]
\[ s_2g_2 + t_2h_2 = -648x + 1216 \equiv 1 \mod 81. \]
After a third lifting step, we obtain
\[ g_3 = x^2 + 103x - 19, \quad h_3 = x^2 - 25x + 38, \quad s_3 = 2591x + 1630, \quad t_3 = -2591x + 1333, \]
and
\[ f - g_3h_3 = 0. \]
Not only did we achieve a factorization modulo 38 (which holds true modulo \( 3^l \) for all \( l \leq 8 \)), but we eventually obtained the actual factorization in \( Z[x] \).

Grouping factors with LLL

We assume the following result:

Lemma 212. Let f and g be two polynomials in Z[x] of degrees n and k (non-zero). Let m be an integer such that (∥f∥₂)ᵏ(∥g∥₂)ⁿ < m. Suppose u ∈ Z[x] is a non-constant, unitary polynomial dividing f and g in Z/mZ[x]. Then f ∧ g ∈ Z[x] is non-constant.

This result is applied as follows: f is the polynomial to be factored, u is one of the modular factors of f, and g is a polynomial constructed expressly to satisfy the conditions of the lemma. To achieve this, we choose a polynomial with low norm of the form:

\[ g = qu + rm \]

This problem can be solved by viewing the polynomials as vectors and finding a short vector, using the LLL algorithm, in the lattice generated by:

\[ \{u(x)x^i; 0 ≤ i < j - d\} \cup \{m x^i; 0 ≤ i < j\} \]

where d is the degree of u and j is a parameter to be chosen.

Example 213:

We aim to factorize \(x^3 - 1\) whose factorization (still unknown) is \(f = (x - 1)(x^2 + x + 1) \in Z[x]\). We fix \(p = 7\), \(e = 8\), and \(m = p^e = 5764801\) (which exceeds the Mignotte bound). We start by calculating a factorization in F7, which is:

\[ f = (x - 2)(x - 1)(x + 3) \in F7[x] \]

We apply the Hensel's lemma (2 times) to lift this factorization to:

\[ f = (x + 19)(x - 1)(x - 18) \in Z/72Z[x] \]

where the terms correspond. We reapply Hensel's lemma to obtain:

\[ f = (x + 1048)(x - 1)(x - 1047) \in Z/74Z[x] \]

and finally:

\[ f = (x + 3376854)(x - 1)(x - 3376853) \in Z/78Z[x] \]

At this stage, we look for a factor of f by applying the lemma. We choose \(u(x) = x - 3376853\) (arbitrarily), so g must be of the form:

\[ g(x) = (x - 3376853)v(x) + 5764801w(x), \quad v, w \in Z[x] \]

We look for g as a vector in the lattice generated by the column vectors of:

\[ \begin{bmatrix} -3376853 & 0 & 5764801 & 0 & 0 \\ 1 & -3376853 & 0 & 5764801 & 0 \\ 0 & 1 & 0 & 0 & 5764801 \end{bmatrix} \]

whose LLL-reduced basis is:

\[ \begin{bmatrix} 1 & -1424 & 80 \\ 1 & 1345 & 0 \\ 1 & 80 & 1345 \end{bmatrix} \]

The first vector is relatively short and corresponds to the polynomial \(g(x) = x^2 + x + 1\). It can be verified that \(f ∧ g = x^2 + x + 1\), which is indeed a factor of f in Z[x].

Exercise 214:

Let \(f = x^4 - x^3 - 5x^2 + 12x - 6 \in Z[x]\) be a polynomial to factorize.

1. (a) Show that f has 4 roots α, β, γ, δ in F13.
   (b) Give 4 integers αˆ, β, γˆ, and δ ∈ Z ∩ [−134, 134] such that
       \( αˆ \equiv α \mod 13, \; β \equiv β \mod 13, \; γˆ \equiv γ \mod 13, \; δ \equiv δ \mod 13 \),
       and \( f(x) = (x - αˆ)(x - βˆ)(x - γˆ)(x - δˆ) \in Z/134Z[x] \).
   (c) Show that αˆ, β, γˆ, and δ are not roots of f.
2. Let \(u(x) = x + 7626\) and \(j = 3\).
   (a) Verify that \(u(x)\) is a divisor of f modulo 13. Give a LLL-reduced basis of the lattice described by equation 9.
   (b) Deduce a factor of f.
   (c) Calculate the complete factorization of f over Z.