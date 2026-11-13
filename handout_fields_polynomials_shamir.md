# Handout: Fields, Polynomials, and Shamir Secret Sharing

---

## 2.10 Rings, Quotient Rings, Polynomial Rings, and Finite Fields

> Note to the Reader: This section covers topics typically from introductory abstract algebra. The most important topics for cryptographic applications are the theory of finite fields of prime power order (used in elliptic curve cryptography) and quotients of polynomial rings (used in lattice-based NTRU). You may wish to skim this on first reading and return when needed.

---

### 2.10.1 An Overview of the Theory of Rings

Groups are fundamental objects in mathematics — a group G is a set with one operation. A **ring** is a set with *two* operations (analogous to addition and multiplication), linked by the distributive law.

**Definition.** A *ring* is a set R with two operations + and ★ satisfying:

**Properties of +**
- *[Identity Law]* There is an additive identity 0 ∈ R such that 0 + a = a + 0 = a for every a ∈ R.
- *[Inverse Law]* For every a ∈ R there is an additive inverse b ∈ R such that a + b = b + a = 0.
- *[Associative Law]* a + (b + c) = (a + b) + c for all a, b, c ∈ R.
- *[Commutative Law]* a + b = b + a for all a, b ∈ R.

Briefly: R with only + is a commutative group with (additive) identity 0.

**Properties of ★**
- *[Identity Law]* There is a multiplicative identity 1 ∈ R such that 1 ★ a = a ★ 1 = a for every a ∈ R.
- *[Associative Law]* a ★ (b ★ c) = (a ★ b) ★ c for all a, b, c ∈ R.
- *[Commutative Law]* a ★ b = b ★ a for all a, b ∈ R.

R with only ★ is *almost* a commutative group with identity 1, except elements are not required to have multiplicative inverses.

**Property Linking + and ★**
- *[Distributive Law]* a ★ (b + c) = a ★ b + a ★ c for all a, b, c ∈ R.

> *Remark 2.38.* Our rings are really *commutative rings with (multiplicative) identity*. Every element of a ring has an additive inverse, but many nonzero elements may lack multiplicative inverses (e.g., in ℤ, only 1 and −1 have multiplicative inverses).

**Definition.** A (commutative) ring in which every nonzero element has a multiplicative inverse is called a **field**.

**Example 2.39.** Examples of rings and fields:

(a) R = ℚ, with usual + and ×. Every nonzero element has a multiplicative inverse, so **ℚ is a field**.

(b) R = ℤ, with usual + and ×. Only ±1 have multiplicative inverses, so **ℤ is a ring but not a field**.

(c) R = ℤ/nℤ, n any positive integer, with usual + and ×. The multiplicative identity is 1.

(d) R = 𝔽_p, p any prime, with usual + and ×. By Proposition 1.22, every nonzero element has a multiplicative inverse, so **𝔽_p is a field**.

(e) The collection of all polynomials with coefficients from ℤ forms a ring under polynomial addition and multiplication, denoted ℤ[x]:
$$\mathbb{Z}[x] = \{a_0 + a_1 x + a_2 x^2 + \cdots + a_n x^n : n \geq 0,\ a_0, a_1, \ldots, a_n \in \mathbb{Z}\}.$$

(f) More generally, if R is any ring, we can form R[x] — the ring of polynomials with coefficients from R (e.g., ℤ/qℤ or 𝔽_p).

---

### 2.10.2 Divisibility and Quotient Rings

**Definition.** Let a and b be elements of a ring R with b ≠ 0. We say that **b divides a** (written b | a) if there is an element c ∈ R such that a = b ★ c.

> *Remark 2.40.* Basic divisibility properties from ℤ apply to rings in general. However, not every ring behaves like ℤ. For example, in ℤ/6ℤ, we have 2 ≠ 0 and 3 ≠ 0, but 2 · 3 = 6 = 0.

**Definition.** Let R be a ring. An element u ∈ R is called a **unit** if it has a multiplicative inverse, i.e., if there is v ∈ R such that u ★ v = 1.

An element a of a ring R is said to be **irreducible** if a is not itself a unit and if in every factorization a = b ★ c, either b is a unit or c is a unit.

> *Remark 2.41.* The integers have the unique factorization property: every integer factors uniquely into irreducible integers (primes), up to reordering and factors of ±1. Not every ring has this property, but polynomial rings over fields do (see Section 2.10.3).

**Definition (Congruence in a ring).** Let R be a ring and m ∈ R with m ≠ 0. Two elements a and b of R are **congruent modulo m** if their difference a − b is divisible by m. We write:
$$a \equiv b \pmod{m}.$$

Congruences in arbitrary rings satisfy the same equation-like properties as in ℤ.

**Proposition 2.42.** Let R be a ring and m ∈ R with m ≠ 0. If a₁ ≡ a₂ (mod m) and b₁ ≡ b₂ (mod m), then:
$$a_1 \pm b_1 \equiv a_2 \pm b_2 \pmod{m} \quad \text{and} \quad a_1 \star b_1 \equiv a_2 \star b_2 \pmod{m}.$$

