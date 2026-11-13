Chapter 11

Topics in Algebra and Number
Theory
Public-key cryptography relies for security on the (supposed) intractability of various number-theoretic problems. The integer factorization problem (FACTOR) and
the discrete logarithm problem (DLP) are well-known examples:
FACTOR: Find the prime factorization of a given positive integer n.
DLP: Given prime p, generator a of Z~, and ax mod p, find x.
To be slightly more precise, public-key schemes require two (related) problems,
one which is computationally feasible, and one which is intractable. In the case of
integer factorization, for example, it is relatively easy to find the product n = pq;
efficient recovery of p and q given n in the general case remains an open problem.
Four topics are considered in the following sections: quadratic residues, primality testing, integer factorization, and discrete logarithms. Clearly, the coverage
will not be exhaustive, and the goal is to consider a respectable sampling of material from algebra and number theory (and algorithmic number theory) fundamental
to public-key schemes.

11.1

Algorithms, complexity, and modular arithmetic

Several algorithms involving arithmetic operations on integers are presented in
this section. One measure of efficiency is a count of the number of operations
involving single-bit arithmetic. For example, if x and y are k-bit binary numbers,
then x + y may be done in at most k bit operations, and the usual approach to
calculating xy requires at most k 2 bit operations (there are at most k- 1 additions,
each requiring at most k bit operations). These are "worst-case" counts, over all
integers with at most k bits. We can regard the number of bit operations as a
measure of the running time, in terms of the "size" k of the input, required by an
algorithm.

253

254

11 Topics in Algebra and Number Theory

The running times in terms of the input size are often written using the following notation. Given sequences f and g defined on the positive integers, we say
that f is "big-oh" of g, written f = O(g), in case there exist numbers c and no so
that lf(n)l :S clg(n)l for all n 2: no. For example, f(n) = 3n 4 + 7n -1 = O(n 4 ),
logn = O(nt) for any t > 0, and f = 0(1) means that 1/1 is bounded above by a
constant. In the examples of addition and multiplication of k-bit binary numbers,
the bit complexity may be said to be O(k) and O(k 2 ), respectively. Note that bigoh notation is about asymptotic upper bounds; the statement f(n) = 0(2n) does
not preclude the possibility that f(n) = O(n 2 ), a much slower-growing bound.
An algorithm for arithmetic on k-bit binary numbers x and y is said to be
polynomial-time if the worst-case running time is bounded by a polynomial in the
size k of the input; i.e., the running time is O(kt) for some integer t. Roughly
speaking, such algorithms will be considered efficient, although it is important
to note that it is possible for a polynomial-time algorithm to be slower than an
exponential-time algorithm for all input values of interest.
If x and y are integers with 0 :S x, y :S n, then the size of the input to the
algorithm may be taken to be the number of bits k = Llog 2 n J + 1 in the binary
representation of n. In the big-oh notation, we may use the simpler expression
k = log 2 n. In particular, the algorithm is efficient if the running time is O(kt) not
O(nt).
The integers

Let a, bE Z, the set of integers {0, ±1, ±2, ... }. Then a divides b, written a I b, if
there is an integer c such that b = ac. As examples, -3 115 since 15 = (-3)( -5),
and every integer divides 0. If b has a divisor a lit {± 1, ±b}, then a is said to be
nontrivial. An integer a 2: 2 is prime if it has no nontrivial divisors; otherwise,
a is composite. The prime number theorem gives an asymptotic estimate on the
number of primes n(x) in the interval [2, x] as n(x) "'x jlogx. The estimate is in
fact a lower bound; for example, with x = 103 , n(x) = 168 > xjlogx ~ 144.8.
It is routine (Exercise 11.1.9) to verify the following properties: if a I b and
b I c then a I c; if c is a common divisor of a and b in the sense that c I a and
c I b, then c I ax+ by for every x, y E Z; if p is prime and p I ab, then p I a or
p I b. A common divisor d 2: 0 of a and b is the greatest common divisor, written
d = gcd(a, b) or more simply d = (a, b), if c I d for all common divisors c of a
and b. The Euclidean algorithm discussed below shows that the greatest common
divisor exists. The fundamental theorem of arithmetic (every integer a 2: 2 can
be written uniquely, up to ordering, as a product of primes) can be used to find
common divisors; for example, 36 = 22 · 32 , 24 = 23 · 3, and (36, 24) = 22 · 3.
Exhibiting the prime factorization of a given integer is believed to be a difficult problem; however, it is possible to find the greatest common divisor without
factoring. First, recall the division algorithm: given integers a and b 2: 1, then

11.1 Algorithms, complexity, and modular arithmetic

255

long division may be used to write a = qb + r with 0 :::= r < b. The integers
q (the quotient) and r (the remainder, denoted r =a mod b) are unique. The
Euclidean algorithm for finding the greatest common divisor uses the fact that
(a, b)= (b,a mod b) if a> b > 0.
Algorithm 11.1.1 (Euclidean algorithm for greatest common divisor) Input: integers a :::: b :::: 0. Output: the greatest common divisor (a, b) of a and b.
1. Set ro = a and TI = b.
2. Determine the first n:::: 0 so that rn+I = 0, where ri+I = ri-I mod ri; i.e.,
ri+I is defined by ri-l = qi+ITi +ri+I obtained from the division algorithm.
3. Return (rn).
It is clear that the process will terminate, since 0 :::= ri+I < ri fori > 0. In
fact, ri+2 < r; j2, so that there are at most 1 + 2log2 a divisions. Each division
takes 0 (log~ a) bit operations, so the running time of the Euclidean algorithm is
0 (log~ a) bit operations. A more careful analysis shows that the time is actually
0 (log~ a); in any case, the Euclidean algorithm is efficient. (Exercise 11.1.11
verifies that the value returned is indeed (a, b).)

Example 11.1.2 We apply the algorithm to calculate (299, 221). The long divisions may be written:
299 = 1 . 221 + 78

(q2 = 1, r2 = 78)

221 =2·78+65

(q3 = 2, r3 = 65)

78 = 1· 65 + 13

(q4 = 1, r4 = 13)

65 = 5·13+0

(qs = 5, rs = 0)

and (299, 221) = r4 = 13.
The Euclidean algorithm may be written somewhat more concisely; however,
the present form is easily extended to provide integers x and y so that ax +by =
(a,b). The basic idea is to walk through the long divisions, beginning at the
bottom (Appendix A presents this more formally). The first two steps are:
(a, b)= rn = rn-2- qnrn-I

= rn-2- qn(rn-3- qn-lTn-2)
= (1

+ qnqn-I)rn-2- qnrn-3

obtaining (a, b) as a linear combination of rn-2 and rn-3· The process, known as
the extended Euclidean algorithm, eventually obtains (a, b) as a linear combination of ro =a and r1 =b. In the example,
(299, 221) = 13 = 78-65
= 78- (221- 2. 78) = 3. 78-221
= 3(299-1·221)-221 = 3·299-4·221

and (a= 299,b = 221) = 13 = 3a-4b.

11 Topics in Algebra and Number Theory

256

If (a, b)= 1, then a and bare said to be relatively prime. For n ~ 1, the
number of integers in the interval [1, n] which are relatively prime to n is denoted
¢(n), known as the Euler phi function. For example, ¢(6) = 2 and ¢(pi)=
pi- 1 (p- 1) if p is prime. The phi function is multiplicative in the sense that
¢(ab) = ¢(a)¢(b) if (a, b)= 1; in particular, ¢(pq) = (p -1)(q -1) for primes
p f. q.

The integers modnlo n

