Chapter 12

Public-key Cryptography
The fundamental feature which distinguishes public-key from symmetric-key
schemes is the separation of encryption and decryption capabilities. To be slightly
more precise, the key kin a public-key scheme is written as a key-pair k = (e, d),
where e is used for encryption and d is used for decryption. In this context, e is
the public key and need not be secret; d is the private key, and is held only by
those needing to decrypt messages. To be useful as an encryption scheme, it must
be computationally infeasible for an adversary in possession of e and ciphertext c
to find m so that Ee(m) =c.
The idea of public-key schemes was introduced by Diffie and Hellman in 1976
to exchange keys over an unsecured but authenticated channel. In the context of
Figure 10.1, the understanding is that Alice and Bob are certain of the origin
and integrity of communication over the unsecured channel, but there may be
eavesdropping (passive attacks) by an adversary Eve. The problem is to maintain
secrecy (even though Eve sees the entire conversation) without relying on the
secure channel portion of the figure to transfer keys or other information.
The following key agreement is proposed: Alice and Bob choose a prime p
and a generator a of z;; p and a may be public. In private, Alice chooses a
random a, 1 ::::; a < p, and transmits a a to Bob (in full view of Eve) .1 Similarly,
Bob chooses a random b, l ::::; b < p, and sends ab to Alice. Alice and Bob
may efficiently calculate (ab)a mod p and (aa)b mod p, respectively, arriving at
the shared secret k = aab mod p from which a common key may be derived for
subsequent use in a symmetric-key scheme such as DES to achieve confidentiality.
Eve, of course, would also like to have the shared secret k. Her quest is known
as the Diffie-Hellman problem (DHP), related to the discrete log problem (DLP):
DHP: Given prime p, generator a of
DLP: Given prime p, generator a of

z;, aa, and ab, find aab.
z;, and ax, find
X.

1 For brevity, the operator "mod p" will often be omitted if the meaning is clear from context. Here,
for example, we mean that aa mod p is transmitted.

279

12 Public-key Cryptography

280

It is clear that DHP::; DLP; that is, DHP can be solved in polynomial time given
a polytime oracle for DLP. In some special cases, it is known that DLP::; DHP,
but the general case remains an open question [63]. The presumed intractability
of DHP is the basis for the security of the Diffie-Hellman key agreement.
Public-key schemes can be used to address some difficulties inherent in
symmetric-key schemes. The Diffie-Hellman key agreement suggests that secrecy
can be maintained even without a secure channel for exchanging keys. (It is important to note the assumption of an authenticated channel-the agreement is not
secure against an active adversary, as illustrated in Section 12.5.) Provided that
the public keys can be authenticated, key distribution does not require a trusted
courier. Since secrecy of the public key is not required, authentication is presumably easier than exchanging symmetric keys.
Public-key reduces the key management problem. If there are n users of a
cryptosystem, then G) keys are required in a symmetric-key scheme compared
with 2n in a public-key scheme, a significant difference if n is large.
Digital signatures (discussed in the following sections) are another area where
public-key schemes are attractive. Alice should be able to sign a message in such
a way that not only is Bob convinced that the message originated from Alice,
but Bob can convince a third party of the same. The basic problem with using
symmetric-key schemes for signatures is that Bob and Alice have the same capabilities. A trusted third party can be used to enable signatures, but public-key
schemes provide a more mathematically elegant and practical solution.

12.1

One-way and hash functions

Two cryptographic primitives are examined in this section, trapdoor one-way
functions (the fundamental notion in public-key schemes), and cryptographic hash
functions (commonly used in signature schemes).
One-way functions

Informally, a function f : %---+ Cis one-way if f(m) is easy to compute for
all m E %, and for essentially all c E C it is computationally infeasible to find
m so that f(m) =c. The function described in Section 10.3.2 for Unix passwords is believed to be one-way (under some ceiling on computational ability),
storing f(userid,password) rather than the password itself. 2 Another candidate
is discrete exponentiation. Let p be prime and a a generator of Z~. The function
f: z~---+ z; defined by f(a) =Cia mod pis believed to be one-way.
2There was sufficient confidence in the scheme that the stored values were visible to all the users
of the system, inviting password-cracking analysis. Needless to say, this turned out to be a bad idea,
since users often select predictable passwords. Modern systems tend to demand better passwords and
have made the stored values private.

12.1 One-way and hash functions

281

It is not known if one-way functions exist. Public-key schemes certainly assume that various functions, if not one-way, will be effectively one-way for the life
of the specific security objective. A number of candidates for use in public-key
schemes are discussed in the following sections.
A one-way function is trapdoor if it has the property that given some extra
information, it becomes feasible to find m so that f(m) = c for any given c. The
trapdoor one-way function is the basic tool in public-key cryptography.

Example 12.1.1 (applications of trapdoor functions) Confidentiality. Each user
A picks their own trapdoor one-way function fA and makes it public. To transmit
a confidential message m to A, the sender forwards c = fA (m ). Only A has the
trapdoor information allowing f to be inverted-even the sender cannot recover
m from c. No secret information needs to be shared in advance, although it is
necessary for the sender to have a means of authenticating the public key.
Non-repudiation. The analogue of a hand-written signature is desired: a message m from Alice is to be signed in such a way that Bob can convince any third
party that m originated with Alice. Assume that m has some prescribed degree
of redundancy. If Alice's trapdoor one-way function is fA : r;v[---+ %, then Alice sends s = fi_ 1 (m). Bob computes m = fA(s) and (m,s) is the signed message. Although an adversary can calculate m = fA (s) for chosen s, forgeries are
thwarted by the assumed redundancy in m. If secrecy is desired, Alice could send
c = fs (s) rather than s directly, where fs is Bob's trapdoor one-way function.
Cryptographic hash functions

A mapping H : X ---+ Y which is not one-to-one is known as a hash function. For
a given x E X, the value H (x) is called a hash, and serves as an "identifier" for
x. By definition, there are necessarily collisions-values XJ 'I= x2 with the same
hash. Typical design goals for a hash function include spreading conditions on the
collisions and efficient calculation of the hash values.
Public-key cryptosystems tend to be relatively computationally intensive, and
signing long messages is a slow procedure. In practice, a hash function is used to
create a message digest of the message, and it is the digest that is signed. In order
for this to be secure, additional features of the hash function are needed.
Definition 12.1.2 A cryptographic hash function is a mapping H : {0, 1}* ---+
{0, l}n such that:

1. H can be efficiently computed;
2. (preimage resistance) for essentially ally E {0, l}n, it is computationally
infeasible to find x E { 0, 1} * such that H (x) = y;
3. (collision resistance) it is computationally infeasible to find XJ '/= x2 E
{0, 1}* such that H(xJ) = H(x2).

282

12 Public-key Cryptography

It is not known if cryptographic hash functions exist (since 1 and 2 imply that
such a function is one-way); however, there are a number of candidates used for
data integrity and signature schemes.