**Definition (Congruence class / Quotient ring).** For a ∈ R, the **congruence class of a** is the set:
$$\bar{a} = \{a' \in R : a' \equiv a \pmod{m}\}.$$

The collection of all congruence classes is denoted R/(m) or R/mR:
$$R/(m) = R/mR = \{\bar{a} : a \in R\}.$$

We add and multiply congruence classes by:
$$\bar{a} + \bar{b} = \overline{a+b} \quad \text{and} \quad \bar{a} \star \bar{b} = \overline{a \star b}.$$

We call R/(m) the **quotient ring of R by m**.

**Proposition 2.44.** The formulas above give well-defined addition and multiplication rules on R/(m), making R/(m) into a ring.

---

### 2.10.3 Polynomial Rings and the Euclidean Algorithm

For a ring R, the **polynomial ring** R[x] is:
$$R[x] = \{a_0 + a_1 x + a_2 x^2 + \cdots + a_n x^n : n \geq 0,\ a_0, \ldots, a_n \in R\}.$$

The **degree** of a nonzero polynomial a(x) = a₀ + a₁x + ⋯ + aₙxⁿ (with aₙ ≠ 0) is n, written deg(a). The leading coefficient is aₙ. A polynomial with leading coefficient 1 is called a **monic polynomial**.

Especially important are polynomial rings where R is a field 𝔽. The ring 𝔽[x] behaves very much like ℤ.

**Proposition 2.45 (Division with remainder in 𝔽[x]).** Let 𝔽 be a field and let **a** and **b** be polynomials in 𝔽[x] with **b** ≠ 0. Then it is possible to write:
$$\mathbf{a} = \mathbf{b} \cdot \mathbf{k} + \mathbf{r}$$
with **k** and **r** polynomials, and either **r** = 0 or deg **r** < deg **b**.

We say **a** divided by **b** has quotient **k** and remainder **r**.

Rings that have a "division with remainder" algorithm are called **Euclidean rings**.

**Example 2.46.** The GCD of x² − 1 and x³ + 1 is x + 1, since:
- x² − 1 = (x + 1)(x − 1)
- x³ + 1 = (x + 1)(x² − x + 1)

**Proposition 2.47 (Extended Euclidean Algorithm for 𝔽[x]).** Let 𝔽 be a field and let **a** and **b** be polynomials in 𝔽[x] with **b** ≠ 0. Then the GCD **d** of **a** and **b** exists, and there are polynomials **u** and **v** in 𝔽[x] such that:
$$\mathbf{a} \cdot \mathbf{u} + \mathbf{b} \cdot \mathbf{v} = \mathbf{d}.$$

**Example 2.48.** Use the Euclidean algorithm in 𝔽₁₃[x] to compute gcd(x⁵ − 1, x³ + 2x − 3):

```
x⁵ − 1 = (x³ + 2x − 3)(x² + 11) + (3x² + 4x + 6)
x³ + 2x − 3 = (3x² + 4x + 6)(9x + 1) + (9x + 4)    ← gcd = 9x + 4
3x² + 4x + 6 = (9x + 4)(9x + 8) + 0
```

Thus 9x + 4 is a GCD. To get a monic polynomial, multiply by 3 ≡ 9⁻¹ (mod 13):
$$\gcd(x^5 - 1,\ x^3 + 2x - 3) = x - 1 \quad \text{in } \mathbb{F}_{13}[x].$$

**Proposition 2.50 (Unique Factorization in 𝔽[x]).** Let 𝔽 be a field. Then every nonzero polynomial in 𝔽[x] can be uniquely factored as a product of monic irreducible polynomials (up to reordering and constant multiples).

---

### 2.10.4 Quotients of Polynomial Rings and Finite Fields of Prime Power Order

Just as we form ℤ/qℤ from ℤ, we can form 𝔽[x]/(m) from a polynomial ring 𝔽[x].

**Proposition 2.51.** Let 𝔽 be a field and let m ∈ 𝔽[x] be a nonzero polynomial. Then every nonzero congruence class $\bar{a} \in \mathbb{F}[x]/(\mathbf{m})$ has a unique representative **r** satisfying:
$$\deg \mathbf{r} < \deg \mathbf{m} \quad \text{and} \quad \mathbf{a} \equiv \mathbf{r} \pmod{\mathbf{m}}.$$

**Example 2.52.** Consider the ring 𝔽[x]/(x² + 1). Every element is uniquely represented by a polynomial of the form:
$$\overline{\alpha + \beta x} \quad \text{with } \alpha, \beta \in \mathbb{F}.$$

Addition:
$$\overline{\alpha_1 + \beta_1 x} + \overline{\alpha_2 + \beta_2 x} = \overline{(\alpha_1 + \alpha_2) + (\beta_1 + \beta_2)x}.$$

---

## 8.4 Secret Sharing Schemes

A **secret sharing scheme** provides a way to share a secret among several people such that any subgroup of n − 1 people cannot gain an advantage in discovering the secret.

**Simple scheme:** To share a secret number S mod m among n people, select n − 1 random numbers D₁, D₂, ..., D_{n−1} modulo m and set:
$$D_n \equiv S - D_1 - D_2 - \cdots - D_{n-1} \pmod{m}.$$