Let n be a positive integer. Then a is congruent to b modulo n, written a = b
(mod n), ifnI a -b. As examples, 14 9 (mod 5), -11 3 (mod 7), and -1
n- 1 (mod n). Congruence is an equivalence relation on Z; that is, congruence
is reflexive (a= a (mod n)), symmetric (if a= b (mod n) then b =a (mod n)),
and transitive (if a= b (mod n) and b = c (mod n) then a= c (mod n)). If a=
qn + r with 0::::; r < n, then a= r (mod n). Hence every integer a is congruent to
a unique integer in [0, n- 1], namely r =a mod n. An equivalence (or residue)
class [a] consists of all integers congruent to a modulo n; the set of equivalence
classes modulo n is denoted Zn.
It can be shown that the operations of addition and multiplication on Zn given
by [a]+ [b] = [a+ b] and [a][b] = [ab ], respectively, are well-defined; that is, if
a= a' (mod n) and b = b' (mod n) then a+ b =a'+ b' (mod n) and ab = a'b'
(mod n). Under these definitions of addition and multiplication, (Zn, +, ·) is a
ring (see Exercise 11.1.12). The class notation is commonly omitted, and the set
of equivalence classes is written Zn = {0, ... , n- 1} with the understanding that
an integer a is identified with its equivalence class [a].
Given a E Zn, if there exists x E Zn such that ax = 1 (mod n), then a is said
to be invertible and its inverse x is denoted a- 1 . For example, in Z9, 2- 1 = 5
since 2. 5 = 1 (mod 9); the elements in {0, 3, 6} do not have inverses. If a E Zn
is invertible, then ax = 1 (mod n) and n I ax- 1 so that ax- ny = 1 for some
integer y, and (a, n) must be 1. Conversely, if (a, n) = 1, then x andy exist by the
extended Euclidean algorithm. Hence, a E Zn is invertible if and only if (a, n) = 1,
in which case the inverse may be found by the extended Euclidean algorithm.

=

=

=

Example 11.1.3 Let a= 7 and n = 9. The first column uses the Euclidean algorithm to show that (7, 9) = 1, and hence a is invertible in Z9. The second column
finds integers x andy so that xa + yn = (a, n).
Euclidean algorithm
to find (a, n)
9=1·7+2

extended Euclidean algorithm
to write (a, n) = xa + yn
1=7-3·2

7=3·2+1

= 7 - 3 (9 - 1 . 7) = 4 . 7 - 3 . 9

2=2·1+0

Hence, 7- 1 = 4, which can be verified by 7 · 4

=28 =1 (mod 9).

11.1 Algorithms, complexity, and modular arithmetic

257