Example 12.1.3 (application to signatures) To sign a message m, Alice computes
and signs the hash H (m). The message m and the signature on H (m) are sent
to Bob, who computes H (m) and verifies the signature. A scheme where the
message itself is required in the verification process is known as a digital signature
scheme with appendix.
If the hash function H is not preimage resistant, then an adversary (perhaps
Bob) given a valid signature for H (m) can forge Alice's signature by finding a
message m' with H (m') = H (m), obtaining a valid signature form'.
If m f= m' giving collisions can be found, then Alice can cheat by signing m
and later claiming to have signed m'. A variation on this occurs if an adversary
finds such a collision and convinces Alice to sign one of the messages.
In the example, the signature presumably protects the integrity of the hash
computed by Alice. In general, if the hash value can be protected by some mechanism, then a hash function can be used to verify that the corresponding data has not
been modified. In this context, an unkeyed hash function is called a modification
detection code (MDC). A keyed hash function providing data origin authentication
using a secret key is called a message authentication code (MAC); the CBC-MAC
described in Section 10.3.2 is an example.
Several hash functions have been proposed which are built on block ciphers.
As noted, CBC-MAC is a keyed hash function, while the following two examples
are unkeyed.
Example 12.1.4 (Matyas-Meyer-Oseas hash) Let E be a block cipher, Ek : {0, 1}n
~ {0, 1}n, with keyspace 1(. Let g : {0, 1}n ~ 1( be a (non-secret) function, and
let Ho be a (non-secret) initial value in {0, l}n. The mapping H: {0, 1}* ~ {0, l}n
is defined as follows:
1. Break x into n-bit blocks, say x = XJ · · · x 1 •
2. Let Hi= Eg(H;_!)(Xi)(Bxi, 1 :S i :St. Then H(x) = H1 •
A schematic appears in Figure 12.l(a).
Roughly speaking, if the roles of the Xi and Hi-! are interchanged, then the
following proposed hash function is obtained. Note however, that the splitting of
x differs.
Example 12.1.5 (Davies-Meyer hash) Let E be ann-bit block cipher using k-bit
keys. Let Ho be a (non-secret) initial value in {0, l}n. The mapping H: {0, 1}* ~
{0, l}n is defined as follows:
1. Break x into k-bit blocks, say x = x1 · · · x 1 •
2. Let Hi= Ex;(Hi-I) EB Hi-!, 1 :S i :St. Then H(x) = H1 •
A schematic appears in Figure 12.1(b).

283

12.1 One-way and hash functions
H;-l

Xj

"'':i
H;

(a) Matyas-Meyer-Oseas hash

x;~
key

tJ
H;

(b) Davies-Meyer hash

Figure 12.1: Unkeyed hash functions based on block cipher E.

The message digest produced by an unkeyed hash function must be of sufficient length to prevent birthday attacks (random searches for collisions). For
n-bit hash functions, it is expected that collisions can be found in at most 2n!Z
operations. In particular, neither of the hash functions in Figure 12.1 is collisionresistant if E =DES. Two well-known hash functions, the Secure Hash Algorithm
(SHA-1) and MD5 (both based on MD4, number 4 in a series of Message Digest
algorithms from Rivest) have digest bitlength 160 and 128, respectively.
Exercises
12.1.6 Suppose g: {0, 1}*-+ {0, l}n is a collision-resistant hash function. Let h
be defined by
if x has bitlength n,
h(x) = 1111x,
0 II g(x), otherwise,

where 'II' denotes concatenation. Give an informal argument to show that h is an
(n + 1)-bit hash function which is collision resistant but not preimage resistant.
(The example is from [63, Note 9.20].)
12.1.7 This exercise is adapted from [63, Example 9.64], illustrating that some
care must be taken when constructing a MAC from an MDC.

Suppose that h is an MDC defined in an iterated fashion on message x =XI · · · x 1
by H; = f(H;-],X;), h(x) = H 1 , where Ho is a given initial value. It is proposed
that h can be converted into a MAC by prepending a secret key k so that the MAC
on a message xis M = h(kx). Show that knowledge of an (M,x) pair can be used
to forge a MAC on xy, even though k is unknown.
12.1.8 Suppose p and q are primes such that p' == 2 p + l and q' = 2q + 1 are
also prime, and let n = p' q'. Suppose a is an element of order 2 pq in ::Z~, and let
h: ::Z-+ ::Z~ be defined by h(x) =ax mod n. If h(xi) = h(xz) = h(x3), show that
n can be efficiently factored under suitable conditions on the collisions x;.

Illustrate the process with the toy modulus n = 77 and a = 2, where h(9) =
h(69) = h(129). (This exercise is adapted from [86, Exercise 7.4].)

284

12.2

12 Public-key Cryptography

RSA

Proposed in 1977 by Rivest, Shamir, and Adleman, the RSA cryptosystem is
widely-used for encryption and signature schemes, with security based on the
presumed intractability of the integer factorization problem. The RSA function
itself is defined as follows.
1. Let p =I= q be large primes, and define n = pq and¢= (p- 1)(q- 1).
2. Choose random encryption exponent e, 1 < e <¢,so that (e, ¢) = 1.
3. Use the Euclidean algorithm to find decryption exponent d, 1 < d < ¢, so
that ed = 1 (mod¢).
4. Define f: Zn-+ Zn by f(m) =me mod n.
The RSA function f is believed to be one-way with trapdoor d. Given d, recovery of m from c = f (m) can be done efficiently using the property ed = 1
(mod¢). To see this, first suppose that p f m. Note that ed = 1 + k¢ = 1 +
k(p- 1)(q- I) for some k, and by Fermat's theorem mP- 1 = 1 (mod p) so that
m 1+k(p-l)(q-l) = m (mod p). If pI m, then the last congruence holds trivially.
This shows that
ml+k(p-l)(q-1) =med =m (mod p).
By a similar argument med = m (mod q), and hence med = m (mod n) since p
and q are distinct primes. It follows that
cd = (me)d = m

(mod n),

efficiently recovering m from c by modular exponentiation.
Inverting f without possessing d is the RSA problem: given a product n = pq
of distinct primes, a positive integer e relatively prime to (p- 1)(q- 1), and an
integer c, find m such that me = c (mod n). If n can be factored, then d may
be computed and m obtained efficiently. However, factoring n for appropriately
chosen p and q is believed to be a hard problem.
Exercise 12.2.12 shows that an efficient method to factor n is obtained if d can
be computed. Hence, the problem of factoring n and the problem of computing
d from n and e are computationally equivalent. The equivalence provides some
evidence that the RSA problem may be as hard as factoring, but this remains an
open question. 3
In the RSA cryptosystem, (n, e) is the public key and d is the private key.
Encryption of mE [0, n- 1] gives c =me mod n; decryption ism= cd mod n.
Example 12.2.1 (RSA with toy parameters) Suppose p = 7 and q = 13 are used in
RSA key generation. Then¢= (p -1)(q -1) = 72, and we may choose encryption exponent e = 5. The conditioned= 1 (mod¢) gives decryption exponent
3Boneh and Venkatesan [15] provide evidence that breaking low-exponent RSA (that is, RSA with
small encryption exponent e) cannot be equivalent to factoring.

12.2 RSA

285

d = 29. The public key is (n = 91, e = 5) and the private key is d = 29. Messages
m consist of integers in [0, 91). If m = 23, then c =me = 23 5 mod 91 = 4 and
cd mod n = 429 mod 91 = 23 can be verified using repeated squaring.

An RSA encryption scheme passes some messages unconcealed; that is, there
are messages m with c =me mod n = m. The messages mE {0, 1, n -1} arealways unconcealed. Note that me= m (mod n) if and only if me= m (mod p)
and me= m (mod q). Solutions of me= m (mod p) satisfy m = 0 (mod p) or
me-! = 1 (mod p ); there are 1 + (e- 1, p- 1) such solutions (Exercise 11.1.22).
The Chinese remainder theorem can be applied to show that the number of unconcealed messages is (1 + (e- 1, p- 1))(1 + (e- 1, q- 1)). In particular, there
are always at least 9 unconcealed messages since e, p, and q are odd. Example
12.2.1 has 15 unconcealed messages; m = 8 is one such message. Exercise 12.2.3
illustrates the extreme case where every message is unconcealed.
Although known- and chosen-plaintext attacks generally do not apply to
public-key encryption, there are scenarios where a chosen-ciphertext attack is a
concern. Let ct and c2 be RSA encryptions of mt and m2, respectively. Then
(m1m2)e = mJ.m2 = CJC2 (mod n), giving c = qcz mod n as the ciphertext corresponding tom= mtm2 mod n. An adversary Eve may be able to exploit this
multiplicative property by concealing the target ciphertext c in c and convincing the private-key holder Alice to provide decryption of c. Eve chooses X E z:;
and passes c = cxe to Alice for decryption, obtaining m = cd mod n. Since
cd cdxed mx (mod n), Eve recovers m = mx- 1 mod n. Structural requirements on plaintext messages may defeat the attack.
The modulus selected during key generation must be sufficiently large to circumvent factoring attacks. In 1996, Menezes et al. [63] recommended a 768-bit or
larger modulus, with 1024-bit moduli for long-term security. Lenstra and Verheul
presented an argument for longer moduli in 1999, concluding that "768-bit RSA
keys will soon no longer offer security comparable to the security of the DES in
1982" [55]. Additional restrictions on the factors may be imposed due to various
factoring attacks; for example, p and q should be roughly the same bitlength but
not so that Ip - q I is particularly small.

=

=

RSA signature scheme (with message recovery)
RSA may be used in digital signatures, interchanging the functions of encryption
and decryption as outlined in Example 12.1.1. Alice's signature on m is s =
md mod n, where dis Alice's private key. On receiving s, Bob computes se mod
n =musing Alice's public key (n, e), obtaining signed message (m, s).
In order to provide a mechanism to detect invalid messages, structural requirements are placed on messages m. A forger can chooses (and obtain corresponding
m = se mod n using Alice's public key), but has little control over the structure of

286

12 Public-key Cryptography

the message m. Bob accepts signed message (m, s) only if the prescribed structure
of m is present.
To reduce signature-verification or encryption times, it is desirable to choose
a small exponent e, although exercises 12.2.4 and 12.2.5 illustrate some concerns
with such a choice. Similarly, a private exponent d which is relatively small would
improve decryption and signature-generation times, but the concern is that the key
itself may be leaked. Under fairly general conditions, Wiener [95] showed that the
private key can be recovered if dis small relative ton, using a continued-fractions
method (see Exercise 12.2.7).
In practice, a scheme based on RSA would be implemented in a more sophisticated fashion than the basic versions described here. Padding of the message before encryption is commonly used to defeat chosen-ciphertext and other
attacks; at a minimum, padding with some randomness has the advantage that
repeated encryptions of a fixed message will likely give different ciphertexts [4].
Boneh [12] surveys two decades of attacks, noting that "none of them is devastating. They mostly illustrate the dangers of improper use of RSA. Indeed, securely
implementing RSA is a nontrivial task."
Exercises
12.2.2 Alice chooses p = 31, q = 4 7, and e = 77 for use in RSA.

(a) What is Alice's private key?
(b) Encrypt the message m = 3 using Alice's public key.
(c) Verify your answer to (b) by decrypting the ciphertext to recover m.
12.2.3 Show that every message is unconcealed if p = 5, q = 17, and e = 33 are
chosen in RSA.
12.2.4 A small encryption exponent in RSA is desirable for encryption efficiency.
Suppose three users have e = 3 and RSA moduli n 1, n2, and n3, resp. An adversary
intercepts ciphertext c; =me mod n;, 1 ~ i ~ 3, from the common message m. If
the moduli are pairwise relatively prime, show how Gauss' algorithm can be used
to recover the message m.

This is an example of a protocol failure in RSA. A larger exponent (such as e =
2 16 + 1 with only a few ls in its binary representation to maintain efficiency) is
one countermeasure; generating a random bitstring and appending to the message
for each encryption, known as salting, is another.
12.2.5 (Related messages and RSA) Coppersmith et al. [24] describe an effective
attack against low-exponent RSA in the case that the underlying plaintext messages satisfy a known linear relationship. Consider the case that the encryption
exponent is e = 3 and that ciphertexts c1 and c2 correspond to messages m and
m + 1 for some (unknown) m; i.e., Ci = m 3 mod n and c2 = (m + 1) 3 mod n.

12.2 RSA

287

(a) Verify that m can be recovered from the ciphertexts by
C2 +2ct -1
C2 - q +2

=m

(mod n).

(b) Note that x- m is a common factor of x 3 - q and (x + 1) 3 - c2. Establish
the formula in (a) by adapting the Euclidean algorithm to find ((x + 1) 3 c2, x 3 - c1 ). Verify that the algorithm will indeed return a linear polynomial.

12.2.6 (Partial key-exposure) Let n = pq where p and q are primes with 5 :=:: p <
q < 2p. Let integers e and d satisfy 1 < e, d < ¢(n) and ed = 1 (mod ¢(n)).
(a) Since ed = 1 (mod ¢(n)), there exists an integer k with ed- k¢(n) = 1.
Prove that 1 :=:: k <e.
(b) Let d = Lkn: 1 J. Prove that ld- dl < 3.,fii.
(c) If e = 3, then prove that k = 2.
(d) Devise an efficient algorithm for computing the leftmost-half bits of d, given
only nand e = 3. (More precisely, the algorithm should narrow the possible
values to one or two.)
The exercise is adapted from [12].

12.2.7 (Small decryption exponent) Let n = pq for odd primes p and q satisfying
p < q < 2p. Let integers e and d satisfy 1 < e, d < ¢(n) and ed = 1 (mod ¢(n)).
Suppose that d < ~ n 114 , and let k be such that ed - k¢ (n) = 1.
(a) Prove that n- ¢(n) < 3.,fii; that is, n is a good approximation of ¢(n).
(b) Prove that ~ is a good approximation of ~. More precisely, prove that I~ k

I

1

d < 2d2"

(c) Using the fact below, devise an efficient algorithm for computing d, given
only nand e.
Fact. Let 1 :=:: xo < yo for integers xo and yo. The number of pairs (x, y) with
1<
y < yo such that I& - ~I < 2 12 is bounded by 2log2 yo. Moreover, all
YO
Y
y
such pairs can be found efficiently (as convergents of the continued fraction
expansion of&; see Hardy and Wright [42]).
YO

The attack is discussed in [12, 14] and is due to Wiener [95].

12.2.8 (Fault analysis attack [13, 47]) Suppose that a smartcard is using the Chinese remainder theorem for RSA decryption; that is, decryption of ciphertext c is
performed by computing mp = cdmod(p- 1) mod p and mq = cdmod(q- 1) mod q,
and then finding m, 0 :=:: m < n, such thatm = mp (mod p) andm = mq (mod q).
(a) Prove that the process provides the correct decryption (i.e., m = cd mod n).
(b) Suppose that the smartcard can be induced to compute mp incorrectly (and
mq correctly). Let m' be the resulting (incorrect) decryption of c. Show how
this information can be used to factor n.

288

12 Public-key Cryptography

12.2.9 A cycling attack against RSA finds the first positive integer k such that
k
ce = c (mod n).
(a) Show that such an integer k exists. Then show that an adversary recovers the
k-1
message with ce
= m (mod n).
(b) The attack can be generalized. Let u be the smallest positive integer such that
(ce"- c, n) > 1. Show that either a nontrivial factor of n has been obtained
or the method has reduced to the basic cycling attack (i.e., u = k).
Cycling attacks are not likely to succeed for n = pq if p- 1 and q - 1 have large
prime factors [63]. The generalized attack is expected to terminate before the
basic cycling, and can be viewed as a factoring attack (and so not likely to be
successful if factoring is intractable).

12.2.10 Verify that RSA key generation could use the universal exponent A(pq) =
lcm(p -1, q -I) in place of ¢(n). Using A may result in a smaller decryption exponentd.
12.2.11 RSA key generation requires large primes p and q. Suppose, in fact,
that a probabilistic primality test produced a "probably prime" p that was actually
composite. Consider the case that p = Pl P2 where PI =!= P2 are primes distinct
from q. Key generation will obtain e and d from ¢ (p, q) = (p - 1) (q - 1) rather
than ¢(n) =(PI- l)(p2- l)(q- 1).
(a) Note thatA = lcm(pJ -1, P2 -1, q -1) I ¢(n). Show that ifA I ¢(p, q) then
encryption and decryption will succeed.
(b) Illustrate (a) with p = 15 and q = 7, finding A, ¢(n), and ¢(p, q).
(c) Suppose p = 21 and q = 5. Show that A f ¢ (p, q). Find d for encryption
exponent e = 3, and exhibit a message m with cd mod n =!= m where c =
me mod n.

12.2.12 This exercise shows that possession of the decryption exponent d in RSA
allows n to be factored efficiently. The main idea is that nontrivial square roots
modulo n of l can be found.
Since ed = 1 (mod¢), it follows that med-l = 1 (mod n) for all mE z~. Write
ed - 1 = 2s t where t is odd. The proof consists of showing that, for at least half
of all m E Z~, there exists r E [1, s] such that m 2'- 11 ¢= ±1 (mod n) and m 2' 1 = 1
(mod n); for such an m, (m 2' - 11 -l,n) is a nontrivialfactorofn.
An m E Z~ fails to lead to a factor if

m 1 =:I (modn)

or

m 2' 1 =-I (modn)forsomeO:Sr<s.

Determining the number solutions to these congruences is the main work. Let
p- 1 = 2; p' and q- 1 = 21 q' for p' and q' odd. Recall that ax= b (mod n) has
a solution x iff (a, n) I bin which case there are exactly (a, n) solutions (Exercise
11.1.20).

12.2 RSA

289

Case m 1 = 1 (mod n). First consider the congruence modulo p. Let a be a
We seek the number of solutions x of (ax)t = 1 (mod p).
generator of
(a) Show that p' divides t.
(b) Note that otxt = 1 (mod p) iff xt = 0 (mod p -1). Show that there are
exactly p' solutions.
(c) Similarly, the number of solutions ofm 1 = 1 (mod q) is q'. Use the Chinese
remainder theorem to show that the number of solutions of m 1 = 1 (mod n)
is p'q'.

z;.

Case m 2' 1 = -1 (mod n). As above, the congruence is first is examined modulo p and then modulo q. We seek the number of solutions x of (ax)2' 1 = -1
(mod p).
(d) Notethata<p-l)/ 2 =-1 (modp)sothat(otx) 2r 1 =-1 (modp)iffx2rt=
(p -1)/2 (mod p- 1). Show that there are solutions iff r < i, in which
case there are exactly 2r p' solutions

We may assume without loss of generality that i ::::; j. The Chinese remainder theorem may be applied, showing that the number of solutions to m 2r 1 = -1 (mod n)
is 22 r p' q' if r < i and 0 otherwise.
An upper bound on the numberofm E Z~ satisfying m2' 1 = -1 (mod n) for some
0 ::::; r < s is then I:~~~ 2 2 r p' q' = p' q' (2 2 i - 1) /3. Necessarily, an upper bound
on the number of m E Z~ which fail to lead to a factorization of n is then
22i -1
2 22i
p'q'( 1 + -3-) = p'q'(3 + 3 ).

Since 2 2i p' q'::::; 2i p'2j q' = (p- 1)(q- 1) = ¢(n), it follows that
2"

p'q'(~ + ~) < if>(n) + if>(n) = if>(n).
3

3

-

6

3

2

Necessarily, for at least half the elements m E Z~ there exists 1 .:::; r ::::; s such that
mY- 11 ¢= ±1 (mod n) and m 2r 1 = 1 (mod n). To factor n, select random mE Z~

and determine if r exists; if so, calculate (my-It -1, n). A nontrivial factor of n
is expected in two trials.
12.2.13 If it is known that ciphertext c =me mod pq was obtained form E [0, p ),
then decryption can be done in Zp, recovering the message as m = cd mod n =
cdmodp-l mod p. Shamir [78] proposed an unbalancedRSA which selects primes
p < q of very different sizes, say 500 and 4500 bits, respectively, with plaintext
messages restricted to the interval [0, p). Factoring attacks on n = pq are defeated, but decryption is as fast as RSA with a 500-bit modulus.
Show that unbalanced RSA is subject to a complete break by a chosen-ciphertext
attack.

12 Public-key Cryptography

290

12.2.14 A "dark side" of black-box cryptography is examined in CRYPTO '96
[102]. The black-box is contaminated with a mechanism that allows the manufacturer to extract secrets, but protects against detection and attacks by others. The
mechanism is known as a Secretly Embedded Trapdoor with Universal Protection
(SETUP).
The authors' most straightforward SETUP. concerns RSA. The idea is to hide
enough information in an RSA public exponent e so that the attacker can factor n.
The black-box normally selects primes pI= q and then produces an RSA key (n =
pq, e, d). The process is contaminated using an attacker's key (n', e'), attempting
to select a prime p for which e = pe' mod n' satisfies (e, ¢ (n)) = 1.
(a) The authors of [102] left a few details to the reader. How does the attacker
factor n from the public key (n, e), and what assumptions have been made?
(b) Speculate on the likeliness that the SETUP will be discovered, perhaps based
on key-generation times or the form of the keys produced.
The SETUP described will not be effective in cases where a small e is required.
The authors give a "Pretty Awful Privacy" SETUP which is similar to PGP (Pretty
Good Privacy [37, 104]), but hides information about p in the modulus n. See
also [103] and Exercise 12.5.6.

12.3

Provable security

The security in RSA is said to be based on the presumed intractability of factoring;
however, systematic recovery of plaintext from ciphertext in RSA is not known to
be as difficult as factoring. It is possible that the integer factorization problem
may be intractable but breaking RSA is easy.
In 1979, Rabin [69] introduced a scheme which is provably secure in the sense
that recovery of plaintext from some given ciphertext is as difficult as solving a
computational problem believed to be intractable. Provable security clarifies and
minimizes assumptions, and allows a more precise definition of security. It should
be emphasized that the proof is relative to the assumptions about the related computational problem-there may be considerable evidence that problems such as
factoring and discrete logs are hard, but none are proven computationally infeasible.
The Rabin scheme is similar to RSA in that the encryption function in both
is of the form f(m) =me mod pq. In RSA, the operation is invertible; however,
Rabin uses e = 2 and computation of square roots recovers the possible choices
form. To be precise, the Rabin scheme chooses primes pI= q, forming public
key n = pq and private key (p, q). The encryption function f : Zn ---+ Zn is
given by f(m) = m 2 mod n. Given ciphertext c, decryption proceeds by solving
c = m 2 mod n for the four possible roots using the techniques described in Section
11.4.

12.3 Provable security

291

Example 12.3.1 Consider the Rabin scheme with toy parameters p = 31 and q =
41. The message m = 814 is encrypted with the public key n = pq = 1271,
obtaining c = f(m) = m 2 mod n = 8142 mod 1271 = 405. To decrypt, the private
key (p = 31, q = 41) is used to extract square roots, solving the congruences

m 2 = 405 = 2

(mod 31)

m 2 = 405 = 36

(mod 41)

form. Section 11.4 shows that this can be done efficiently. In this toy problem,

=

=

=

3 (mod 4) so that the roots modulo p are given by m
±2(p+l)/4 ±8
(mod 31); the second congruence gives m = ±6 (mod 41) by inspection. Gauss'
algorithm 11.1.6 may be applied, obtaining (at most) four possibilities form:
p

m = 8 (mod 31)
m = 6 (mod 41)
m1 = -240

m = -8 (mod 31)
m = 6 (mod 41)
m2 =457

m = 8 (mod 31)
m -6 (mod 41)
m3 = -457

=

m = -8 (mod 31)
m -6 (mod 41)
m4 = 240

=

(Note that only the first two need to be solved; the roots are then ±m1, ±m2.)
Four distinct candidates mi are obtained form. Unless structural requirements are
placed on messages, the receiver is unable to determine that m3 mod n = 814 is
the original message.
To understand the security of the scheme, consider the problem of interest to
an adversary:
RABIN: Given n = pq and c = m 2 mod n, find x so that c = x 2 (mod n).
This is the square root problem discussed in Section 11.4. If p = q = 3 (mod 4),
then n = pq is a Blum integer and square roots can be extracted in polynomial
time. In the example, q = 1 (mod 8), and n is not of the special form. There is no
known polynomial-time deterministic algorithm for solving x 2 = c (mod q) when
q = 1 (mod 8); there is, however, the randomized method discussed in Section
11.4 which runs in expected polynomial time. Hence, RABIN ::::; FACTOR; that is,
given an oracle for FACTOR that runs in expected polynomial time, then we may
solve RABIN in expected polynomial time.
To see that FACTOR ::::; RABIN, recall that if x 2 = y 2 (mod n) and x ¢= ±y
(mod n), then (x + y, n) is a nontrivial factor of n. Choose x E Z~ at random, and
compute c = x 2 mod n. The oracle for RABIN is queried, and a square root y of c
is returned in expected polynomial time. With probability 1/2, the root y satisfies
y ¢= ±x; in this case, (x + y, n) is one of the prime factors of n.
Hence, breaking the Rabin system is equivalent to factoring n. Note, however,
the argument also shows that the scheme is not secure against a chosen-ciphertext
attack. In addition, a protocol failure of the type discussed in Exercise 12.2.4
applies to Rabin.

292

12 Public-key Cryptography

A difficulty in Rabin decryption is that the correct plaintext must be identified
from four candidates. One practical solution is to add structural requirements (e.g.,
replicated bits) to the messages in such a way that it is unlikely that more than
one of the roots will possess the proper structure. This also thwarts the chosenciphertext attack to recover roots (since the oracle returns no useful information
to the attacker in this case). However, the proof of equivalence to the factoring
problem no longer holds for this modified scheme. Exercise 12.3.3 examines a
Rabin-like scheme (on Blum integers n) that permits identification of the actual
message.
Exercises
12.3.2 Let n = 551 in a (toy) Rabin encryption scheme.

(a) Encrypt the message m =53, obtaining ciphertext c = m 2 mod n.
(b) Show the procedure for decrypting c, obtaining four candidates form.
(c) Illustrate a chosen-ciphertext attack to factor n as follows. A random x E
2.551 is selected, say x =53. The value c = x 2 mod n is passed to the oracle
for RABIN. What happens if the oracle returns 498? If the oracle returns
517, show that n can be factored.
12.3.3 This exercise outlines a scheme proposed by Williams [96, 97] for certain
composite n which is similar to that of Rabin. Recovering plaintext from ciphertext is equivalent to factoring n, but the ambiguity in selecting the proper root is
eliminated.

Let n = pq with p = q = 3 (mod 4), and defined= ((p -1)(q- 1)/4 + 1)/2.
Chooses so that(*) = -1. The public key is (n, s) and dis the private key.
Encryption: Given message m with (m, n) = 1, calculate b1 E {0, 1} so that ( ~) =
(-1)b1 and calculate mo = sb 1 m mod n. The triple (c = m6 mod n,b1,b2 =
mo mod 2) is sent.
Decryption: Given the triple (c, b1, bz), calculate mo = ±cd mod n, where the
sign is chosen so that mo = ( -1)b2 (mod 2). Then m = s-b 1 mo mod n is the
recovered text.

(a) If(~)= 1 and c = x 2 mod n, show that cd = ±x (mod n).
(b) Verify that the decryption procedure recovers m.
(c) Let p = 7, q = 11, and n = pq. Note that p = q = 3 (mod 4). Verify that
s = 2 satisfies ( *) = -1. Illustrate the encryption and decryption steps for
the message m = 31.
The basic idea used in this scheme is that a distinguished square root (among the 4
possible) can be found in the case that n = pq with p = q = 3 (mod 4). Following
the notation in Williams' paper, let [a, b] denote the residue class modulo n such
that if y E [a, b] then y =a (mod p) andy= b (mod q).

293

12.4 EJGamal

(d) Show that ( - l ) = -1. Hence, if (c, p) = 1 then exactly one of ±c is a
quadratic resfctue modulo p.
(e) If (y,n) = 1, show that the square roots of y 2 in Zn can be written [a,b],
[-a, -b], [-a, b], and [a, -b], where a E Qp and bE Qq.
(f) If y E [a, b] or y E [-a, -b ], then y is said to be a type 1 root of y 2 ; the
other roots are said to be of type 2. Show that y is type 1 iff ( 7f) = 1; hence
factoring is not necessary to determine type.
(g) If Yl and Y2 are the distinct type 1 roots, then Y1 = - Y2 (mod n) and exactly
one of these roots is even.
An adversary with the ability to decrypt obtains a type 1 and a type 2 root of some
number, and is then able to factor n.
(h) The adversary seeks an x with (~) = -1 and for which the decryption procedure can be applied to x 2 . If every valid ciphertext can be decrypted, then
x = s may be selected. The adversary decrypts (x 2 , 0, 0), obtaining a type 1
root y. Verify that (y - x, n) is a nontrivial factor of n.

12.4 ElGamal
The presumed intractability of the discrete logarithm problem is the basis for encryption and signature schemes proposed by ElGamal in 1985 [29]. The first
recognized digital signature algorithm, adopted by the US Government in 1994
as the Digital Signature Standard (DSS), is a variant of the ElGamal signature
scheme.
Each user
ElGamal encryption requires a prime p and a generator a of
selects a random a, 1 :::; a :::; p- 2, as a private key, and the corresponding public
key is (p, a, a a mod p). To send a message m, 0 :::; m < p, using the public key,
a random k, 1 :::; k < p, is selected, and the pair (ak, m(aa)k) mod pis delivered.
Possession of the private key a allows recovery of m, since a-ak can be computed
and then m(aa)ka-ak mod p = m.

z;.

Example 12.4.1 (ElGamal encryption with toy parameters) Consider the case p
= 13. We may select generator a = 2 of Zi 3 . For the private key a, 1 :::; a :::; 13 - 2,
suppose Alice chooses a= 6. Alice calculates aa mod p = 26 mod 13 = 12, and
publishes (p, a, aa mod p) = (13, 2, 12) as her public key.
To send a message m = 9, Bob selects a random k, 1 :::; k < p, perhaps k = 3.
Using the public key, Bob delivers
(y, 8) = (ak, m(aa)k) mod p = (2 3 , 9(12) 3 ) mod 13 = (8, 4)

to Alice. On receipt, Alice may recover the message with her private key a = 6
by calculating
(ak)-a = y-a = yp-1-a = 813-1-6 = 12

(mod p),

294

12 Public-key Cryptography

and then m = 8a-ak mod p = 4 · 12 mod 13 = 9. Note that key-generation in
this example has given aa = -1 (mod p), which is probably undesirable (see
Exercise 12.4.5).
ElGamal has explicit randomization in its encryption, and a message m will
map to different ciphertexts depending on the choice of the random k. This may
be a desirable feature, making some attacks less feasible. Randomization added to
the message in an RSA-based scheme has similar advantages. On the downside,
ElGamal suffers from message expansion (since ciphertext consists of pairs of
integers each as large as the message itself).
Consider the problem of interest to an adversary:
ELGAMAL: Given prime p, generator a of Z~, aa, ak, and m(aa)k, find m.
Clearly, if there is a polynomial-time algorithm for the discrete log problem, then
ELGAMAL may be solved efficiently; that is ELGAMAL .::: DLP. The security is
said to be based on discrete logs, although it is unknown if ELGAMAL is equivalent to DLP.
Note that the exchange of aa and ak is part of a Diffie-Hellman key agreement to obtain a session key aak; ElGamal then uses the session key to encrypt
the message m by multiplication. An algorithm for the Diffie-Hellman problem
(DHP) immediately solves the ElGamal problem. To see that DHP.::: ELGAMAL,
suppose we have a polynomial-time oracle for the ElGamal problem; that is, given
an instance (p, a, aa, ak, maak), the oracle returns m in polynomial time. In the
Diffie-Hellman problem, we seek aak from (p, a, aa, ak). The oracle for ELGAMAL is given (p, a, aa, ak, 1), and m = a-ak is returned; inverting can be done in
polynomial time, giving aak.
Signatures
The ElGamal signature scheme uses a hash function to map a message m of arbitrary length to a message digest x, which is then signed. Verification requires the
message m as part of the process, and hence ElGamal is an example of a signature
scheme with appendix.
Key generation for signatures is the same as for encryption. Suppose m E
{0, 1}* is to be signed. A known hash function H: {0, 1}*-+ {0, ... , p -1} is used
to obtain a message digest x = H(m). A random k, 1.::: k < p, is selected with
(k, p - 1) = 1, and then r = ak mod p is calculated. The congruence x = ar + ks
(mod p- 1) is solved for s (using the private key a), and the pair (r, s) is the
signature on m.
Verification from the public key uses the fact that ax aar+ks (aay (akf
(mod p) for a valid signature. Given a purported signature (r, s) on m, x = H (m)
is obtained from the known hash function. The signature is accepted if 1 .::: r < p

=

=

12.4 EJGamal

295

and the values of ax and (aaY(ak)s agree (see Exercise 12.4.3 concerning the
first condition).
An adversary wishing to forge a signature on a message m may calculate x =
H(m) and r = ak for any k, but the congruence x = ar + ks (mod p- 1) has
unknowns a and s. However, it is possible to construct an x and signature (r, s)
for which the verification condition ax = (aay (akY holds. Choose integers j, k
with 1 _:::: k < p and (k, p- 1) = 1, and let
r = ai (aa)k mod p,
s = -rk- 1 mod (p- 1), and

x = sj mod (p- 1).

To see that (r, s) is a valid signature on x, note that (a a y rs = aar ai s aaks = ai s =
ax (mod p). The signature would be accepted if a message m which hashes to x
can be found. In this case, the deception is known as an existential forgery, since
the adversary has little control over the content of m.
. A more interesting break of the ElGamal signature scheme would occur if an
adversary could solve ax= (aayrs (mod p) for the signature (r,s). Of course,
if the private key a could be recovered (perhaps from a known signature), then
the scheme is completely broken; however, recovering a from public information
appears to be the discrete log problem. Solving for s in terms of r is also a discrete log problem, and solving for r in terms of s leads to a mixed exponential
congruence in r for which no efficient method of solution is known.
For security, the modulus p must be sufficiently large so that the discrete log
is infeasible. In 1996, Menezes et al. [63] noted that "a 512 bit
problem in
modulus p provides only marginal safety from concerted attack." A modulus of at
least 768 bits was recommended, and 1024 bits or more should be used for longterm security. Unfortunately, the signatures are twice as long as the modulus,
which may be of concern in applications such as smartcards.
The Digital Signature Standard (DSS), adopted in 1994, uses a variant of the
ElGamal scheme. Although the modulus pis 512-1024 bits, a 320-bit signature
is obtained on a 160-bit hash by working in a subgroup of
A brief summary
follows; see [63] for details on the Digital Signature Algorithm (DSA) and the
Secure Hash Algorithm (SHA-1) used as the hashing function.
Key generation in DSA chooses a 160-bit prime q and a prime number p so
that q I p- 1 and so that p has 512 + 64t bits for some 0 _:::: t _:::: 8 (for a ceiling of
1024 bits). Let f3 be a generator ofthe cyclic subgroup of order q in
(see Exercise 12.4.4). A private key a, 1 _::::a< q, is randomly selected; the corresponding
public key is (p, q, {3, f3a mod p ).
To sign a message m, the 160-bit hash x form is obtained. A random k,
1 _:::: k < q, is selected, and then the signature (r, s) is calculated by r = (f3k mod

z;

z;.

z;

296

12 Public-key Cryptography

p) mod q and s = (x + ar )k- 1 mod q. Verification accepts (r, s) as a valid signature on m if the condition

holds (see Exercise 12.4.4).
Exercises

12.4.2 Illustrate the ElGamal signature scheme in this toy example. Suppose Alice chooses p = 17, generator a= 3 of Zh, and private key a= 6; the corresponding public key is then (p, a, aa mod p) = (17, 3, 15). The (toy) hash function H
is given by H (m) = m mod p.
(a) Find the hash value x and signature (r, s) on the message m = 26, assuming
that Alice chooses k = 11 (so that (k, p- 1) = 1 is satisfied).
(b) Show the details of the verification of (r,s) on m. (Show clearly that the
process does not require knowledge of the private key a.)

12.4.3 If the condition 1 .:::: r < p is not checked in the verification of an ElGamal
signature, then a signature on a given message m' can be forged provided a valid
signature (r, s) on some digest x with (x, p - 1) = 1 is known. In this case, let
x' = H(m') and u = x'x- 1 mod (p -1). Defines'= su mod (p -1) and note
that the system
r' = ru

(mod p -1)

r' = r

(mod p)

has a solution r' by the Chinese remainder theorem. Verify that (r', s') is a signature on m' which will be accepted if the requirement that 1 .:::: r' < p is omitted.
(This exercise is adapted from [10]; see also [63, Note 11.66].)

12.4.4 This exercise examines two details in the Digital Signature Algorithm.
(a) Suppose that g E
satisfies f3 = g<P- 1)/q mod p f. 1. Show that f3 has
order q.
(b) If s f. 0, show that the verification condition holds.

z;

12.4.5 In Example 12.4.1, key-generationyielded aa = -1 (mod p). Why is this
a bad choice? (Hint: what is m(aa)k for each choice of k?)

z;
z;

12.4.6 (Weak generators) Suppose p = 1 (mod 4) and the generator a of
satisfies a I p - 1. If computing logarithms in the subgroup G of order a in
is
feasible, then an adversary can construct a signature (r, s) on a given message m
as follows.
Let aa be Alice's public key, and let r be defined by p-I= ar.

12.5 Cryptographic protocols

297

(a) Verify that a' is a generator of G. Hence, it is feasible to find z such that
a'z = (aay (mod p)
(b) Show that r<p-I)/ 2 = -1 (mod p).
(c) Lets= P2 3 (H (m)- rz) mod (p- 1). Verify that (r, s) will be accepted as
a signature on m.
This exercise is adapted from [10]; see also [63, Note 11.67].
12.4. 7 Re-use of the random k in ElGamal could be disastrous.
(a) Suppose that the same k is used for encrypting messages m1 and m2. Show
that an adversary in possession of the ciphertexts and m 1 :f. 0 can efficiently
recover m2.
(b) Suppose that messages m1 and m2 are signed using the same k. Show that
an adversary is likely to recover the private key.

12.5

Cryptographic protocols

A framework of procedures in which cryptographic primitives are applied is
known as a protocol, defined informally in [25} as "an algorithm for implementing a class of transactions (logical units of communication activity)." This short
section introduces several protocol-related topics. Two "classical" problems are
discussed: in the coin-tossing protocol, two parties (who do not trust each other)
wish to flip a coin by telephone to resolve a dispute; the idea in zero-knowledge
is to provide proof of possession of a secret without revealing anything about the
secret itself. The need to clearly identify the assumptions in the protocol is illustrated by an active attack on the Diffie-Hellman key exchange. Even when the
assumptions are clear, it may be unknown if they are in fact met in a specific case;
an example of this type is given by the mental poker protocol (a fair game of
poker without cards) of Shamir, Rivest, and Adleman [79], which was later shown
to leak enough information to (partially) mark the cards.
When a security weakness is discovered, it is not always clear if the protocol
or an underlying cryptographic function deserves the blame. Moore [64] writes:
"The distinction seems to be that when a weakness is reported in a cryptosystem,
the effect of which is to merely limit the scope of application or more clearly define the range of parameters that should be used for the algorithm, then the flaw
discovered probably represents a protocol failure. However, if the effect is to leave
the cryptosystem useless in any setting or to so severely restrict the possible range
of parameters that the definition of a strong cryptofunction is infeasible, then the
cryptosystem is actually 'broken'." Hence, the weakness in Exercise 12.2.4 is a
protocol failure according to Moore. In Exercise 10.3.8, two apparently secure
schemes were combined in such a way that encryption compromised authentication. It can be argued that this is a kind of protocol failure, although Moore's
distinction would label this a broken (by design, actually) cryptosystem.

298

12 Public-key Cryptography

Shamir's 3-pass protocol The difference between a protocol and a cryptographic function is illustrated by Shamir's 3-pass protocol, designed to achieve confidentiality with no advance exchange of keys. A symmetric-key encryption scheme
is selected with the property that Ek 1 Ek2 = Ek2 Ek 1 for all keys k1, k2. Encryption
can be viewed as adding a "lock" to the envelope containing the message. The
following protocol delivers a message m from A to B.
1. A and B pick random private keys kA and kB, respectively.

2. A adds her lock to the message, sending C! = EkA (m) to B.
3. B adds his lock, returning c2 = Ek 8 (cl) = Ek 8 EkA (m) to A.
4. A removes her lock, sending c3 = DkA (c2) to B. B decrypts c3 to recover
m.
Under suitable complexity assumptions on E, it appears that the isolated exchanges are secure from passive attacks. However, the protocol places additional
(implicit) assumptions on the cipher. Consider the case that the cipher is the onetime pad, Ek(m) = kfJ)m. A passive adversary obtains C! = kA tJJm, c2 = kB fJJc1,
and c3 = kB fJJ m. Since C! fJJ c2 fJJ c3 = m, the transfer is totally insecure, even
though an unconditionally secure cipher was employed. Exercise 12.5.1 suggests
an alternative cipher for use in this protocol.

12.5.1

Diffie-Hellman key agreement

In the key agreement described on page 279, Alice and Bob exchange aa mod p
and ab mod p over an unsecured channel, arriving at the shared secret aab. The
scheme is secure against passive attacks, but not against active attacks.
If the keys are not authenticated, then in addition
to impersonation, an active adversary may mount an
intruder-in-the-middle attack which will go undetected by
Alice and Bob. The adversary Eve shares the secret aab'
with Alice and aa'b with Bob, where Eve has selected a'
and b'. If Alice sends a message encrypted with a key
derived from aab', then Eve intercepts the message, decrypts it, re-encrypts with a key derived from aa'b, and
passes it on to Bob. In this fashion, Eve obtains the entire
Intruder-in-the-middle
plaintext correspondence. 4
A trusted party or Certification Authority (CA) is sometimes used to provide
authentication. After verifying the identity of an entity A, theCA forms a message
containing identifying information (name, address, etc.), along with A's public
key (perhaps aa in the present context). The message is signed by theCA, forming
a "certificate" C A· binding A's identity to her public key. Alice and Bob exchange
4See also Exercise 12.5.5 for a suggested protocol to force Eve to act nontransparently.

12.5 Cryptographic protocols

299

their certificates C A and C B, which include aa and ab, respectively. On receipt of
CA, Bob verifies theCA's signature, and is thus convinced that Alice's public key
is aa. Similarly, Alice is convinced that Bob's public key is ab. Impersonation is
still possible in the sense that an intruder may forward a certificate belonging to,
say, Alice, but will not be able to form the shared secret aab.
Use of a CA in this fashion requires an expensive one-time creation of the
certificate for each entity, but the verification of signatures does not require online support from the CA. The CA's public key must be authenticated, but secrecy
is not required. On the downside, a CA providing services to a large number
of clients becomes a very attractive target, and compromise of the CA may be
catastrophic. Also, procedures are needed to distribute revocation information in
the case that individual certificates or CA signatures become invalid.
Certificates are commonly used in secure purchases via internet. The authentication is typically one-way (the merchant delivers a certificate to the client),
since client-side certificates are rare in this context. The client, perhaps using a
web browser such as Netscape, maintains a list of Certification Authorities from
which signatures will be accepted. There is a rather optimistic protocol assumption that the person on the client-side will actually examine the certificate. A valid
certificate from http://www.delta.com offers convincing evidence that the public
key for www.delta.com has been obtained; however, the person expecting to purchase plane tickets may be surprised to learn that Delta Air Lines is at the address
www.delta-air.com. 5

12.5.2 Zero-knowledge proofs
Alice (the prover) wishes to convince Bob (the verifier) that she possesses a secret
s. One option open to Alice is to simply reveal the secret-of course, it is then no
longer her secret. In the context of the password authentication scheme discussed
in Section 10.3.2, an adversary obtains Alice's password, and can then impersonate Alice. Zero-knowledge protocols are designed to allow the prover to provide
convincing evidence that she possesses the secret, without revealing anything that
could be used by the verifier in subsequent demonstrations.
A rigorous discussion of zero-knowledge in interactive protocols involves nontrivial and somewhat subtle concepts. Readers interested in more formal or extensive treatments than presented in this brief overview should consult the references
mentioned in Section 12.6 (where two non-mathematical introductions to zeroknowledge concepts are also noted).
As an illustration of (not necessarily zero-knowledge) proof systems, consider
the problem of proving that a given v E In is a quadratic nonresidue, where n is a
product of large and secret primes. A prover in possession of the factorization of
5deltaComm remarks that 8000 visitors per day access www.delta.com looking for Delta Air. In
1999, modifications in their web pages were made to minimize the data-transfer cost of the unwelcomed accesses.

300

12 Public-key Cryptography

n could offer irrefutable proof that v E Qn by exhibiting the factors. Considerably

more than simply "v is indeed a quadratic nonresidue" has been revealed to the
verifier, however.
. A more interesting proof system for the problem is given in [40]. In this context, "proof" is understood to mean "convincing evidence" rather than certainty.
1. The verifier B chooses a security parameter t > 0, and random bi E {0, 1}
and Zi E Z~, 1 ~ i ~ t. B then challenges the prover A with the values
b· mo d n, 1 ~ z. ~ t.
Wi = zi2 v'
2. A determines (somehow) if each Wi is a quadratic residue, and gives the
response Ci = 0 if Wi is a quadratic residue and Ci = 1 otherwise, 1 ~ i ~ t.
3. B checks if bi = Ci for every i; if so,. B accepts the proof (that v is a
quadratic nonresidue ).
If v is a quadratic nonresidue, then the challenge Wi is a quadratic residue if and
only if bi = 0. If t is sufficiently large, and both parties follow the protocol, then
B will be convinced that v is a quadratic nonresidue. On the other hand, if v is in
fact a quadratic residue, then every Wi is a quadratic residue and the probability
that bi = Ci for every i is apparently 2- 1 , making it unlikely that B will accept a
claim that v E Qn.
If both parties follow the protocol, then B learns nothing beyond what he could
have calculated without A (other than the fact that prover is indeed able to respond
successfully to the challenges). The protocol is not zero-knowledge, however. The
verifier is not forced to follow the protocol, and may obtain responses for Wi of his
choice; that is, the verifier is able to use the prover (provided the prover is not also
cheating) to determine if elements of his choice (and not necessarily of the form
in the protocol) are quadratic residues. A zero-knowledge version of this protocol
in [40] essentially forces the verifier to play fair.
As mentioned above, one application of zero-knowledge concepts is in identification. The following protocol is built on a zero-knowledge proof that a known
value vis a quadratic residue modulo n.
Protocol (Fiat-Shamir identification) A trusted center chooses an RSA-like modulus n = pq, keeping the factors private. The prover A obtains a secret s E Z~
from the trusted center; v = s 2 mod n is A's public key. The prover convinces the
verifier B of her possession of s by successfully completing the following 3-pass
protocol t times, where t > 0 is a security parameter.
1. A chooses a random commitment r, 1 ~ r < n, and sends the witness x =
r 2 mod n to B.
2. B replies with a random challenge e E {0, 1}.
3. A's response is y = rse mod n.
4. B verifies that y f. 0 and y 2 = xve (mod n).
B accepts the proof if all t rounds succeed.

12.5 Cryptographic protocols

301

The verifier learns nothing about s: the response y = r is independent of s, and
the random r in the response y = r s is unknown to B. In each round, an adversary
impersonating A can cheat successfully if the challenge can be predicted: if e = 0
is expected, the witness and response are unchanged; if e = 1 is expected, then x =
r 2 v- 1 is selected as the witness andy= r as the response. Since the challenges are
selected at random, the probability of a successful prediction is only 1/2 for each
round. Exercise 12.5.2 shows that such an imposter is likely to be detected. This
inability to predict the challenges also thwarts a replay attack from an adversary
in possession of a transcript from a valid protocol session.

12.5.3

Coin-tossing and mental poker

Two additional cryptographic protocols are considered in this section, giving a
glimpse of the wide range of applications addressed by such protocols, and further emphasizing the need to identify protocol assumptions and verify that these
requirements are met. Section 12.6 contains references to many other applications
and attacks on such protocols.
Coin-tossing by telephone was suggested by Blum [11]. Alice and Bob have
experienced a messy divorce, and wish to flip a coin by telephone to see who gets
the kids. The predicament seems to be that neither party is willing to go first in
declaring their call or in announcing the result of the toss. The basic idea in the
following protocol is that Alice commits to "heads" or "tails" and announces the
commitment in a way that conceals the choice but binds her to the selection. Bob
makes his guess at her choice, and then Alice reveals information which makes
the commitment public. The security depends on the intractability of the quadratic
residuosity problem.

Protocol (coin-tossing)
1. Alice selects n = pq for odd primes p f= q and a random x E In, and announces n and x to Bob.
2. Bob responds with either "x E Qn" or "x E Qn" (with 50% chance of being
correct, assuming QRP is hard).
3. Alice reveals p and q. Bob verifies that p and q are indeed prime (and that
x E In); the correctness of Bob's response is determined by(~).
If Bob fails to verify that p and q are prime, then Alice can cheat by forming
n = Pl P2P3 for primes p;, and choosing x with (
= ( ; 2 ) = -1 and (
= 1
(so that (~) = 1). Once Bob announces his guess, then Alice reveals the pair
(p = Pl p2, q = p3) or (p = Pl, q = P2P3), depending on the outcome she desires
(e.g., if she wants Bob to think that x is a quadratic residue, then she reveals the
former).

;J

;J

302

12 Public-key Cryptography

Mental poker Cryptographers are apparently quite interested in a fair game of
poker without cards. In 1979, Shamir, Rivest, and Adleman suggested a protocol
for dealing, published in The Mathematical Gardner in 1981. The basic idea is
to use Shamir's 3-pass protocol. Alice and Bob agree on a set of messages mi,
1 ::::; i ::::; 52, identifying the 52 cards. Alice encrypts these messages, sending
EkA (mi), 1 ::::; i ::::; 52, in random order. Bob selects five ciphertexts as Alice's
hand, and returns them to her. He encrypts an additional five ciphertexts with
Ek 8 , sends these to Alice, who removes her locks with DkA and returns the results
as Bob's hand.
Encryption with RSA-like key-generation was suggested. Alice and Bob agree
on a modulus n (prime or a product of distinct odd primes), and each selects a
secret key-pair k = (e, d) with (e, ¢(n)) = 1 and ed = 1 (mod ¢(n)), and then
Ek(m) =me mod nand Dk(c) = cd mod n. The key-pairs are revealed at the end
of the game.
Unfortunately, shortly after the scheme was proposed, Lipton showed that the
suggested function fails to meet an implicit assumption in the protocol, that the
cards cannot be marked [25]. The Jacobi symbol can be efficiently calculated, and
the value is preserved by encryption; i.e.,(~)= (~r =('':,e). Unless the Jacobi
symbol gives the same result for every mi, Bob can select cards with a specific
Jacobi symbol to return to Alice, possibly offering an advantage. This particular
attack may be defeated by choosing encodings mi which are all quadratic residues.
The interested poker player may find references in Section 12.6 for suggested
protocols and ways to cheat.

Exercises
12.5.1 In Shamir's 3-pass protocol, suppose that Ek(m) = mk mod p for suitable
prime p.
(a) Verify that the encryption scheme meets the commutativity requirement for
use in the protocol.
(b) How will kA and kB be selected? Find C], c2, and C3 in the exchange.
(c) Discuss the security.
12.5.2 This exercise concerns the Fiat-Shamir identification protocol.

(a) Show that if an impersonator successfully predicts a challenge e, then the
witness x and response y will be accepted by the verification step.
(b) If the prediction turns out to be incorrect, explain why the impersonator is
likely to be discovered.
12.5.3 (Proof of possession of a discrete log [20, 19]) Let p be prime and g an
element of prime order q in
Entity A has private key s, 1 ::::; s < q, and public
keyS= gs mod p. A proves knowledge of s toBin t > 0 rounds.

z;.

12.5 Cryptographic protocols

303

1. A chooses random commitment x, 1 :::; x < q, and sends the witness X =
gx mod p to B.
2. B replies with a random challenge e E {0, 1}.
3. A's response is y = x if e = 0, or y = sx- 1 mod q if e = 1.
4. B checks that X = gY mod p if e = 0, or that S = XY mod p if e = 1.
B accepts the proof if all t rounds succeed.

(a) If both parties follow the protocol, verify that B will accept.
(b) Indicate how an imposter can cheat, assuming that the challenges can be
predicted (buts is unknown).
(c) Discuss the dilemma faced by an adversary in the case that a prediction turns
out to be incorrect.

12.5.4 Consider the following proposed identification scheme. A certification
authority binds Alice's identity ton= pq, where n is public and the primes p i:. q
are Alice's secret.
1. Bob challenges Alice with x, a random quadratic residue modulo n.
2. Alice's response is a square root y of x.
3. Bob verifies that y 2 = x (mod n).
If several rounds are performed successfully, will Bob be convinced that Alice
possesses the secret? Explain why this scheme is seriously flawed.

12.5.5 (Key agreement in the presence of active adversaries) Suppose that Alice
and Bob can rely on voice recognition during key agreement. Rather than using
the Diffie-Hellman shared secret aab directly, Alice and Bob establish a voice
connection, and each reads a portion of aab. After verifying these pieces of aab,
the remaining portion becomes the shared secret k.
(a) Assume that the adversary cannot efficiently recover k from aa, ab, and the
leaked portions of aab. Will Alice and Bob be convinced that the adversary
does not know k?
Rivest and Shamir [72] suggest a protocol change which forces an intruder-in-themiddle to act non-transparently (and thus possibly revealing her presence). Alice
is to select a message m and send half of Ek(m), where k is the key that she
has calculated (possibly shared with the intruder rather than with Bob); Bob will
reply with half of the ciphertext for his selected message. Alice then transmits the
remaining part of Ek(m), and Bob responds similarly.
(b) Assume that fragments of plaintext cannot be discovered from possession of
only half of Ek(m). Discuss the options open to the adversary (in particular,
describe what happens if she simply forwards the half of Ek(m) from Alice's
first transmission).

304

12 Public-key Cryptography

12.5.6 (Subliminal channel) Authentication schemes may permit an undetectable
covert channel between parties. Simmons [82, 83] suggests the following scenario.
Prisoners' problem. Two accomplices in a crime have been arrested and are
about to be locked in widely separated cells. Their only means of communication after they are locked up will be by way of messages conveyed for them
by trustees-who are known to be agents of the warden. The warden is willing
to allow the prisoners to exchange messages in the hope that he can deceive at
least one of them into accepting as a genuine communication from the other
either a fraudulent message created by the warden himself or else a modification by him of a genuine message. However, since he has every reason to
suspect that the prisoners want to coordinate an escape plain, the warden will
only permit the exchanges to occur if the information contained in the messages is completely open to him-and presumably innocuous. The prisoners,
on the other hand, are willing to accept these conditions, i.e., to accept some
risk of deception in order to be able to communicate at all, since they need to
coordinate their plans. To do this they will have to deceive the warden by finding a way of communicating secretly in the exchanges, i.e., of establishing a
"subliminal channel" between them in full view of the warden, even though the
messages themselves contain no secret (to the warden) information. Since they
anticipate that the warden will try to deceive them by introducing fraudulent
messages, they will only exchange messages if they are permitted to authenticate them.

The ElGamal signature scheme allows such a subliminal channel to be established
between prisoners Alice and Bob. Key-generation for Alice remains unchanged:
Alice chooses prime p, generator a of Z~, and a private a, 1 ::=:a ::=: p- 2; her
public key is (p, a, aa mod p).
Normally, Alice signs a message m by selecting a random k < p with (k, p- 1) =
1 and computing (r, s), where r = ak mod p and sis obtained from H(m) = ar+
ks (mod p -1); then (m, r, s) is delivered to Bob through the warden. However,
if Alice can arrange to share the secret a with Bob, then k may be used to transport
the hidden message. 6
(a) If Alice and Bob share a, identify conditions on s which will allow Bob to
efficiently recover the subliminal message k from (m, r, s).
Given her current predicament, Alice may be willing to share her secret a with
Bob (even though it means that Bob will be able to forge her signature), although
6 Although an E!Gamal signature (r, s) may require 21og 2 p bits, there are only log2 p bits of security; the remaining bits are available for the subliminal channel. However, since k is relatively prime
to p - I, only <P (p - 1) distinct messages k may be sent, and it may be infeasible to recover those
for which xs o= H(m) -ar (mod p -1) has many solutions x. Simmons [85, 84] observes that these
"shortcomings" are avoided in the DSA, the "most hospitable setting for subliminal communications
discovered to date."

12.6 Notes

305

a method of securely delivering a is required. Young and Yung describe a SETUP
(see Exercise 12.2.14) which may allow Alice to leak her private key to Bob,
provided that Bob has a public key (p, a, ab mod p) known to Alice. Alice contaminates her signing process, producing (m1,q,sJ) and (m2,r2,s2) in such a
way that possession of b permits recovery of a.
To leak a, Alice attempts to select a random k1 with k 1, f3 = (ab)k1 mod p, and
ar 1 mod p relatively prime top -1, where {3- 1 satisfies {3{3- 1 = 1 (mod p -1).
Rather than choosing k2 at random, Alice selects k2 = {3- 1. Finally, let r; =
aki mod p and s; = (H (m;)- ar;)kj 1 mod (p- 1), i E {1, 2}, as usual.

(b) Show that Bob can recover a by calculating r,;\H(m2) -s2/(rf mod p))
mod (p-1).
(c) As an illustration, consider the toy example with p = 13 and a = 2. Verify
that f3 and ar 1 mod p are relatively prime to p- 1 if b = 5 and k 1 = 7.
If b = 3, then show that Alice is unable to find k1 satisfying the desired
properties.

12.6

Notes

In addition to the schemes covered here, cryptographic methods based on elliptic
curves have attracted considerable interest, in part because security is apparently
maintained with shorter keys when compared with, say, RSA. The Elliptic Curve
Digital Signature Algorithm (ECDSA, an elliptic curve analogue of the Digital
Signature Algorithm) was accepted by the American National Standards Institute
in 1999 (ANSI X9.62). Johnson and Menezes [44] discuss design decisions and
related security, implementation, and interoperability issues for ECDSA; their paper also includes a short primer on elliptic curves. Koblitz [50] and Stinson [86]
contain introductions to elliptic curves and applications to cryptography; more advanced texts on elliptic curve methods include Blake, Seroussi, and Smart [8] and
Menezes [62].
Public-key methods were known to British cryptographers in the early 1970s,
according to technical reports released by the Communications-Electronics Security Group (CESG) in December 1997 [31]. Known as "non-secret encryption"
within CESG [30], an RSA-like scheme appears in Cocks [21] and the ideas of
the Diffie-Hellman key agreement are given in Williamson [100]. 7
7CESG remarks "It is interesting to note that although many different ideas for public-key systems
have been proposed, the two which have stood the test of time were the first two to be discovered. It
is also interesting that the order of discovery in academia was the opposite to the order of discovery
at CESG." Williamson [100] offers a few cautionary words in his introduction: "One of the reasons
for the delay in writing this is that I find myself in an embarrassing position; having written [99], I
have come to doubt the whole theory of non-secret encryption. The trouble is that I have no proof that
the method of [99] is genuinely secure, in other words that it has a guaranteed work-factor involved in
breaking it."

306

12 Public-key Cryptography

Well-known applications that combine public-key and symmetric-key methods to provide authentication and confidentiality include Pretty Good Privacy
(PGP) [37, 104] and (at the systems level) Sun Microsystems' remote procedure
call (RPC) mechanism [88, 70]. It is interesting to note that a portion of the security in Sun's scheme rested on exponentiation modulo a 192-bit prime, considered
insecure even in 1991 [54]. Vulnerability assessments must consider the larger
framework of network security, which involves much more than cryptographic
primitives. Indeed, security problems often do not directly involve cryptography.
The 1990s saw a flood of security alerts and system compromises due to buffer
overruns (caused by contrived messages which exceed implicit length assumptions) and shell escapes (privileged processing triggered by messages containing
unexpected characters), some of which were in the very programs implementing
security mechanisms.
The material on zero-knowledge is adapted from Goldwasser, Micali, and
Rackoff [40] and Menezes, van Oorshot, and Vanstone [63]; see also Chapter
13 of Stinson [86]. Brassard and Crepeau [18] sort out various notions of zeroknowledge.
Interesting (or at least amusing) non-mathematical introductions to zeroknowledge have been presented at CRYPTO rump sessions. Quisquater, Guillou,
and Berson [68] explore "The Strange Cave of Ali Baba," which has an entry forking to passages with apparent dead ends. An interactive test is devised so that a
claimant can provide convincing proof of possession of magic words which open
a corridor between the dead ends, without revealing the secret itself. The claimant,
who has entered the cave alone some time earlier, is asked to return using a passage selected by a witness placed at the fork. The process is repeated until the
witness is convinced that the claimant possesses the magic words. 8
In the CRYPTO '98 rump session [65], zero-knowledge was introduced with
the help of Where's Waldo [41], a game where a certain figure (Waldo) is to be
located within a complicated illustration. Alice wishes to prove to Bob that she
has found Waldo, but does not want to reveal the solution. She could use her
scissors to carefully remove Waldo from the background, but perhaps Bob will
not be convinced, accusing her of using an image from some other source. Alice's
solution is to use an opaque sheet of paper, twice the size of the illustration, with
a small window in which to isolate Waldo.
Overviews on protocols (and attacks) for applications ranging from secret
sharing and subliminal channels to anonymous digital cash and voting schemes
appear in Salomaa [75], Seberry and Pieprzyk [77], and Schneier [76]. Simmons
[84] gives a survey of subliminal channels, beginning with a historical introduction concerning verification proposals for the Strategic Arms Limitation Treaty
(SALT II). Suggested protocols and ways to cheat at mental poker may be found
in Fortune and Merritt [33] and Coppersmith [22] and the references therein.
8The proof could have been provided in one step by asking the claimant to complete the loop
beginning at the fork, rather spoiling the story (and the analogy with mathematical zero-knowledge).