The i-th participant receives D_i. Recovery requires all n values:
$$S \equiv D_1 + D_2 + \cdots + D_n \pmod{m}.$$

### Shamir's Secret Sharing Scheme (t, n)-threshold

A **(t, n) threshold sharing scheme**: n participants, any t can recover the secret, but no t − 1 can.

**Key idea:** It takes k + 1 values to determine a polynomial of degree k.
- A linear polynomial ax + b is determined by 2 values (a line needs 2 points).
- A quadratic ax² + bx + c is determined by 3 values.
- Etc.

**Setup:**
- Want to share secret S among n people so that any t can recover S, but fewer than t cannot.
- Set a₀ = S, choose random numbers a₁, a₂, ..., a_{t−1}, and form the polynomial:
$$f(x) = a_0 + a_1 x + a_2 x^2 + \cdots + a_{t-1} x^{t-1}.$$
- Choose n random values for x, say x₁, x₂, ..., xₙ, and compute:
$$y_i = f(x_i) \quad \text{for } 1 \leq i \leq n.$$
- The i-th participant is given the value y_i (in practice, often x_i = i).

**Recovery:**
Suppose t participants (say participants 1 through t) want to recover S. They share their y_i values and form the system:

$$y_1 = a_0 + a_1 x_1 + a_2 x_1^2 + \cdots + a_{t-1} x_1^{t-1}$$
$$y_2 = a_0 + a_1 x_2 + a_2 x_2^2 + \cdots + a_{t-1} x_2^{t-1}$$
$$\vdots$$
$$y_t = a_0 + a_1 x_t + a_2 x_t^2 + \cdots + a_{t-1} x_t^{t-1}$$

This is t linear equations in t unknowns (a₀, a₁, ..., a_{t−1}). Solve via Gaussian elimination (or Lagrange interpolation) to find a₀ = S.

> In practice, a more efficient reconstruction method uses **Lagrange interpolation polynomials**.

---

## Course Handout: Shamir Secret Sharing (Math 3320/5320)

**Problem:** We have n people and want any k of them to be able to launch a missile attack, but no k − 1 of them should have this power.

**Goal:** Divide a secret S into n pieces S₁, ..., Sₙ such that:
1. Knowledge of any k of the Sᵢ makes S easy to compute.
2. Knowledge of any k − 1 of the Sᵢ gives **no information whatsoever** about S.

This is called a **(perfect) (k, n)-threshold scheme**.

### Setup

- Assume S is a positive integer.
- Choose a prime p > max(S, n). All participants know n, k, and p.
- Let a₀ = S, choose integers a₁, ..., a_{k−1} randomly from {0, 1, ..., p−1}.
- Form the polynomial in 𝔽_p:
$$F(T) = a_0 + a_1 T + \cdots + a_{k-1} T^{k-1} \in \mathbb{F}_p.$$
- For i = 1, ..., n, let **Sᵢ = F(i) ∈ 𝔽_p**. The i-th participant receives Sᵢ.

### Recovery via Lagrange Interpolation

Given any k participants with x-values x₁, ..., x_k and shares y₁ = S_{x₁}, ..., y_k = S_{x_k}, reconstruct S = F(0) using:

$$S \equiv \sum_{i=1}^{k} y_i \left( \prod_{j \neq i} x_j (x_j - x_i)^{-1} \right) \pmod{p}.$$

**Security:** Knowledge of only k − 1 pieces gives absolutely no information about S, because the remaining coefficients were chosen at random.

---

## Course Handout: RSA Digital Signatures (Math 3320/5320)

### Version 1 (Public message, signed)

Bob has a public document that Alice agrees to sign. Let m be the document (or a hash of it).

- Alice's **public key**: (n, e)
- Alice's **private key**: d

**Procedure:**
1. Alice *signs* by computing: **s ≡ m^d (mod n)**. She sends s to Bob.
2. Bob *verifies* by computing: **s^e (mod n)**. If the result equals m, the signature is valid.

**Remarks:**
- To forge Alice's signature on a given message m, Fred needs s such that s^e ≡ m (mod n) — this is equivalent to breaking RSA decryption.
- The signature depends on the message contents.

### Version 2 (Encrypted message with signature)

Alice wants to send a **secret** message m to Bob with a signature attached.

- Alice's keys: public (n_A, e_A), private d_A
- Bob's keys: public (n_B, e_B), private d_B

**Procedure:**
1. Alice computes **u ≡ m^{d_A} (mod n_A)** (signs with her private key).
2. Alice computes **v ≡ u^{e_B} (mod n_B)** and sends v to Bob (encrypts with Bob's public key).
3. Bob recovers u as **v^{d_B} (mod n_B)** (decrypts with his private key).
4. Bob recovers m as **u^{e_A} (mod n_A)** (verifies with Alice's public key).

**Remarks:**
- Bob can verify the message came from Alice because only Alice could have performed step 1.
- **Weakness of both versions:** Alice can deny having signed the document, claiming her private key was compromised.