The set z~ = {a E Zn I (a' n) = 1} of invertible elements of Zn forms a group
under multiplication, containing ¢ (n) elements. For example, Zh = { 1, 5, 7, 11}
and
= {1, ... , p -1} if pis prime. For prime panda E
Fermat's (little)
theorem says ap-l = 1 (mod p); a generalization due to Euler shows a<f>(n) = 1
(mod n) for integer n :::: 2 and a E Z~. In particular, we may define the order of
a E Z~, written ord(a), as the least positive integer t such that a 1 = 1 (mod n).
Exercise 11.1.21 establishes some elementary properties concerning Z~; in particular, ord(a) 1¢(n) for a E Z~. If a E Z~ has ord(a) = IZ~I = ¢(n), then a is said
to be a generator of z~; in this case, z~ = {a; 1 o : : : i < ¢ (n)}. It is known that z;
for p prime has a generator; for example, it is easily verified that 2 is a generator

z;

z;,

ofZh.

Zt

Example11.1.4 Consider
5 = {1,2,4,7,8,11,13,14}, having ¢(15)
¢(3)¢(5) = (3 -1)(5- 1) = 8 elements. The orders of the elements are:
a E Z*
ord(a)

1

2
4

4
2

7
4

8
4

11
2

13
4

14
2

As promised, the order of every element divides ¢(n) = 8. Note that
have a generator (since no element has order 8).

Zt5 does not

Theorem 11.1.5 (Chinese remainder) If the integers n 1, ... , nk are pairwise relatively prime, then the system of congruences
x = a1

(mod n1)

has a unique solution modulo n = n 1 · · · nk.

Consider the case k = 2 in the Chinese remainder theorem. Since (n 1, n2) =
1, there exist integers s and t so that sn 1 + tn2 = 1. The verification that x =
(sn 1a2+tn2a1) modn is the solution to the system is Exercise 11.1.18. As a
specific example, consider
x =3

(mod 7),

x =6

(mod 13).

The extended Euclidean algorithm gives (7, 13) = 1 = 2 · 7 - 1 · 13, and then x =
2 · 7 · 6- 1 · 13 · 3 = 45 is the unique solution (modulo n = 7 · 13 = 91) to the pair
of congruences. The generalization is Gauss' algorithm.
Algorithm 11.1.6 (Gauss) The solution x to the system in the Chinese remainder
theorem may be computed as x = Z::1=l a;N;M; mod n, where N; = njn; and
M; = N;-I mod n;. The computations can be performed in O(log~n) bit operations.

258

11 Topics in Algebra and Number Theory

Table 11.1: Bit complexity of basic operations in Zn.
modular operation
addition
(a+b) mod n
multiplication
ab mod n
a- 1 mod n
inversion
exponentiation ak mod n, k < n

bit complexity
O(log 2 n)
O((log2 n) 2)
O((log2 n) 2)
O((log2 n) 3)

Efficient calculation of ak mod n is essential in several cryptographic schemes.
The calculation can be done na"iyely by computing ak and dividing by n or by
computing ai mod n, I :S i :S k, by repeated multiplication; unfortunately, these
calculations are inefficient in terms of storage or number of multiplications required. The method of repeated square-and-multiply provides an efficient calculation. The exponent is written in binary ask= L~=O ki2i, where t = Llog 2 kj,
and then

n

n

i=O

ki=l

t

ak =

aki2i = (a2o)ko ... (a2' )k' =

a2i.

Note that a 2' = (a 2'- 1 ) 2 , and hence the value ak mod n can be calculated with t
modular squarings and at most t modular multiplications. We present an algorithm
for modular exponentiation which proceeds by calculating the partial products;
other square-and-multiply algorithms may be found in [63]. Bit complexities for
the basic operations in Zn appear in Table 11.1.
Algorithm 11.1.7 (exponentiation by square-and-multiply) Input: 0 f. a E Zn,
and integer 0 :S k < n with binary representation k = L~=O ki 2i. Output: ak mod

n.
1. Set A *- a and b *- 1.

2. Fori from 0 to t do the following:
2.1 If i > 0 then set A *- A 2 mod n.
2.2 If ki = 1 then set b *- bA mod n.
3. Return(b).
Example 11.1.8 The algorithm is applied to 326 mod 35, i.e., a= 3, k = 26 =
Lf=oki2i = 110102, and n = 35. The arithmetic can be collected in tabular form
as follows:
i
ki
A

b

0
0
3

1
1
32 mod n = 9
1·9 mod n = 9

2
0
92 mod n = 11
9

3
1
11 2 mod n = 16
9·16 mod n =4

4
1
162 mod n = 11
4·11 modn =9

11.1 Algorithms, complexity, and modular arithmetic

259

and hence 326 mod 35 = 9. In this particular example, we may quickly verify by
reducing the exponent modulo ¢ (n) = ¢ (5 · 7) = 24, obtaining 3k mod n = 32 .
Exercises
11.1.9 Verify the following properties of divisibility: (a) a I a; (b) if a I band b I c,
then a I c; (c) if a I band b I a, then a= ±b; (d) if c I a and c I b, then c I ax+ by
for all x, y.

11.1.10 Define the least common multiple lcm(a, b) of positive integers a and b
by lcm(a, b)= abj(a, b). Show that lcm(a, b) I c whenever a I c and b I c.
11.1.11 This question concerns the Euclidean algorithm 11.1.1.
(a) Show that the remainders satisfy r;+2 < r; /2.
(b) Show that the algorithm returns (a, b).
11.1.12 This exercise examines elementary properties of Zn.
(a) Show that the operations of addition and multiplication on Zn are welldefined in the sense described in the text.
(b) The set Zn together with the defined addition and multiplication forms a
ring, denoted (Zn, +, ·). The ring properties follow directly from corresponding properties in Z. Verify that multiplication distributes over addition;
i.e., ([a]+ [b])[c] = [a][c] + [b][c].
11.1.13 Use the Euclidean algorithm to find d = (105, 180). Then find integers x
andy such that 105x + 180y =d.
11.1.14 Use the square-and-multiply algorithm to compute 47 332 mod 576.
11.1.15 The following statements are false in general. Give a counterexample to
each statement.
(a) Leta,b,n E Z. IfnI ab then n I a or nIb.
(b) Let p be a positive integer and a E Z. If (a, p) = 1 then aP- 1 = 1 (mod p).
(c) If a, b, c E Z, then (ab, c)= (a, c)(b, c).
11.1.16 Find the order of each element of Zj 1 . Identify the generators of Zj 1 .
11.1.17 Let a E Z~. Prove that the elements ai mod n, 0.:::; i < ord(a), are pairwise distinct.
11.1.18 Verify the claim made in the paragraph following the Chinese remainder theorem 11.1.5 that x = (sn1a2 +tn2a1) mod n (where sn1 +tn2 = 1) is the
solution to the system of two congruences.
11.1.19 Determine if the pair of congruences
x = 15

(mod 70)

x = 104

(mod 151)

has a solution. If so, use the extended Euclidean algorithm to find all solutions.

11 Topics in Algebra and Number Theory

260

11.1.20 Prove that the congruence ax = b (mod n) has a solution x if and only
if (a, n) I b, in which case there are exactly (a, n) solutions.
11.1.21 Let a E Z~. Prove the following claims:
(a) ax= 1 (mod n) if and only if ord(a) I x. In particular, ord(a) I ¢(n).
(b) ax= aY (mod n) if and only if x = y (mod ord(a)).
(c) ax mod n = axmodord(a) mod n.
11.1.22 Let p be prime and let a :::: 1 be an integer. Prove that the number of
solutions in Zp to xa = 1 (mod p) is (a, p -1).
11.1.23 If pis prime, then every nonzero element of Zp has an inverse and Zp
is a field. Fermat's little theorem says that ap-l = 1 (mod p) for a E z;. An
elementary proof may be given as follows.
(a) LetT= {a, 2a, ... , (p -1)a} <; Zp. Show that the elements ofT are nonzero
and pairwise distinct.
(b) Part (a) shows that T = z;. Hence 1 · 2 · · · (p- 1) = a(2a) · · · (p- 1)a
(mod p). Use division to complete the proof.

11.2

Quadratic residues

Let a E z~. Then a is a quadratic residue modulo n if there exists X E z~ with
x 2 =a (mod n). In this case, xis a square root of a modulo n. Otherwise, a is a
quadratic nonresidue modulo n.
The set of quadratic residues modulo n is denoted by Qn; the nonresidues are
Qn. Note that Z~ = Qn U Qn· There are elements a E Zn \ Z~ for which x 2 =a
(mod n) has a solution, but a is not a quadratic residue; for example, 4 E 1:6 has
22 = 4 (mod 6) but 4 ¢: Q6 (since 4 ¢: Z(;).
Theorem 11.2.1 Let p > 2 be prime and let a be a generator ofz;. Then a E z;
is a quadratic residue modulo p if and only if a = a 2i (mod p) for some i E Z.
Proof: If a = a 2i mod p for some i, then x = ai satisfies x 2 =a (mod p) and a
is a quadratic residue. Conversely, if a E QP, then x 2 =a (mod p) has a solution
x = ai (mod p) for some i, and a 2 i =a (mod p).
D
Corollary 11.2.2 Let p > 2 be prime and let a be a generator of z;.
1. QP = {ai mod p I i even, 0 ::::: i ::::: p - 2}, QP = {ai mod p I i odd, 0 ::::: i :::::
p-2}.
--

-p-i

2. IQpi-IQpl- -2-.
3. If a E QP, then x 2 =a (mod p) has exactly two solutions.
p-1

4. a-z = -1 (mod p).

11.2 Quadratic residues

261

Proof: The first two are immediate consequences of the theorem. The third follows since there are at least two solutions ±x by definition of a quadratic residue,
and statement 2 of this corollary shows that there cannot be more than two solutions. For the fourth, note that a<P- 1ll 2 is a solution of x 2 = 1 (mod p) since the
order of a is p- 1. Necessarily, a<P- 1l/ 2 = ±1 (mod p), and only the congruence in 4 is compatible with the order of a.
0

z;,

For a given a E
an exhaustive search through the elements {x 2 mod p 1
x E z;} will, of course, determine if a is a quadratic residue. This is not efficient,
requiring 0 (p) modular multiplications in the worst case. The corollary can be
used to develop a more efficient criterion. Define the Legendre symbol ( ~) for
prime p > 2 and integer a by

(~) =
p

I

0,

1,
-1,

pI a,
a mod p E Qp,
a mod p E QP.

Theorem 11.2.3 (Euler's criterion) Let p > 2 be prime and a E Z. Then(~)=
p-I

aT"

(mod p).

Proof: We consider the case that a E Qp; the cases a E QP and p I a are left
as Exercise 11.2.15. Let a be a generator of
Then there exists i such that
2
a i =a (mod p) by Theorem 11.2.1. By definition, (-"-) = 1, and a<p-l)/ 2 =
(a2i)(p-1)/2 = (aP-1)i = 1 (mod p).
P
0

z;.

Euler's criterion suggests using aPz 1 to determine if a given a is a quadratic
residue modulo the prime p. This can be done efficiently (O(log~p) bit operations) using a square-and-multiply approach. However, unlike exhaustive search,
it will not exhibit a square root in the case a E QP.

Example 11.2.4 Euler's criterion can be used to determine if 3 is in QP for p =
23. To calculate 3(P- 1l/ 2 mod p, write
p-1
I: ki2'..
2- = 11 = 10112 =
3

i=O

Following Algorithm 11.1.7, the table for the arithmetic is
ki
A
b

0
1
3
3

1
32 =9
3·9 mod p =4

2
0
92 mod p = 12
4

3
1
122 mod p = 6
4·6 mod p = 1

and {t) = 3 P2 1 mod p = 1. This shows that 3 E Q23, and hence there is an x
with x 2 = 3 (mod 23). Later, we will have an efficient algorithm for exhibiting
such an x. In this toy example, it is easily verified that 7 2 = 3 (mod 23).

11 Topics in Algebra and Number Theory

262

Properties of the Legendre symbol can be used to calculate ( £) more efficiently than the method suggested by the example. We shall return td'this question
after developing an extension of the Legendre symbol.
Let n :::: 3 be an odd integer with prime factorization n = p~ 1 · · · p~k, and let
a E Z. The Jacobi symbol ( ~) is defined in terms of the Legendre symbol by

Properties of the Jacobi symbol Let m, n :::: 3 be odd integers, and let a, bE Z.

1. (~) E {-1,0, 1}, and(~) =Oifandonlyif(a,n) -=f=.l.
2. (a,;'}=(~)(*) and

C:n) = (~) (~).

3. If a= b (mod n) then(~)=(*)·

-1:

.
n 1
{ 1
4 · (*)=land(~ 1 )=(-l)T=

5

·

(-n2 )=(-l)n28-1={_I1,

n = 1 (mod 4),
n=3 (mod4).

n=±1 (mod8),
n = ±3 (mod 8).

6. (law of quadratic reciprocity) ( ';) = ( ~) ( -1) m2 1n2 1.
Applying these properties to the calculation of ( ~) in Example 11.2.4, we
obtain
3) = (23)
( 23
3 (-1) 3-123-1
2 2 = (2)
3 (-1) 1·11 = - (2)
3 = - (-1) = 1.
Since 23 is prime, this establishes that 3 E Q23, as seen earlier using Euler's criterion.
The definition of the Jacobi symbol (~) suggests that n must be factored,
which is believed to be a hard problem. However, the properties can be used to
evaluate the symbol without factoring.
Example 11.2.5 Properties of the Jacobi symbol may be used to evaluate
follows:

G~) = (525) 2 (;5)

nn as

(property 2)

55)
55-17-1
= ((-l)-2---r
7

(property 6)

(property 3)
=-C;) =-(~)
-1) = -(-1)--r = 1 (property 4)
=- (7
7-1

Although the Jacobi symbol is 1, we cannot conclude that 28 is a quadratic residue
modulo 55 (in fact, it isn't). Note that 28<55 - 1l/ 2 =52 (mod 55), illustrating that

11.2 Quadratic residues

263

Euler's criterion need not hold for composites. Finally, recall that a quadratic
residue modulo a prime has exactly two square roots. Here, n = 55 is not prime,
and, for example, 1 has square roots 1 and 55 - 1 and also 21 and 55 - 21. (Roots
are discussed in Section 11.4.)
As the example shows, the condition ( ~) = 1 does not distinguish quadratic
residues from nonresidues in the case of composite n. However, if a E Qn, then
there exists x E Z~ such that x 2 =a (mod n) and hence (~) = (~2 ) = (~) 2 = 1.
Stated in the contrapositive, this says that ( ~) = -1 implies that a is a quadratic
nonresidue. For odd integers n ~ 3 we define
In= {a E Z~ I(~)= 1}.
The elements of Qn = In\ Qn are called pseudosquares modulo n. Note that
Qn s; In, and In= Qn in the case that n is prime.
Example 11.2.6 The quadratic residues and pseudosquares for n = 15 are obtained. Note that ( 1~) = ( ~) (%) and

(~) ={-~: a=±2 (mod 5).

(~)={-~: a=2 (mod 3),
a=1

(mod 3),

a=±1

(mod 5),

The Jacobi symbols(~) are calculated in the following table:
a EZj5

2

(1)
(5)

-1
-1

({3)

4

7

-1
-1

8

11

13

-1
-1

-1
1
-1

-1

-1

14
-1
1
-1

Hence, l1s = {1, 2, 4, 8}. It can be verified that Q15 = {1, 4}, and then the pseudosquares are given by Q1s = hs \ Q15 = {2, 8}.
Determining if a given a E In is a quadratic residue or pseudosquare modulo
n is known as the quadratic residuosity problem (QRP). Consider the special case
n = pq for distinct odd primes p and q. It is shown in the exercises that a E Jpq
is a quadratic residue if and only if a E Qp, and that IQpql = IQpql = (p1)(q -1)/4. Applied to Example 11.2.6, (~) = 1 =(%)if and only if a E {1,4};
necessarily, Q15 = {1,4} and IQ1sl = 2 = (3 -1)(5 -1)/4.
Exercises
11.2.7 Find Q3o and Q3o·
11.2.8 Evaluate the Jacobi symbols Gj~) and

es ii). Is 156 Qm?
8

E

11.2.9 Find the quadratic residues and pseudosquares for n = 21.

11 Topics in Algebra and Number Theory

264

11.2.10 A careless colleague remarks "36 is a quadratic residue modulo n for any
n > 36 since 62 = 36." Correct the statement, and then determine if 36 is in Q745.
11.2.11 Assume that the following properties of Legendre symbol are known:
• ( ~1 ) = ( -1)(p-Il/2 and(%) = ( -l)(P 2-Il/ 8 for odd primes p.
• If p and q are odd primes, then (~) = (~) ( -1)(p-I)(q-I)/4.
Let n 2: 3 be an odd integer. Prove the following facts about the Jacobi symbol.
(a) Show thatifn1 andn2 are odd integers, then n1nri = n121 + nzzl (mod 2).
Hence prove that
= ( -1)Cn-Il/2.

c·;/)

(b) Showthatifn1 andn2 are odd integers, then
Hence prove that(~)= (-1)(n 2 -ll/ 8 .

2 2

n2

n2

n n -i
-1
-1
--Y= T+Y
(mod 2).

(c) Prove that if a 2: 3 is an odd integer, then(~) =G) ( -1)(a-I)(n-ll/4.
11.2.12 Let p be an odd prime. Prove that -3 E QP if and only if p = 1 (mod 3).
11.2.13 Prove that a quadratic residue can never be a generator of Z~.
11.2.14 Let n = pq for distinct odd primes p and q.
(a) Show that a E Qn iff(~)= 1 = (~).
(b) Show that IQn I = (p-i~(q-1). Suggestion: show that the map f : Qn --+
Q P x Qq defined by f (a) = (a mod p, a mod q) is a bijection.
11.2.15 Complete the proof of Euler's criterion (Theorem 11.2.3).

11.3 Primality testing
The generation of (large) prime numbers is a basic requirement in several publickey schemes. Determining if a given integer n > 2 is prime or composite is a
sub-problem. Trial division by all integers (or all primes) between 2 and .jii will,
of course, determine if n is prime. It also has the pleasant feature of exhibiting
a nontrivial factor in the case that n is composite; unfortunately, trial division
requires 0 (.jii) divisions.
In this section we consider two probabilistic primality tests, Solovay-Strassen
and Miller-Rabin. They are probabilistic in the sense that the test concludes either
"composite" (in which case n is certainly composite) or "probably prime" (in
which case n may actually be composite). For this reason, the tests are more
accurately called tests for compositeness.
Solovay-Strassen is built on Euler's criterion (Theorem 11.2.3): if n is prime,
then aCn-ll/ 2 = (~) (mod n). This motivates the following definition.
Definition 11.3.1 Let n be an odd composite integer and 1 .::=:a < n. If (a, n)-=/= 1
or aCn-ll/ 2 'I=(~) (mod n) then a is an Euler witness (to compositeness) for n.

11.3 Primality testing

265

Theorem 11.3.2 Let n be an odd composite integer and Jet a E Z~ be an Euler
witness for n. Then at least half of all elements in Z~ are Euler witnesses for n.
Proof: The set G = {x E Z~ I x<n- 1)12 = (~) (mod n)} of non-witnesses is
closed under the product in the finite group Z~, and hence G is a subgroup. By
Lagrange's theorem, IGI is a divisor of IZ~I- Since a E z~ \ G, it follows that

IGI ~ IZ~I/2.
As an alternative argument, verify directly that ab is an Euler witness for n
whenever b E z~ is not a witness. Necessarily, the number of non-witnesses in z~
is at most IZ~ 1/2.
D
Theorem 11.3.3 Let n be an odd composite integer. Then there exists an Euler
witness for n in z~.
Proof: First, suppose that n is not square-free, i.e., p 2 1 n for some prime p. Let
a= 1 +nfp. Then

= (1+n/p) = (1+n/p) (1+njp) = (-1 ) (_!_) = 1
( ~)
n
(njp)p
njp
p
njp
p
and, in particular, a E Z~. Also
p

aP

=(1 +nfp)P =1 + L (f)(njp)i =1 (mod n).
i=1

Hence ord(a) = p since a¢= 1 (mod n). Since p f n -1, it follows that a<n- 1)/ 2 ¢=
1 (mod n), and a is an Euler witness.
Now consider the case that n is the product of distinct primes. Let p be a
prime factor of n, and let b be a quadratic nonresidue modulo p. By the Chinese
remainder theorem, there exists an a satisfying
a=b

(modp),

a=1

(modnjp).

By properties of the Jacobi symbol,

(~) = (p(:jp)) = (~) c~p) = (%) c;p) = - 1
(in particular, a E Z~). The definition of a implies that a<n- 1)1 2 = 1 (mod njp)~
and hence a<n- 1ll 2 ¢= -1 (mod njp). It follows that a<n- 1)/2 ¢= -1 (mod n),
and a is an Euler witness.
D

11 Topics in Algebra and Number Theory

266

Roughly speaking, the Solovay-Strassen test looks for Euler witnesses for a
given n, and, if none are found, concludes that n is probably prime. The number
of Euler witnesses for a composite n is used to give an error bound.
Algorithm 11.3.4 (Solovay-Strassen) Input: an odd integer n > 2 and security
parameter t ::::: 1. Output: an answer of "composite" or "probably prime."

1. Do the following (at most) t times:
1.1 Select a random integer a, 1 < a < n.
1.2 If (a, n) I= 1, then return ("composite").
1.3 If a(n-I)/ 2 =f= (~) (mod n), then return("composite").
2. Return ("probably prime").
The running time for the inner loop is 0 (log~ n) bit operations. The algorithm
has been written for clarity; however, note that step 1.2 may be replaced by a
comparison on a(n-I)/ 2 mod n, terminating with "composite" if the value is not 1
or n -1.
If the test outputs "composite," then a certificate is provided which allows efficient verification that n is indeed composite. For Solovay-Strassen, the certificate
is an Euler witness for n, and the verification algorithm checks that (a, n) I= 1
or that aCn-I)/ 2 =/= (~) (mod n). The probability that the test outputs "probably
prime" when n is composite is at most 2- 1 •
Tests such as Solovay-Strassen (and Miller-Rabin, discussed in the exercises)
can be used to generate large random probable primes: choose random n of the
desired size until the test declares n to be "probably prime." In practice, n may
first be subjected to divisibility tests by small primes, and there may be additional
constraints depending on the application.
Exercises
11.3.5 Let n be an odd composite integer and let 1 :Sa < n. If an-I =f= 1 (mod n),
then a is said to be a Fermat witness (to compositeness) for n. Prove that if a is a
Fermat witness for n, then a is also an Euler witness for n.
11.3.6 Perform the Solovay-Strassen test on n = 91. Choose a = 74 as your
first "random" a. Unless you are rather unlucky, your second random a should
determine that n is composite.
11.3.7 (Miller-Rabin probabilistic primality test) Let n be an odd integer, and let
n -1 = 25 r where r is odd. Let a E Z~.
Fact. If n is prime, then either ar
j,O:Sj<s.

=1 (mod n) or a r =-1 (mod n) for some
2j

Definition. Suppose that n is composite. If ar =/= I (mod n) and if a 2j r =/= -1
(mod n) for all j, 0 :S j < s, then a is called a strong witness for n.

11.4 Factoring and square roots

267

Fact. If n f. 9 is an odd composite integer, then at least three-quarter of all a E Z~
are strong witnesses for n.

(a) Using the above concepts and facts, devise a test for determining the primality of an integer n.
(b) What is the running time (number of bit operations) of your test?
(c) Discuss the correctness of the results provided by this primality test.

11.4 Factoring and square roots
The problem of resolving a composite integer n into its prime factors is of more
than academic interest. Some widely-used cryptographic schemes (such as RSA,
discussed in Chapter 12) depend completely on the intractibility of factoring carefully chosen composite n. Factoring such ann can bring a measure of fame: the
New York Times devoted front page coverage in 1988 to the factoring of a 100digit integer by the quadratic sieve (discussed in Section 11.4.2) with a distributed
network of 400 computers. 1
No efficient method for factoring general n is known. A special-purpose algorithm can be effective when n is of certain form; for example, simple trial division
can be used to find small factors, and Pollard's rho (discussed below) may efficiently find relatively small factors of n. The factoring challenge discussed in
the New York Times article concerned n = pq for primes p and q of 41 and 60
decimal digits, respectively, specifically chosen to defeat certain special-purpose
algorithms. For such cases, a general-purpose algorithm from the random squares
family has been the method of choice.
For some material which follows, there is an implicit assumption that n has at
least two distinct prime divisors. An integer which is a perfect power (i.e., of the
form xk for some integers x and k > 1) is easily detected and, moreover, nontrivial
factors are easily obtained (Exercise 11.4.9).

11.4.1

Pollard's rho

Consider the following problem. Let X be a finite set, let f : X ~ X, and let

=

=

xo EX. We can define a sequence (xi) c; X as follows: XJ
f(xo), x2
f(xJ),
and, in general, Xi= f(xi-1) for each positive integer i. Since X is finite, this

sequence must eventually start repeating. In particular, for some i < j, we must
have Xi = x j. The problem is to find such a pair (i, j).
1 Malcolm W. Browne, "A Most Ferocious Math Problem Tamed," 12 October 1988. Apparently,
the title was insufficiently dramatic for the factoring enthusiast among the NYT editorial staff who
inserted the heading "World's Fiercest Math Problem Is Tamed by Hundreds of Computers."

268

11 Topics in Algebra and Number Theory

To help visualize this problem, we can construct the rhodiagram off at the seed xo. It has a vertex for each different
x;, and an arc from each x; to Xi+l = f(x;). A typical rhodiagram may resemble the Greek letter p. We denote by c
the number of arcs in the cycle of the diagram, and by t the
number of arcs in the tail. Then the entire diagram has k =
t + c arcs, and k vertices. The point where the tail meets
xo
the cycle is Xt = Xt+c· Note that if i and j are non-negative
integers, then x; = x j if and only if either i = j, or t :S i, j and i == j (mod c).
One way to solve our problem would be to successively compute and store x;,
comparing each new x; to all the previously stored values, until a match is found.
Unfortunately, this necessitates storing all k of the x; in the rho diagram, and k is
typically huge. Instead, Pollard's rho method stores only two variables, x andy.
Begin with x = xo andy= xo. The calculations
x *- f(x)

and

y *- f(f(y))

(that is, f (x) replaces x and f (f (y)) replaces y) are repeated until x = y. After i
repetitions, we'll have x = x; andy = x2;. If the method stops after m steps with
Xm = x = y = X2m, then (m, 2m) will be the required pair solving our problem.
It is easy to show that m will be the smallest positive integer with m ::=: t and
c 1m. The method will terminate after m < k steps, using 3m < 3k applications of
the function f. It can be shown that, for a random function f, if JXJ = n is large,
we can expect both t and c to be about J n n j8, so that k = c + t is about J n n /2,
or approximately 1.253-Jii. 2
We now turn to the problem of factoring a given composite integer n, and
suppose p :S ..jii is an (unknown) prime divisor of n. Our goal is to find integers
x, y with x ¢= y (mod n), butx == y (mod p). Then d = (x- y, n) will be not only
a multiple of p, but also a nontrivial divisor of n. The process may be repeated on
the factor d or n j d if detected as composite (perhaps by one of the tests in Section
11.3).
The idea is to run the rho method with some function f on Zn, but pretend it's
really running on Z P, even though p is unknown. To get away with this pretension,
f must have the property that for all a,b E Zn, if a== b (mod p), then f(a) ==
f(b) (mod p). Polynomials have this property, so f is typically chosen to be a
polynomial. It is desirable that f resemble a "random" function on Zp to improve
our chances of success. A typical choice off is f (x) = x 2 + 1, with a seed xo = 2,
though other choices can be used (a first degree polynomial would definitely be
bad).
2The estimate on k is related to the birthday paradox: in a group of 23 people, the probability that at
least two celebrate a birthday on the same day of the year is just over I /2 [63, 86]. Such considerations
are common in schemes designed to thwart attacks based on finding collisions by random search
(known as birthday attacks), leading to "square root" lower bounds on sizes for certain sets.

269

11.4 Factoring and square roots

Here are the details, for our polynomial f with integer coefficients, and any
xo E Zn. Begin with x = xo and y = xo. Repeat the two calculations
x +- f(x) mod n

and

y +- f(f(y)) mod n

until d = (x - y, n) > 1. If d < n, we have succeeded. If d = n, the method failed.
We could try again, with a different f or a different xo.
We expect this method to use the function f at most 3-ylnp/2 = 0(./P) =
O(n 114 ) times, since p :::; .jri. This is still not theoretically efficient, but it's better
than the simple-minded method of trying all d, 1 < d :::; .jri, checking if d I n,
which could take O(n 112 ) divisions.
As an example, let's factor n = 551 by this method, with j(x) = x 2 + 1 mod
551 and xo = 2. Here are successive values of x, y, and d = (x - y, n):
x +--- f(x)
5
26
126

y +--- f(f(y))

d=(x-y,551)

26

1
1
19

449
240

Note that 19 and 551/19 = 29 are both prime, so we've factored 551 = 19 · 29.

11.4.2 Random squares
Among general-purpose factoring methods, the most powerful have been those in
the random squares family. The quadratic sieve variant was used to factor integers
with 100-129 decimal digits by 1994. A more complicated variant known as
the number field sieve factored well-known challenges from RSA Laboratories of
130 digits in 1996 and 140 and 155 digits in 1999. (The 155-digit result in August
1999 was somewhat of a milestone due to the implications for applications relying
on the intractability of factoring 512-bit numbers.)
Given composite n, the idea is to locate x, y E Zn with x 2
y 2 (mod n);
if x "/= ±y (mod n), then (x + y,n) is a nontrivial factor of n since n divides
x 2 - y 2 = (x + y)(x- y) but does not divide either of the factors. As a special
case, if n = pq for distinct primes p and q, then x 2 = a 2 (mod n) for given a E Z~
has exactly four solutions, as can be seen by the Chinese remainder theorem. If
n = 15, for example, and we manage to stumble upon x = 2 andy= 7 satisfying
x 2 = y 2 (mod 15), then (x + y, n) = (9, 15) = 3, a nontrivialfactor of n.
One method for finding appropriate x andy is to collect pairs (a;, b; =a? mod
n) for random a;, seeking a subcollection S for which TI; ES b; is a perfect square.
In thiscase,x = TiiESa; and thesquarerooty off1;Esb; satisfy x 2 = y 2 (mod n).
If, in addition, x "/= ±y (mod n), then the method has been successful; otherwise a different S can be selected (perhaps requiring the generation of more pairs
(a;, b;)).

=

11 Topics in Algebra and Number Theory

270

To be more precise, a factor base B = {Pl, ... , p 1 } consisting of the first t
primes is selected. Pairs (a, b = a 2 mod n) are retained if b factors over B; such
a value b is said to be p 1 -smooth. Suppose t + 1 such pairs (a;, b;) are obtained,
where the factorizations are given by b; = f1}= 1 p~ij, 1 ::::; i ::::; t + 1. A setS is
to be selected so that TiiESbi has only even powers of primes appearing. Note
that the t + 1 vectors e; = (en, ... , eit) mod 2 are necessarily linearly dependent
over Z2, and there exists S so that LiESe; is the zero vector. The corresponding
TI; ES b; is then of the desired form.
Example 11.4.1 The method is applied to factor n = 10057. Suppose t = 5 so
that the factor base B = {2, 3, 5, 7, 11} is selected. The following table lists the
a; selected, retaining only those for which b; = af mod n factors over the factor
base.
a;
7231
105
2
115
3 1006
4 3010
5 4014
6 4023

b; =a? mod n

factorization

1018
968
3168
6336
8800
882
2816

2·509
23 ·112
25 .32 ·11
26.32 ·11
25 .5 2 ·11
2·3 2 ·7 2
28 ·11

The choice a = 7231 is discarded since a 2 mod n does not factor over the factor
base. In such cases, the last column may be only a partial factorization.
There are t + 1 = 6 valid pairs (a;, b; ). Necessarily, there must be some S
so that TiiES b; is a perfect square. By inspection, S = {4, 5, 6} gives x 2 = y 2
(mod n) for x = 3010·4014·4023 mod n = 2748 andy =2 7 ·3·5 ·7 ·11 mod n =
7042. Since x ¢= ±y (mod n), we obtain a nontrivial factor (x + y, n) = 89, and
10057 = 89. 113.
A perfect square is also obtained for S = {1, 5}. The corresponding values are
x = 105 · 4014 mod n = 9133 andy= 22 · 3 · 7 ·11 = 924. Unfortunately, x = -y
(mod n), and no useful information is obtained.
Choosing a larger factor base increases the chances that a given b; will be
p 1-smooth at the cost of requiring more relations. For a given t, one strategy to
improve the chances of obtaining p 1 -smooth numbers is to select a so that b = a 2
(mod n) is relatively small. The quadratic sieve is a famous example of such a
method.
Letn be given anddefinem = LJnJ. Define q: Z ~ Z by q(z) = (z+m) 2 -n.
Note that q(z) ~ z2 + 2zm and Jq(z)J is small relative ton if lzl is small. The
quadratic sieve algorithm takes a = z + m and b = q (z) = a 2 - n for z = 0, ± 1, · · ·.
Since b can be negative, -1 is added to the factor base. In addition, note that if
a prime p divides b, then a 2 = n (mod p) and n is a quadratic residue modulo p

11.4 Factoring and square roots

271

(unless pIn). Hence, the factor base need only contain those primes p for which
G)= 1.

Example 11.4.2 The value n = 10057 from the last example is factored. Define
LJ11J = 100 and q(z) = (z + 100) 2 - 10057. For a factor base, suppose
B = {2, 3, 11, 19} U {-1} is selected (which includes those primes p:::; 19 such
that (!!) = 1). The first few z for which q(z) factors over the factor base are listed
in th/following table.
m =

z a=z+m
0
-1
1
-3
5

100
99
101
97
105

b = q(z)
-57
-256
144
-648
968

factorization
-3·19
-28
24 ·32
-23 34
23 ·112
0

The relations for z E { -1, -3, 5} give x 2 = y 2 (mod n) for x = 99 · 97. 105 and
y = 27 · 32 · 11. Unfortunately, x = y (mod n) and no useful information is obtained.
The line corresponding to z = 1 gives 101 2 = 24 · 32 . Note that x = 101 and
y = 22 · 3 satisfy x ¢- ±y (mod n), and hence a nontrivial factor (x + y, n) = 113
of 10057 has been obtained.
Collecting appropriate (a, b) pairs can be a distributed effort, and trial division is replaced by a more efficient sieving process to test smoothness. The 1994
factoring of a well-known 129-digit challenge involved 600 people and 1600 machines collecting more than 8 million relations over seven months, with a factor
base of size 524339 [1]. However, as of 1999, factoring appropriately-chosen n
with at least 1024 bits (308 decimal digits) appears to be well out of reach of even
the more advanced number field sieve.

11.4.3 Square roots
The previous section suggests a relationship between the problems of factoring
and extracting square roots. In fact, the problems are equivalent in a certain precise
sense to be discussed in this section.
Recall that a random square factoring method attempts to construct x and y
with x 2 = y 2 (mod n); if x ¢- ±y, then a nontrivial factor (x + y, n) ofn has been
obtained. Clearly, if it was possible to extract all of the square roots for some
quadratic residue x 2 in Z~, then a nontrivial factor of n can be found. No efficient
method in general for uncovering roots is known; however, we wish to consider
the implications of such an algorithm.
To be specific, we will consider the case that n = pq for distinct odd primes p
and q, although the discussion extends to the general case. By Corollary 11.2.2,

11 Topics in Algebra and Number Theory

272

given a quadratic residue modulo a prime, there are exactly two square roots;
the Chinese remainder theorem can be used to show that x 2 = a (mod pq) for
a E Qpq has exactly 4 solutions.
We suppose there exists an oracle for SQRooT; that is, we assume there is
an algorithm returning a square root when presented with a quadratic residue. A
factoring attack can then be constructed by selecting random x E Z~ and passing
x 2 mod n to the oracle. For each such x, the root y returned by the oracle satisfies
y f,. ±x with probabilty 1/2; a nontrivial factor (x + y, n) of n is expected in two
trials.
The problem of factoring n is said to reduce to the problem of finding square
roots. More precisely, given computational problems A and B, we write A s B
if A can be solved in polynomial time (in terms of the size of the input) given a
polynomial-time algorithm for B. Informally, A s B says that A is no harder than
B. The problems are computationally equivalent if A s B and B s A. The same
notation will be used in the case that the algorithm involves randomization and has
expected running time which is polynomial in the size of the input. In particular,
FACTORS SQROOT.
Now consider the problem of finding square roots of a quadratic residue a E
Qn given the factors of n. If roots modulo a prime can be extracted, then the
4 roots of a modulo n = pq can be obtained by finding x and y satisfying the
congruences
x =ap
x=aq

(mod p)
(modq)

and

y = -ap

y=aq

(mod p)
(modq)

where a, denotes a square root of a modulo r; the roots are then ±x, ± y. Hence,
we obtain an efficient algorithm for SQROOT if there is an efficient algorithm for
extracting roots modulo a prime.
Consider first the case of a prime p = 3 (mod 4). If a E Qp, then a(p-l)/Z =
1 (mod p) by Euler's criterion, and (a(p+Il/ 4) 2 = aCp+ll/ 2 = a(p-llf 2a =a
(mod p). Hence, the two square roots of a modulo p are ±aCP+ll/4. If both
p and q are of this form (in which case n = pq is known as a Blum integer), then
we have an efficient deterministic algorithm for finding roots of a modulo n.
It turns out that there is a relatively simple algorithm for extracting roots modulo a prime provided that a quadratic nonresidue is known (in particular, if p = 5
(mod 8) since 2 E QP in this case; see [63]). However, there is no known efficient
deterministic algorithm for finding quadratic nonresidues modulo a prime. Since
half of the elements of Z~ are quadratic nonresidues, an efficient probabilistic procedure for finding roots modulo a prime can be developed by selecting elements
x at random until one is found with ( ~) = -1.
In short, there are efficient methods for extracting roots modulo a prime. It
follows that SQROOT s FACTOR and hence the two problems are computationally equivalent. The result is discussed again in Section 12.3 in connection with
provable security.

11.4 Factoring and square roots

273

Exercises
11.4.3 This exercise illustrates Pollard's rho factoring algorithm on the toy examplen = 391 = 17 ·23.
(a) PerformPollard'srhowithxo=2andx;+l =f(x;)=xf+1 modn. (Recall
that the process terminates if 1 < (x2; - x;, n) < n. A nontrivial factor is
obtained fori = 4.)
(b) Complete the following table:
x;mod17
x; mod 23

0123
2 5 9 14
2

4
10

5678
16 2

Sketch a rho-diagram for each row of the table, identifying the tail and the
cycle.
(c) How should a composite n be chosen to defeat a factoring attack based on
Pollard's rho?
11.4.4 Factor n = 5141 using Pollard's rho algorithm, with xo = 1 and Xi+l =
xf + 2 mod n for i :::: 0.
11.4.5 Apply the quadratic sieve (as in Example 11.4.2) to factor n = 1081 using
an appropriate factor base consisting of integers ::; 11.
11.4.6 Apply the quadratic sieve (as in Example 11.4.2) to factor n = 24961 using
an appropriate factor base consisting of integers :::: 23.
11.4.7 If n = ab with Ia -hi relatively small, then Fermat factorization efficiently
recovers nontrivial factors.
• Assume that n is odd. There is a 1-1 correspondence between the factorizations of n in the form n = ab, where 0 <a ::; b, and the representations
n = t 2 - s 2 , where t and s are nonnegative. The correspondence is given by
ab = t 2 - s 2 = (t- s)(t + s), t =(a+ b)/2, and s = (b- a)/2.
• If a and bare close together, then s = (b- a)/2 is small and t ~ .fii. To
factor n, test consecutive values for t starting with
until t 2 - n is a
perfect square.

r.fiil

Illustrate the method on n = 2881. Fermat factorization is discussed in [50, 74].
11.4.8 Illustrate the procedure for finding square roots of 179 in z187 using the
factorization 187 = 11 · 17.
11.4.9 Devise an efficient test which determines if n is a perfect power (i.e., n =
xk for some integers x and k > 1) and, if n is such an integer, finds a partial

factorization.
11.4.10 Let n = pq for primes p f. q, and let¢= ¢(n) = (p -1)(q- 1). Let
COMPUTE-¢ denote the problem of finding¢ given n. Describe an efficient algorithm for FACTOR which uses an oracle for COMPUTE-¢.

274

11 Topics in Algebra and Number Theory

11.5 Discrete logarithms

z;

z;'

Let p be an odd prime and Ol a generator of
Given f3 E
the discrete
logarithm of f3 to the base ot, written loga f3, is the unique integer x, 0:::: x:::: p- 2,
such that f3 = otx (mod p ). For example, ot = 3 is a generator of Z5 and log 3 4 = 2
since 4 = 32 (mod 5).
Finding x given (p, ot, f3) is known as the discrete logarithm problem (DLP).
As with integer factorization, it is of course a trivial problem in theory since we
may calculate otx for x 2: 0 until f3 is found. This is not efficient, requiring 0 (p)
modular multiplications. No efficient algorithm is known, and several cryptographic schemes rely for security on the supposed intractability of DLP. We shall
consider two algorithms which, while not polytime, are more efficient than exhaustive search.

11.5.1

0

Baby-step giant-step

The baby-step giant-step algorithm is roughly similar to the meet-in-the-middle
attack on double-DES, reducing the number of modular multiplications in exhaustive search at the cost of significant storage requirements.
If f3 = otx (mod p), then we may write x = im + j for
Let m =
0:::: i, j < m and f3 otx otimoti, or f3ot-im
ai. Form a table with entries
(j,oti mod p), 0:::: j < m. For each i, 0:::: i < m, calculate f3ot-im mod p and
check for a match in the table. On a match, we have f3ot-im = ai (mod p) and
loga f3 = im + j.

I.JP=Tl

= =

=

Example 11.5.1 The method is applied to find log 6 2 in Z41 (i.e., p = 41, ot = 6,
and f3 = 2). Let m =
= 7 and form a table with entries (j, ai) for 0:::: j < 7:

f,J40l
j

10 1

al mod p

1

=

6

2
36

3
11

4
25

5
27

6
39

= =

We find ot- 1 7 (mod p) and ot-m 77 17 (mod 41). It is now a matter of
calculating f3(ot-m)i mod p until a table entry is found:
i = 0:
i = 1:
i=2:
i=3:

= =

f3(a-m)O
f3 2
f3(a-m) 1 = 2·17 = 34
f3(a-m) 2 =34-17=4
f3(a-m) 3 =4·17=27

A match has been found fori = 3 and j = 5, giving f3ot-3m
(mod p). Hence, log 6 2 = 26 in Z41·

=ots and f3 =a +

21 5

The table requires space form group elements, built with m - 1 modular multiplications. The giant steps require an inversion and O(m) modular multiplications, so the running time of the algorithm is 0 (,JP=l) modular multiplications.
This is better than exhaustive search, but much worse than polynomial time.

275

11.5 Discrete logarithms

11.5.2 Index calculus
The most powerful methods for finding discrete logarithms are variants of the
index calculus algorithm, portions of which resemble random squares algorithms
for factoring. A computationally-expensive first stage determines logarithms for
elements of a selected factor base B (which need not depend on the specific f3 for
which loga f3 is desired). The second stage attempts to find an integer k so that
ak f3 factors over B; if successful, then loga f3 is easily obtained.
For Zp, the factor base B ={PI, ... , p 1 } consisting of the first t prime numbers
may be selected. In the first stage, random k are selected in an attempt to find
values ak mod p which factor over the factor base. For such a k, ak mod p =
p~ 1 ••• p~' for some e; 0::: 0 and herpce
k == e1loga PI+··· +etloga Pt

(mod p -1).

Somewhat more than t such congruences are obtained, in the hope that the resulting linear system in the unknowns loga p; has a unique solution.
In the second stage, we seek a value of k for which ak f3 mod p factors over
B. If successful, then ak f3 mod p = p~ 1 • • • p~' for some e; 0::: 0, which implies
k + loga f3 == e1loga PI+···+ e1 loga p 1

(mod p- 1)

and loga f3 = (e1loga PI+···+ et loga Pt- k) mod (p- 1).

Example 11.5.2 The method is illustrated for p = 19 and a = 2, finding loga 17.
Suppose the factor base B = {2, 3, 5} is selected. In this particular example, a E B
and loga 2 = 1 is obtained directly. To determine the logs of the remaining two
elements of B, we calculate ak mod p for random k, seeking at least two values
which factor over the factor base:
29 mod p = 2 · 3 2

27 mod p = 14
2 11 mod p = 3 · 5

The second line is dropped since 14 does not factor over B. This gives the following system of congruences
9=loga2+2loga3 (mod p-1)
11

=loga 3 + loga 5 (mod p- 1)

for the unknowns loga 3 and loga 5. However, these congruences do not determine
a unique pair, giving candidates such as the incorrect solution (loga 3 = 4, loga 5 =
7). If the relation a 14 mod p = 6 is added, then the new system with the additional
congruence 14 == loga 2 + loga 3 (mod p - 1) has a unique solution loga 3 = 13
and loga 5 = 16.

11 Topics in Algebra and Number Theory

276

To find loga 17, we seek k such that ak · 17 mod p factors over B. If k =
5, for example, then a 5 · 17 mod p = 22 · 3 and loga 17 = (2loga 2 + loga 35) mod (p-1) = 10.
The precomputation of logarithms of elements in the factor base is expensive,
although collecting suitable relations can be a distributed effort. The first-stage
results can be used to find the logarithm of any given f3 provided that a value of k
can be found for which ak f3 factors over the factor base. Choosing a larger factor
base allows more elements of
to be represented as products of elements of B
at the cost of a larger system of congruences to be solved.
Sophisticated enhancements of the basic method were used in the 1990s to calculate logs modulo primes of 50-100 decimal digits. As an example, LaMacchia
and Odlyzko [54] demonstrated in 1990 that calculation of logs modulo a certain
192-bit (58 decimal digit) prime was quite feasible using an index-calculus variant
known as the Gaussian integer method. A system of 288017 relations with 96321
unknowns was reduced to one with 7262 equations and 6006 unknowns and then
solved. The database could then be used to obtain individual logarithms with relatively little effort. The problem was of practical interest, since the security in a
widely-used identification scheme proposed by Sun Microsystems depended, in
part, on the difficulty of extracting logarithms modulo the prime [88].
In 1998, Joux and Lercier calculated discrete logarithms in
where p was a
90 digit prime, using a Gaussian integer method. A network of 4 machines (each
based on a 180MHz Pentium Pro, a relatively inexpensive processor) collected
6.7 million equations over a month, obtaining 976062 equations for which every
variable appeared at least twice. The linear system was reduced and then solved
in 3 weeks. Selected logarithms required an average of 9 hours each on a single
machine.
The number field sieve for factoring integers has been adapted for the log problem. Joux and Lercier used a sieve variant in 1999 to compute logarithms in
with p having 100 digits. A machine based on a 450 MHz Pentium II collected
2.9 million equations over 8 months, passing the results to a 4-processor (DEC
Alpha 500 MHz) computer which solved the linear system in 3 weeks. Individual logarithms were calculated in a day. They concluded that the number field
sieve "seems to be better than the Gaussian integer method for computing discrete
logarithms modulo primes over 100 digits" [46].

z;

z;

z;

Exercises

11.5.3 Given that a = 5 is a generator of Z9 7 , illustrate baby-step giant-step on
log5 4 in Z97.

z;).

