In this practical work (T.P.), we delve into the manipulation of systems of polynomial equations. The mechanism for finding a Gröbner basis that we are going to study "generalizes three classical techniques": Gauss's pivot method for the triangularization of linear systems, Euclid's algorithm for finding the greatest common divisor (gcd), and the simplex method for optimizing linear programs.

In the following, K represents a field, and K[x] denotes the set of polynomials in the variable x = (x1, ..., xn). Here, x denotes the monomial x1 ··· xn. If you have any specific questions or concepts you'd like me to explain further, please let me know.

Title: Existence of Gröbner Bases
Orders, Division, and Remainder

Definition 215. A monomial order is a binary relation ≼ on Nn such that:
1. ≼ is a total order,
2. for all α, β, and γ in Nn, if α ≼ β, then α + γ ≼ β + γ, and
3. ≼ is well-ordered (every non-empty subset of Nn has a smallest element).

Example 216. Lexicographic order is a monomial order.

Let f(x) = ∑α∈Nn cαxα ∈ K[x1, ..., xn] be a non-zero multivariate polynomial (the coefficients cα ∈ K are all zero except for a finite number of them). Each product cαxα (where cα ≠ 0) is called a term. The concept of monomial order allows us to define a multidegree:
mdeg(f) = max{α ∈ Nn; cα ≠ 0} ∈ Nn
and the notions of leading coefficient cmdeg(f), leading monomial xmdeg(f), leading term td(f) = cmdeg(f)x^mdeg(f), where td(G) denotes {td(g); g ∈ G} for a family of polynomials G.

Example 217. For lexicographic order, the leading term of the polynomial f = 9xy - 2xy + 4x is td(f) = -2xy.

Exercise 218. Explain the following code lines:
```python
MPol.<x,y,z> = PolynomialRing(QQ,3, order='lex')
x<y^2
f = 2*x^2*y + 7*z^3
f.lt()
f.lc()
f.lm()
```
Here I get one problem, what are the f.lt(), f.lc(), f.lm() represant?

What are the other monomial orders implemented in SageMath?

Recall that the Euclidean division algorithm aims to express an element as a multiple of another. The following algorithm adapts the Euclidean division algorithm to the multivariate case.
Translation:

Theorem 230 (Dickson's Lemma): Every monomial ideal is generated by a finite family of monomials. Intuitively, Dickson's Lemma states that the step diagram of a monomial ideal has only a finite number of corners.

I don't know how to use this. How to use a finite group of monomials to generate the ideal?


Proof: Let \(I = \langle x^A \rangle\) be a monomial ideal. We introduce \(\leq\), the product order on \(N^n\), i.e., \(\alpha \leq \beta\) if \(x^\alpha | x^\beta\), or equivalently, \(\forall i \in \{1, \ldots, n\}\), \(\alpha_i \leq \beta_i\). Let \(B\) be the set of minimal elements of \(A\) for \(\leq\). Since \(\leq\) is a well-founded order (no infinite strictly decreasing sequence), for any exponent \(\alpha\), there exists an exponent \(\beta \in B\) such that \(\beta \leq \alpha\). Also, \(\langle x^B \rangle = \langle x^A \rangle\). It remains to show that \(B\) is finite, which we prove by induction. If \(n = 1\), the order is total, and \(B\) contains only one element. Otherwise, let \(A' = \{\alpha' \in N^{n-1}; \exists \alpha_n \in N, (\alpha', \alpha_n) \in A\}\). By the induction hypothesis, the set of minimal elements \(B'\) of \(A'\) is finite. For any \(\beta' \in B'\), let \(b_{\beta'}\) be the maximum \(\beta'\) in \(B'\). Prove that for all \(\beta = (\beta', \beta_n) \in B\), \(\beta_n \leq b\). Let \(\alpha = (\alpha', \alpha_n) \in A\). Then, there exists a certain \(\beta \in B\) such that \(\beta \leq \alpha\). If \(\alpha_n > b\), we would have \((\beta', b_{\beta'}) \leq (\beta', b) \leq \alpha\), and \(\alpha\) is not minimal. By the same reasoning, we show that each coordinate of the elements in \(B\) is bounded, hence \(B\) is finite.

Exercise 231: Let \(A = \{(\alpha, \beta) \in N^2; -7\alpha^2 + 19\alpha\beta - 7\beta^2 + 13 = 0\}\) and \(I = \langle x^A \rangle\), the ideal from Figure 9.
1. Do the polynomials \(x^6y^3 + x^2y\) and \(x^2y^4 + x^7y^3\) belong to \(I\)?
2. Give a finite set of generators for the ideal from Figure 9.

Gröbner Basis: In the following, \(\langle td(E) \rangle\) denotes the ideal generated by the leading terms of polynomials in \(E\).

Definition 232: Let \(\preceq\) be a monomial order and \(I \subseteq K[x]\) an ideal. A finite set of polynomials \(G \subseteq I\) is a Gröbner basis of the ideal \(I\) with respect to the order \(\preceq\) if the leading terms of \(G\) and \(I\) generate the same monomial ideal, i.e., \(\langle td(G) \rangle = \langle td(I) \rangle\).

Remark 233: The term "basis" is poorly chosen because a Gröbner basis remains one if elements are added to it. The following lemma shows that a Gröbner basis is automatically a generating family for the ideal.

Lemma 234: Let \(I\) be an ideal in \(K[x]\). If \(G \subseteq I\) is a Gröbner basis for \(I\), then \(G\) generates \(I\).

Proof: Let \(G = \{g_1, \ldots, g_s\}\) be a family of polynomials that forms a Gröbner basis for an ideal \(I\). Consider any polynomial \(f\) in \(I\). Let \((q_1, \ldots, q_s)\) and \(r\) be the quotients and remainder of \(f\) in its "division" by \(G\). Then, \(r = f - \sum_{i=1}^s q_i g_i \in I\). Therefore, \(td(r) \in td(I) \subseteq \langle td(g_1), \ldots, td(g_s) \rangle\). Lemma 225 shows that \(td(r)\) is divisible by one of \(td(g_1), \ldots, td(g_s)\), while Algorithm 15 assures the opposite. Hence, \(r = 0\) and \(f \in \langle G \rangle\).

Example 235: Consider, in the ring \(Q[x, y]\) with lexicographic order, the ideal \(I\) generated by the polynomials \(f_1 = xy^2 + 2y^2\) and \(f_2 = x^5 + 5\). The set of leading monomials of polynomials in \(I\) is (as computed later in Example 250):
\[ \{x^\alpha y^\beta; \alpha \geq 5 \text{ or } \beta \geq 2\}, \]
while \(td f_1\) and \(td f_2\) generate only the monomials
\[ \{x^\alpha y^\beta; \alpha \geq 5 \text{ or } (\beta \geq 2 \text{ and } \alpha \geq 1)\}. \]
Therefore, the family \(\{f_1, f_2\}\) is not a Gröbner basis for the ideal \(I\) that it generates.

Theorem 236: Every ideal \(I\) in \(K[x]\)

 has a Gröbner basis. Proof: Apply Dickson's Lemma to the monomial ideal \(\langle td(I) \rangle\). Obtain a finite family of \(td(I)\), lift it to a family of \(I\) to get a Gröbner basis.

Corollary 237 (Hilbert): Every ideal \(I\) in \(K[x]\) is Noetherian (i.e., generated by a finite set of elements).
Translation:

Membership Test
Lemma 238: When a polynomial family \(G\) is a Gröbner basis for the ideal \(I\), the remainder of a polynomial \(f\) in its division by \(G\) is the unique polynomial \(r\) such that \(f - r \in I\) and no term of the polynomial \(r\) is divisible by a monomial of \(td(G)\).

Proof: Suppose there exist two polynomials \(r_1\) and \(r_2\) satisfying the assumptions. Then, \(r_1 - r_2 \in I\). By Lemma 225, \(td(r_1 - r_2)\) would be divisible by one of the \(td(g)\) for \(g \in G\), which is excluded by our assumptions.

Theorem 239: Let \(G\) be a Gröbner basis for an ideal \(I \subseteq K[x]\), and \(f \in K[x]\) be a polynomial. Then, \(f\) belongs to \(I\) if and only if \(f \, \text{rem} \, G = 0\).

Example 240: Consider the ring \(Q[x, y]\) with lexicographic order and the polynomials
\[ f = 5y^3, \quad f_1 = xy^2 + 2y^2, \quad f_2 = x^5 + 5. \]
Let \(I\) be the ideal \(I = \langle f_1, f_2 \rangle\). We have already seen (see Example 223) that the division of \(f\) by \(f_1\) and \(f_2\) does not result in 0, even though \(f\) is indeed in \(I\). This indicates that \(\{f_1, f_2\}\) is not a Gröbner basis for \(I\). A Gröbner basis for \(I\) is \((x^5 + 5, y^2)\) (see Example 250). It is easy to see that the division of \(f\) by this basis does yield the zero polynomial.

Exercise 241:
1. What is the remainder of \(f\) by \((f_1, f_2)\)?
2. What is the least common multiple (lcm) between \(td(f_1)\) and \(td(f_2)\)? By how much should \(f_1\) and \(f_2\) be multiplied, respectively, to have the same leading term?
3. Show that there exist polynomials \(q_1\) and \(q_2\) such that \(f = q_1f_1 + q_2f_2 + r\) with \(r = 0\).
4. Conclude.

## Below we start the exercise on Grobner

# Construction of Gröbner Bases
## Characterization of Gröbner Bases

### Definition 242
Let g, h ∈ K[x] be two non-zero polynomials. The syzygy polynomial of g and h is defined as:

\[ S(g, h) = \frac{{\text{ppcm}(td(g), td(h)) \cdot td(g)}}{{\text{ppcm}(td(g), td(h))}} \cdot (g - h) \]

### Example 243
Consider the ring \( Q[x, y] \) with lexicographic order and the polynomials:

\[ g_1 = xy^2 + 2y^2 \]
\[ g_2 = x^5 + 5.25 \]

The leading term of \( g_1 \) is \( xy \), and that of \( g_2 \) is \( x \). Their least common multiple (ppcm) is \( 5x \cdot y \). To make these terms cancel each other, we multiply \( g_1 \) by \( x \) and \( g_2 \) by \( y \). The syzygy polynomial is:

\[ S(g_1, g_2) = 2xy - 5y \]

### Lemma 244
Let \( G = \{g_1, ..., g_s\} \subseteq K[x] \) be a family of polynomials. Suppose there exists an exponent \( \delta \in \mathbb{N}^n \), exponents \( (\alpha_1, ..., \alpha_s) \subseteq \mathbb{N}^n \), and scalars \( (c_1, ..., c_s) \subseteq K^n \) such that for all \( i \) between 1 and \( s \):

\[ \alpha_i + \text{mdeg}(g_i) = \delta \]

And the polynomial:

\[ f = \sum_{i=1}^{s} c_i x^{\alpha_i} g_i \]

Satisfies \( \text{mdeg}(f) \prec \delta \) (cancellation of leading terms). Let \( x^{\gamma_{i,j}} = \text{ppcm}(td(g_i), td(g_j)) \) for \( 1 \leq i < j \leq s \). Then \( x^{\gamma_{i,j}} \) divides \( x^\delta \), and there exist scalars \( c_{i,j} \in K \) such that:

\[ f = \sum_{1 \leq i < j \leq s} c_{i,j} x^{\delta - \gamma_{i,j}} S(g_i, g_j) \]

And \( \text{mdeg}(x^{\delta - \gamma_{i,j}} S(g_i, g_j)) \prec \delta \) for all \( 1 \leq i < j \leq s \).

### Proof
We prove the lemma by induction on \( s \). For \( s \geq 2 \), we have:

\[ f = (c_1 + c_2) x^{\alpha_2} g_2 + \sum_{i=3}^{s} c_i x^{\alpha_i} g_i \]

If \( s = 2 \), then \( c_2 = -c_1 \) for cancellation of the leading term to occur, concluding the proof. For \( s \geq 3 \), we reason by induction. Considering equation (11) and the degree of \( f \), we must have \( \text{mdeg}(g) \prec \delta \). We can apply our induction hypothesis to the polynomial \( g \) to conclude.

### Example 245
To illustrate this lemma, consider \( Q[x, y, z] \) with lexicographic order. Let:

\[ g_1 = x^2 + 5x \]
\[ g_2 = xy + y + 1 \]
\[ g_3 = 2xyz + z \]

Choose \( x^{\alpha_1} = yz, x^{\alpha_2} = xz, x^{\alpha_3} = x, \) etc., and \( c_1 = c_2 = 1, c_3 = -2 \). We obtain:

\[ f = 6xyz = yz \cdot g_1 + xz \cdot g_2 - x \cdot g_3 \]

The highest degree terms, with \( \delta = (2, 1, 1) \), cancel each other in the sum, leaving only a term of degree \( (1, 1, 1) \). The syzygy polynomials are:

\[ S_{1,2} = S(g_1, g_2) = yg_1 - xg_2 = 4xy - x \]
\[ S_{1,3} = S(g_1, g_3) = 2yzg_1 - xg_3 = 10xyz - xz \]
\[ S_{2,3} = S(g_2, g_3) = 2g_2z - g_3 = 2yz + z \]

The least common multiple between \( td(g_1) \) and \( td(g_2) \) is \( x^2y \) (with \( \gamma_{1,2} = (2, 1, 0) \) in the proof's notation). The proof starts by introducing:

\[ g' = f - x^{\delta - \gamma_{1,2}} S_{1,2} = f - zS_{1,2} = 2xz \cdot g_2 - x \cdot g_3 \]

As expected by the lemma's proof, only a linear combination of the last two polynomials remains. The least common multiple between \( td(g_2) \) and \( td(g_3) \) is \( xyz \) (with \( \gamma_{2,3} = (1, 1, 1) \) in the proof's notation). We calculate:

\[ g' = g - x^{\delta - \gamma_{2,3}} S_{2,3} = g - x \cdot S_{2,3} = 0 \]

By tracing back the equations, we have found the decomposition of \( f \) in terms of the syzygy polynomials:

\[ f = zS_{1,2} + xS_{2,3} \]