11.5.4 Let p = 41 and ot = 6 (a generator of
Illustrate the index calculus
method on log 6 13 by completing the following outline.

277

11.6 Notes

(a) Choose factor base B = {2, 3, 5}. Suppose that ak mod p has been calculated fork E {8, 20, 16} giving
a 8 modp=lO

==}

8:=loga2+loga5 (modp-1)

a 20 mod p = 40

==}

20 = 3loga 2+loga 5 (mod p -1)

a 16 modp= 18

==}

16:=loga2+2loga3 (mod p-1)

Verify that the system of congruences does not determine a unique solution
(log"' 2, log"' 3, log"' 5).
(b) Add the congruence obtained from a 1 mod p = 2 · 3, and then solve. (The
value of log"' 2 should match that found in Example 11.5 .1.)
(c) Find log"' 13 by applying (b) to ak · 13 mod p fork= 11.

11.5.5 Let p be prime and a a generator of Z~. Show that the least significant bit
of x can be efficiently determined from ax mod p.
11.5.6 Bach [2] discusses relationships between factoring and finding logarithms.
In particular, this exercise shows that an oracle giving x for ax = b (mod n) can
be used in a probabilistic factoring attack on composite n.
Consider the case n = pq for odd primes p f. q. Let A. = lcm(¢ (p), ¢ (q)).
(a) Show that K = {z E z~ I z'A/ 2 = ±1 (mod n)} is a proper subgroup of z~.
Hence, at least half the elements of Z~ are outside K.
(b) Let a E Z~ \ K, and suppose that ax = 1 (mod n) for some 0 f. x (known as
an exponent for a). Show that there exists 0 < k < log 2 x such that axf 2k is a
nontrivial square root of 1 (that is, axf 2k ¢. ±1 and (axf2k)2 = 1).
(c) Conclude that (axf 2k + 1, n) is a nontrivial factor of n.
A factoring attack uses the oracle for ax = b (mod n) to find an exponent for a.
Although ¢ (n) is unknown, there must be some r among the first log 2 n primes
such that (r, ¢ (n)) = 1. For such an r, the oracle returns a solution y of (ar)y =a
(mod n), and x = ry- 1 is an exponent for a.

11.6

Notes

Much of the material in this chapter is classical, for which many good introductory
books exist, e.g., Rosen [74] and Koblitz [50]. Chapters 2 and 3 of Menezes,
van Oorshot, and Vanstone [63] concisely covers some of the relevant material,
and includes many references. Koblitz includes several examples of calculating
time estimates (in terms of bit operations) for arithmetic, a topic which is lesscommonly covered in number theory books.

This page intentionally left blank

