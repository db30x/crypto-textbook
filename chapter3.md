Chapter 3

Perfect and Related Codes
3.1

Some bounds for codes

We now turn our attention to the problem of determining how many words a linear
code of given length n and distanced can have. This problem is far from solved in
general, though it has been settled for certain values of n and d. We can, however,
find some bounds on the size of a code with these given parameters.
Recall that if t and n are integers, 0 _: : : t _: : : n, then the symbol

(n)
t

n!
- t!(n-t)!'

is just the number of ways that an unordered collection oft objects can be chosen
from a set of n objects. Thus G) is the number of words of length n and weight t.

Theorem 3.1.1 If 0 _: : : t _: : : n and if v is a word of length n, then the number of
words of length n of distance at most t from v is precisely

Since there are 2n words of length n, setting t = n in Theorem 3.1.1 yields

Exercises
3.1.2 Illustrate Theorem 3.1.1 for v = 10110 and t = 3 by listing all words in K 5
of distance at most 3 from v, and then check that Theorem 3 .1.1 does give the
correct number of such words.
To find all words of a given distance t from a fixed word v, we simply add
to v, all words of weight t. There are
such words. If Cis a code of length

G)

65

66

3 Perfect and Related Codes

n and distance d = 2t + 1, then there is no word w at distance at most t from
two different codewords VJ and v2. Indeed, if d(w, vi) ::=:: t and d(w, v2) ::=:: t with
Vi =F v2, then

which is impossible since C has minimum distance d. Thus, if C has length n and
distance 2t + 1, the list of words in Kn at a distance at most t from a codeword
VJ has no codewords in common with the list of words a distance at most t from a
codeword v2, VJ =F v2. This establishes the following result.

Theorem 3.1.3 (Hamming bound) If C is a code of length n and distance d =
2t + 1 or 2t + 2 then

IC I {{~) + {~) + ··· + {7)) ::=:: 2n,
or

2n

ICI <

- (~) + (~) + ... + (7)

.

The Hamming bound is an upper bound for the number of words in a code
. (linear or not) oflength nand distanced= 2t + 1. Note that t = L(d- 1)/2J, so,
by Theorem 1.12.9, such a code will correct all error patterns of weight less than
or equal to t.

Example 3.1.4 We compute an upper bound for the size or dimension k of a linear
code C of length n = 6 and distanced= 3. From d = 3 = 2t + 1 we get t = 1.
The Hamming bound gives
26

64

64

ICl ::::: (~) + (~) = 1 + 6 = 7

But ICI must be a power of2, so ICI ::=:: 8, and thus k ::=:: 3.
Exercises

3.1.5 Find an upper bound for the size or dimension of a linear code with the
given values of n and d.
(a) n = 8, d = 3
(d) n = 15, d = 3

(b) n=7,d=3
(e) n = 15, d = 5

(c) n = 10, d = 5
(f) n = 23, d = 7

3.1.6 Verify the Hamming bound for the linear code C with the given generator
matrix.
111110000000000]
(a) G = [ 000001111100000
000001111111111

100111]
(b) G = [ 010101
001011

1000111]
0100110
(c) G = [ 0010101
0001011

67

3.1 Some bounds for codes

Recall that from Section 2.7 and Theorem 2.9.1 we know that the parity check
matrix H of an (n, k, d) linear code is ann by n- k matrix such that every d -1
rows of H are independent. Since the rows have length n- k, you can never have
more than n - k independent row vectors. Hence d - 1 _:::: n - k or equivalently k _::::
n - d + 1. This establishes the following result, which is known as the Singleton
bound.
Theorem 3.1.7 (Singleton bound) For any (n, k, d) linear code, d- 1 _:::: n- k.

The Singleton bound in one sense is much weaker than the Hamming bound.
For example, if n = 15 and d = 5 then Theorem 3.1.7 implies that k _:::: 11, whereas
Theorem 3.1.3 (Hamming bound) implies that k _:::: 8. However some codes do
attain equality in the Singleton bound, so the Singleton bound is used to define an
important and useful class of codes called maximum distance separable codes.
A linear (n, k, d) code is said to be a maximum distance separable (or MDS)
code if d = n- k + 1 (or k = n- d + 1). There are several equivalent characterizations of MDS codes.
Theorem 3.1.8 For a (n, k, d) linear code C, the following are equivalent:
(1) d=n-k+1,

(2) every n- k rows of the parity check matrix are linearly independent,
(3) every k columns of the generator matrix are linearly independent, and
(4) Cis MDS.

Proof: Theorem 3.1.7 states that d _:::: n- k + 1; but d ~ n- k + 1 iff every n- k
rows of the parity check matrix are independent. Thus (1) and (2) are equivalent.
For (3), note that if d = n- k + 1, no nonzero codeword can have more than
k - 1 zeros in it. However, k columns of the k x n generator matrix are linearly
dependent iff some nonzero codeword has k zeros in those coordinate positions.
The last statement is relatively easy to see and is left to the exercises.
D
Corollary 3.1.9 The dual of an (n, k, n- k + 1) MDS code is an (n, n- k, k + 1)
MDScode.

We will encounter MDS codes later when we study Reed-Solomon codes.
Exercises

3.1.10 Columns 2, 3, and 5 of the generator matrix
11001]
G = [ 01110
00101

are linearly dependent. Find a codeword which has zeros in positions 2, 3, and 5

68

3 Perfect and Related Codes

3.1.11 Show that if a k x n generator matrix has k linearly dependent columns
then there is a nonzero codeword with zeros in those k positions.

We still would like to construct codes for given parameters n, k and d. Upper
bounds rule out some parameter values for example the Hamming bound says
that a code of length n = 15 and distanced= 5 can not have dimension k = 10.
However, this bound does not rule out the possibility of a (15, 8, 5) code existing.
How would we go about finding a (15, 8, 5) code? In general this is a very
difficult problem. One approach is to find the parity check matrix for such a code.
That is, assuming r = n- k, we must find n vectors of length r to form the rows
of H such that every set of d - 1 of these vectors is linearly independent.
Example 3.1.12 Let n = 15, k = 6, and d = 5. Then r = 15-6 = 9. So we
wish to find 15 nonzero vectors of length 9 with the property that any 4 of these
are linearly independent. Finding the first 9 rows is easy: take the 9 x 9 identity
matrix /9.
Suppose we have some how found 3 more vectors for a total of 12 rows and
so we have,
111100000
/9

H = [ 100011100

J

101000011
?

Before searching for the next vector we notice that the following counting
argument tells us that one must exist. Among all 29 vectors, we cannot select the
zero vector nor any of the 12 choosen so far. This rules out 1 + 12 vectors. We
also rule out any vector which can be written as the sum of 2 or 3 of these vectors,
as this would create a dependent set of 3 or 4 vectors respectively. This rules
out at most (~) + 32) additional vectors. However, any remaining vector can be
selected.
Since
1 + ( 12)
1 + (12)
2 + (12)
3 < 29

C

we know that we can find yet another vector. For example one could choose the
vector 010101010 to be the next row of H. The chore of finding the remaining
rows of H is left to Exercise 3 .1.21.
Example 3.1.12 (and the related exercises) show that a (15, 6, 5) code exists.
This establishes a lower bound on the maximum size (or dimension) of a linear
code with n = 15 and d = 5, i.e., 6::::: k ::::: 8.
The next result formalizes the approach of Example 3.1.12 to constructing
linear codes (and thus establishing lower bounds). The proofs are left to Exercise
3.1.22.

69

3.1 Some bounds for codes

Theorem 3.1.13 (Gilbert- Varshamov bound) There exists a linear code of length
n, dimension k, and distanced if

(n ~ 1) + (n ~ 1) + ... + (: =~)

< 2 n-k.

Corollary 3.1.14 Ifn f. 1 and d f. 1, then there exists a linear code C oflength
n and distance at least d with

ICI C':: (n(Jl) + (njl) + ... + (~=~) ·
Example 3.1.15 Does there exist a linear code of length n = 9, dimension k = 2,
and distanced= 5?
To determine if such a code exists, we use the Gilbert-Varshamov bound:

(n~ 1) +···+ G=~) =(~)+G)+ G)+ G)
=93
and 2n-k = 29- 2 = 27 = 128. Since 93 < 128, such a code exists.
Example 3.1.16 What is a lower and an upper bound on the size or the dimension,
k, of a linear code with n = 9 and d = 5?
To find a lower bound for the most number of codewords such a code C could
have, we use Corollary 3.1.14:
2n-1

29-1

28

256

ICI:::: (n()t) + ... + (~=~) = @+ (~) + (~) + (~) = 93 = 93 = 2.75.
Since IC I is linear, it is a power of 2, and thus IC I C:: 4.
To find an upper bound for IC I, we use the Hamming bound:
29

512

512

ICI::; (~) + (i) + (~) = 1 +9+36 = 46 = 11.13.
Since IC I is linear, it is a power of 2, and thus IC I ::; 8.
Combining the bounds, a linear code with parameters (9, k, 5) with 4 codewords exists, but no (9, k, 5) linear code with more than 8 codewords exists.
Example 3.1.17 Does there exist a (15, 7, 5) linear code? Again we can try to use
the Gilbert-Varshamov bound to answer this question.

C~

1
) + ···+

G=~)

4
= Co ) +

C:)

4
4
+ C2 ) + C3 )

= 1 + 14+91 +364
=470,

3 Perfect and Related Codes

70

and 2n-k = 2 15 - 7 = 256. In this case the inequality is not satisfied, so the GilbertVarshamov bound does not tell use whether or not such a code exists. In fact, as
we shall see later, these are the parameters of the 2 error-correcting BCH code, so
such a code does exist.
Exercises
3.1.18 For each part of Exercise 3.1.5, let k = 2d and decide, if possible, whether
or not a linear code with the given parameters exists. Find a lower and upper
bound for the maximum number of codewords such a code can have, assuming
that k is unrestricted.
3.1.19 Find a lower and an upper bound for the maximum number of codewords
in a linear code of length n and distance d where

(a) n = 15, d = 5
(d) n = 12, d = 3

(b) n = 15, d = 3
(e) n = 12, d = 4

(c) n = 11, d = 3
(f) n = 12, d = 5.

3.1.20 Is it possible to have a linear code with parameters (8, 3, 5)?
3.1.21 Find a (15, 6, 5) code by constructing the parity check matrix. (See Example 3.1.12, each of the 3 missing vectors must have weight at least 4. Why?)
3.1.22 Let H; be any i x (n- k) matrix with nod- 1 rows linearly dependent.

(a) Prove that there are at most

words in Kn-k which are linear combinations of at most d- 2 rows of H;.
(b) Prove that if N; < 2n-k, then a row can be added in such a way that no d- 1
rows of the resulting matrix are linearly dependent.
(c) Prove the Gilbert-Varshamov bound.
(d) Prove Corollary 3.1.14.

3.2 Perfect codes
A code C of length n and odd distance d = 2t + 1 is called a perfect code if C
attains the Hamming bound of Theorem 3.1.3; that is, if

IC\ = (~) +(~)+ ... +G)"
Unfortunately, there are not many linear perfect codes, but the ones that do exist
are quite useful. The main problem in finding linear perfect codes is that the
number (~) + G) + · · · + G) must be a power of 2 (since IC\ is a power of 2).

3.2 Perfect codes

71

Example 3.2.1 Lett= 0. Then(~)= 1 = 2°, so ICI = 2n /(~) = 2n. The only
code with 2n codewords of length n is C = Kn. Kn is a perfect code.
Example 3.2.2 Let n = 2t + 1. Then

n ) = (n-i)!(n-(n-i))!
n!
n!
(n)
(n-i
= (n-i)!i! = i ·
Thus

(~)=(:). G)=(n:1)' (;)=(n:2), ... ,

and, from n = 2t + 1,

G)= (n:t) =C:1)·

Therefore

Hence

2n

2n
ICI =(~)+···+G)= 2n-1 =2.

Thus any perfect code of length and distance 2t + 1 has exactly 2 codewords.
Among linear codes there is only one such code, the repetition code consisting
of the zero word and the word in which each digit is 1, and indeed this code is
perfect.
The codes in Examples 3.2.1 and 3 .2.2, while perfect, are not very interesting.
They are called the trivial perfect codes.
Example 3.2.3 Let n = 7 and d = 3. Then t = 1 and

27

IC1 = (b) + (i) =

128

4

s = 16 = 2 .

Thus, there may exist a linear perfect code with n = 7 and d = 3. In the next
section we shall see that there is such a code, the Hamming code.
Example 3.2.4 Let n = 23 and d = 7. Then t = 3, and

223

223

ICI = eti)+e?)+(~)+ei) = 1+23+253+1771
223
223
= - - = - = 2 12 =4096.
2048
2 11
This shows that a linear perfect code with n = 23 and d = 7 may exist. In a later
section, we shall see that such a code does exist, namely the Golay code.

3 Perfect and Related Codes

72

Exercises
3.2.5 Show that for n = 2r - 1, (3) + G) = 2r.
3.2.6 Can there exist perfect codes for the following values of n and d?
(a) n = 15, d = 3
(b) n = 31, d = 3
(c) n = 15, d = 5
The possible lengths and distances for a perfect code were determined by
Tietavairen and van Lint in 1973. The proof of their result is beyond the scope of
these notes.
Theorem 3.2.7 If C is a non-trivial perfect code of length n and distanced =
2t + 1, then either n = 23 and d = 7, or n = 2r - 1 for some r 2: 2 and d = 3.
If a linear code of length n has distanced= 2t + 1, then, by Theorem 1.12.9,
C will correct all error patterns of weight less than or equal tot = (d- 1) /2. Thus
every word of length n and weight less than or equal to t is a coset leader. There

are precisely (3) + G) + ··· + G) such words. But this is precisely the number of
cosets if the code is perfect. We have proved another theorem.
Theorem 3.2.8 If C is a perfect code of length n and distanced = 2t + 1, then C
will correct all error patterns of weight less than or equal to t, and no other error
pattems.
We can interpret Theorem 3 .2.8 as saying that each of the 2n words in K n lies
within distance t of exactly one codeword. This property enables us, for example,
to count the number of codewords of minimum non-zero weight in a perfect code.
A perfect code which corrects all error patterns of weight less than or equal to
tis called apeifect t-error correcting code. From Theorem 3.2.7 the only possible
values fort here are t = 1 and t = 3. We examine the case t = 1 in the next section .

.3.3

Hamming codes

Finally it's time to design a code. We consider an important family of codes which
are easy to encode and decode, and which correct all single errors.
A code of length n = 2r - 1, r 2: 2, having parity check matrix H whose rows
consist of all nonzero vectors of length r is called a Hamming code of length
2r -1.
Example 3.3.1 One possiblity for a parity check matrix for a Hamming code of
length 7 (r = 3) is
111
110

101
H=

011
100
010
001

3.3 Hamming codes

73

From Algorithm 2.5.7, a generator matrix for Hamming code oflength 7 is, therefore,
G =

[

1000111]
0100110
0010101 .
0001011

Thus the code has dimension 4 and contains 24 = 16 codewords. Theorem 2.9.1
can be used to find the distance of the code, which is 3. The information rate is
4/7. In Exercise 2.6.12, we encoded some messages using this code. There are
other possibilities for a parity check matrix for a Hamming code of length 7, but
all yield equivalent codes.
Since the parity check matrix H for a Hamming code C contains all r rows of
weight one, the r columns of H are linearly independent. Thus a Hamming code
has dimension 2'- 1- rand contains 2 2r -i-r codewords.
No row of H is the zero word, so no single row of H is linearly dependent.
Thus C has distance at least 2. No two rows of H are equal, so no two rows of H
are linearly dependent. Thus C has distance at least 3. But H contains the rows
100 ... 0, 010 ... 0, and 110 ... 0, which form a linearly dependent set. Therefore,
by Theorem 2.9.1, a Hamming code has distanced= 3.
Now for n = 2' -1 and d = 2t + 1 = 3 (sot= 1),
22r -I

= ____ = 22' -1-r
(3)+ .. ·+G)

(3) +G)

1 +2' -1

'

so Hamming codes are perfect codes. By Theorem 3.2.8, Hamming codes are
perfect single error-correcting codes.
It is trivial to construct an SDA for a Hamming code. All single errors are
corrected so all words of length 2' - 1 and weight one are error patterns that are
corrected, and hence must must be coset leaders. Since if e is an error pattern then
e H sums the rows of the parity check matrix H corresponding to positions where
errors occurred, and since H has 2' - 1 rows, we have the following as an SDA
for a Hamming code:
coset leader
000 ... 0

syndrome
00 ... 0

hr-J

H

Example 3.3.2 For the Hamming code in Example 3.3.1, decode w = 1101001.
The syndrome is w H = 011, which is the fourth row of H. Thus the coset leader
u is the fourth row of h u = 0001000. We decode was w + u = 1100001.

74

3 Perfect and Related Codes

Exercises

3.3.3 Find a generator matrix in standard form for a Hamming code of length 15,
then encode the message 11111100000.
3.3.4 Construct an SDA for a Hamming code of length 7, and use it to decode the
following words:
(c) 0011010
(a) 1101011
(e) 0100011
(d) 0101011
(b) 1111111
(f) 0001011
3.3.5 Construct an SDA for a Hamming code of length 15, and use it to decode
the following words:
(a) 01010 01010 01000
(c) 11100 01110 00111
(e) 00011 10100 00110

(b) 11110 00101 10110
(d) 11100 10110 00000
(f) 11001 11001 11000

3.3.6 Show that each of the following is a parity check matrix for a Hamming
code of length 7, and that the codes are both equivalent to the one in Example
3.3.1.
001
100
010
110
011
111
H'= 100
H"= 011
101
101
110
010
111
001
3.3.7 Prove that all Hamming codes of a given length are equivalent.
3.3.8 Is the following matrix the transpose of a parity check matrix for a Hamming code oflength 15?
10001
[
HT = 11100
01011
10001

10111
10001
00101
01011

01000]
ll110
11101
00111

3.3.9 Show that the Hamming code of length 2' - 1 for r = 2 is a trivial code.
3.3.10 Use the Hamming code of length 7 in Example 3.3.1 and the message
assignment in Exercise 2.6.12. Decode the following message received:
1010111,0110111,1000010,0010101,1001011,0010000,1111100.

75

3.4 Extended codes

3.4

Extended codes

Sometimes increasing the length of a code by one digit, or perhaps a few digits,
result in a new code with improved error detection or error correction capabilities
which are worth the price of a lower information rate. We consider one simple
possibility in this section.
Let C be a linear code of length n. The code C* of length n + 1 obtained from
C by adding one extra digit to each codeword in order to make each word in the
new code have even weight is called the extended code of C.
In Example 1.3.3 we constructed the extended code of K 2 , and the reader did
the same for K 3 in Exercise 1.3.5.
If the original code C has a k x n generator matrix G, then the extended code
C* has k x (n + 1) generator matrix
G* = [G,b],
where the last column b of G* is appended so that each row of G* has even weight.
A parity check matrix for C* can be constructed from G* using Algorithm
2.5.7. But there is an easier way if we are given a parity check matrix H for the
original code C. In this case, the extended code C* has a parity check matrix

H*=[~ {],
where j is then x 1 column of all ones. Note that H* is an (n + 1) x (n + 1- k)
matrix. Since H has rank n- k, the last row of H* ensures that H* has rank
n - k + 1. Moreover,

G*H*=[G,b][~

{] =[GH,Gj+b].

Now G H = 0 and Gj sums the ones in each row of G. From the definition of b, it
follows that Gj +b = 0. Therefore G* H* = 0. By Theorem2.7.6, G* and H* are
indeed generating and parity check matrices respectively for the linear code C*.
Example 3.4.1 Let C be the linear code with generator matrix
10010]
G = [ 01001 .
00111

Then

76

3 Perfect and Related Codes

is a parity check matrix for C by Algorithm 2.5.7. So we obtain the following
generating and parity check matrices for the extended code:

10010 OJ

G* = [ 01001 0

and H* =

00111 1

10
01
11
10
01
00

If v is a word in the original code C and if v* is the corresponding word in the
extended code C* then

*

wt(v ) =

{ wt(v)

wt(v) + 1

if wt( v) is even
if wt( v) is odd.

Therefore if the distance d of C is odd then the distance of C* is d + 1, but if d
is even then the distance of C* is d. So an extended code is of use only when d
is odd, in which case it corrects no more errors than C but will detect one more
error. Notice then that there is no point in extending a code twice.
Example 3.4.2 Assume C has distanced= 5. Then C* has distanced* = 6. By
Theorem 1.11.14, C detects all nonzero error patterns of weight less than or equal
to d - 1 = 4, and C* detects all nonzero error patterns of weight less than or equal
to d* -1 = 5. By Theorem 1.12.9, C corrects all error patterns of weightless than
or equal to L(d -1)/2J = L4/2J = 2, and C* corrects all error patterns of weight
less than or equal to L(d* -1)/2J = L5/2J = 2.

Exercises
3.4.3 Find generating and parity check matrices for an extended Hamming code
of length 8.
3.4.4 Construct an SDA for an extended Hamming code of length 8, and use it to
decode the following words.
(b) 11010110
(c) 11111111
(a) 10101010
3.4.5 Show that an extended Hamming code of length 8 is a self-dual code (that
is, show c = c_L ).
3.4.6 Find a formula for the distance d* of the extended code C* in terms of the
distance ofthe original code C.
3.4.7 Let C be a Hamming code of length 15. Find the number of error patterns
that Theorem 1.11.14 guarantees the extended code C* will detect, and the number of error patterns Theorem 1.12.9 guarantees C* will correct. How may error
patterns does C* correct?

77

3.5 The extended Golay code

3.5

The extended Golay code

In this and the next two sections we construct and decode two codes which will
correct three or fewer errors. The extended Golay code, discussed in this and
the next section, was in fact used in the Voyager spacecraft program which, in
the early 1980's, brought us those marvelous close-up photographs of Jupiter and
Saturn.
Let B be the 12 x 12 matrix

B=

1 0
0 0 0 1 0
1 0
1 0 0 0 1 0
1 0 0 0 1 0 1
0
1 1 0 0 0 1 0 1 1 0
1 0 0 0 1 0
1 0
1 0 0 0 1 0
0
0 0 0 1 0 1 1 0
1 1
0 0 1 0
1 0 1
1 0
0 1 0
0
1 0 0
1 0
1 0
1 0 0 0 1
0
0 1
0 0 0
1
1
1
0

Let G be the 12 x 24 matrix G = [/, B], where I is the 12 x 12 identity matrix.
The linear code C with generator matrix G is called the extended Golay code and
will be denoted by Cz4.
As an aid to remembering B, note that the 11 x 11 matrix B 1 obtained from B
by deleting the last row and column has a cyclic structure. The first row of B1 is
11011100010. The second row is obtained from the first by shifting each digit one
position to the left and moving the first digit to the end. The third row is obtained
from the second row in the same way, and so on for the remaining rows. Thus B
may be remembered as the matrix

where j is the word of all ones oflength 11. By inspection, we see that B T = B;
that is, B is a symmetric matrix.
We now list seven important facts about the extended Golay code Cz4 with
generator matrix G = [/, B]:
(1) C24 has length n = 24, dimension k = 12 and 2 12 = 4096 codewords. This
is clear upon inspection of G.
(2) A parity check matrix for Cz4 is the 24 x 12 matrix

3 Perfect and Related Codes

78

Algorithm 2.5.7 yields this fact.
(3) Another parity check matrix for c24 is the 24 X 12 matrix

To see this, note first that each row of B has odd weight (7 or 11). The
scalar (dot) product of any row with itself is therefore 1. Next, a manual
check shows that the scalar product of the first row of B with any other row
is 0. From the cyclic structure of B1 it follows that the scalar product of any
two different rows of B is 0. Thus BBT =I. But BT = B, so B 2 = BBT
and,

GH =[I, B]

UJ

= I 2 +B 2 =I +BBT =I +I =0.

We shall use both parity check matrices to decode C24·
(4) Another generator matrix for c24 is the 12 X 24 matrix [B, I].
(5) C24 is self-dual; that is, C24 = Ci4..
(6) The distance of Cis 8.
(7) c24 is a three-error-correcting code.
The proofs of facts (4) and (5) are requested in exercises. We will give a proof
of fact (6) which, in the bargain, contains further useful information about the
code C24· The proof is divided into three stages:

Stage I The weight of any word in C24 is a multiple of 4. To see this, note first
that the rows of G have weight 8 or 12. Let v be a word in C24 which is the sum
v = ri + r j of two different rows of G. The rows of B are orthogonal; hence the
rows of G are orthogonal. Therefore ri and r j have an even number, say 2x, of
ones in common. Thus
wt(v) = wt(ri) + wt(rj)- 2(2x)
is a multiple of 4.
Now suppose the word v in C24 is the sum v = ri + rj + rk of three different
rows of G. Let v1 = ri + r j. Since C24 is self-dual, v1 and rk have scalar product
0, and hence an even number, say 2y, of ones in common. Thus
wt(v) = wt(vl) + wt(rk)- 2(2y)
is a multiple of 4. Continuing in this vein (formally, by induction) we see that if v
in C24 is a linear combination ofrows of G, then wt(v) must be a multiple of 4.

Stage II The first eleven rows of G are codewords in C24 of weight 8, so the
distance of c24 must be either 4 or 8.

3.6 Decoding the extended Golay code

79

Stage III We rule out words of weight 4 being codewords in Cz4. Let v be
a nonzero codeword in Cz4, and suppose wt(v) = 4. Then v = u1[/, B] and
v = uz[B, /] for some u1 and uz (since both [/, B] and [B, /] generate Cz4)
and wt(ut) :::; 2 or wt(uz) :S 2 (since one half of v must contain at most two
1's). However no sum of one or two row of B has weight at most 3, so wt(v) =
wt(ui) + wt(ui B) > 4. Therefore v does not have weight 4.
0
Exercises
3.5.1 Show that the word of all ones is in Cz4. Deduce that Cz4 contains no words
of weight 20.
3.5.2 Prove fact (4) about Cz4.
3.5.3 Prove fact (5) about Cz4.
3.5.4 Use Theorem 2.9.1 to verify that C24 has distance 8.

3.6 Decoding the extended Golay code
We shall now find an algorithm for IMLD for the code Cz4. Throughout this
section, w denotes the word received, v the closest codeword to w and u the error
pattern v + w. For Cz4 we want to correct all error patterns of weight at most 3,
so we assume that wt(u) :S 3. A comma will be placed between the first 12 and
the last 12 digits of words in K 24 . The error pattern u will be denoted by [u1, uz],
where u1 and uz each have length 12. Our aim is to determine the coset leader, u
of the coset containing w without having to refer to the SDA of Cz4
Since we are assuming that wt(u) :S 3, either wt(ut):::; 1 or wt(uz) :S 1. Let s1
be the syndrome of w = v + u using the parity check matrix

Then s1 = wH = [ut,uz]H = u1 +uzB. So if wt(uz):::; 1 then s1 consists of
either a word of weight at most 3 (if wt(uz) = 0) or a row of B with at most 2 of
its digits changed (if wt(uz) = 1). Similary, if wt(ut) :::; 1 then the syndrome

sz=w[~] =utB+uz
consists of either a word of weight at most 3 or a row of B with at most 2 of its
digits changed.
In any case, if u has weight at most 3 then it is easily identified, since at most
3 rows of one of the two parity check matrices can be found to add to the corresponding syndrome. Using this observation we obtain the following decoding

80

3 Perfect and Related Codes

algorithm. We shall make use of the fact that B 2 = I and
SJ =u1+uzB=wH

sz=u1B+uz

= (u! +uzB)B =s1B.
To avoid incorporating both of the parity check matrices into the algorithm, only
H = [

~ J is used. Of course once u has been determined, w is decoded to the

codeword v = w + u. ei is the word of length 12 with a 1 in the ith position and
Os elsewhere, and bi is the ith row of B.

Algorithm 3.6.1 (decoding the extended Golay code)
1. Compute the syndrome s = w H.
2. Ifwt(s):S3thenu=[s,O].
3. Ifwt(s +hi):::: 2 for some row bi of B then u = [s +hi, ei].
4. Compute the second syndrome s B.
5. If wt(s B):::: 3 then u = [0, s B].
6. lfwt(sB +hi):::: 2 for some row bi of B then u = [ei, sB +biJ.
7. If u is not yet determined then request retransmission.
The above algorithm requires at most 26 weight calculations in the decoding
procedure. (Of course, once u has been determined then no further steps in the
algorithm need to be done.)

Example 3.6.2 Decode w = 101111101111,010010010010. The syndrome is
s = wH = 101111101111 +001111101110

= 100000000001'
which has weight 2. Since wt(s) :::: 3, we find that
u = [s' 0] = 100000000001' 000000000000

and conclude that
v = w + u = 001111101110,010010010010

was the codeword sent.
Because G = [/, B] is in standard form and any word in K 12 can be encoded
as a message (C24 has dimension 12), the message sent appears in the first 12
digits of the decoded word v. In Example 3.6.2 the message 001111101110 was
sent.

3.6 Decoding the extended Golay code

81

Example 3.6.3 Decode w = 001001001101, 101000101000. The syndrome is
s = wH = 001001001101 + 111000000100= 110001001001,
which has weight 5. Proceeding to step 3 of the Algorithm 3.6.1 we compute
s + bl = 000110001100
s + b2 = 011111000010
s+b3 = 101101011110
s + b4 = 001001100100
s + bs = 000000010010.

Since wt(s + bs) ~ 2, we find that
u = [s + bs, es] = 000000010010,000010000000

and conclude that
v = w + u = 001001011111,101010101000

was the codeword sent.

Example 3.6.4 Decode w = 000111000111,011011010000. The syndrome is
s=wH=u1+u2B
= 000111000111 + 101010101101
= 101101101010,
which has weight 7. Proceeding to step 3, we find wt(s + b;) 2: 3 for each row b;
of B. We continue to step 4; the second syndrome is

sB = 111001111101,
which has weight 9. Forging ahead to step 5 we compute

sB+b1 =001110111000
sB +b2 = 010111110110
sB+b3 = 100101101010
sB +b4 = 000001010000.
Since wt(s B + b4) ~ 2, we find that
u = [e4, s B

+ b4] = 000100000000,000001010000

and conclude that

v = w+u = 000011000111,011010000000
was the codeword sent.

82

3 Perfect and Related Codes

Exercises
3.6.5 The code is C24. Find the most likely error pattern if possible, for each of
the following received words w.
(a) 111000000 000,011011011011
(b) 111111000000,100011100111
(c) 111 111 000 000, 101 011 100 111
(d) 111 111 000 000, 111 000 111 000
(e) 111 000 000 000, 110 111 001 101
(f) 110 111 001 101, 111 000 000 000
(g) 000 111 000 111, 101 000 101 101
(h) 110 000 000 000' 10 1 100 100 000
(i) 110 101 011 101, 111 000 000 000.

3.6.6 Find the most likely error pattern for any word with the given syndromes.
(a) Sj = 010010000000, S2 = 011111010000
(b) S[ = 010010100101, S2 = 001000110000
(c) Sj = 111111000101, S2 = 111100010111
(d) S[ = 111111111011, S2 = 010010001110
(e) S[ = 001101110110, S2 = 111110101101
(f) Sj = 010111111001, S2 = 100010111111
3.6.7 Show that if s or s B has weight 4 then IMLD requires that the word be
retransmitted.

3.7

The Golay code

Another interesting three-error-correcting code can be obtained by puncturing
C24, that is," by removing a digit from every word in C24. The same digit must
be removed from each word. We shall remove the last digit.
Let B be the 12 x 11 matrix obtained from the matrix B by deleting the last
column. Let G be the 12 x 23 matrix G = [/12, zh The linear code with generator
matrix G is called the Golay code and is denoted by C23· The Golay code has
length n = 23, dimension k = 12, and contains 2 12 = 4096 codewords. Note that
the extended code C23 is indeed C24· C23 has distance 7. This is most easily seen
from the fact that C23 = C24 (see Exercise 3.4.6), but can be shown using Theorem
3.2.8 or by modifying the proof that c24 has distance 8.
The Golay code C23 is a perfect code (Example 3.2.4) and will correct all
error patterns of weight 3 or less, and no others (Theorem 3.2.8). Therefore every
received word w is at most distance 3 from exactly one codeword. So if we append
the digit 0 or 1 to w forming wO or w 1 respectively so that the resulting word has

3. 7 The Golay code

83

odd weight, then the resulting word is distance at most 3 from a codeword c in
C24 (see Exercise 3.7.10). Decoding to c using Algorithm 3.6.1 and removing the
last digit from c then gives the closest codeword to w in c23.
Algorithm 3.7.1 (decoding the Golay code)
1. Form wO or w 1, whichever has odd weight.
2. Decode wi (i is 0 or 1) using Algorithm 3.6.1 to a codeword c in C24.
3. Remove the last digit from c.
In practice, the received word w is normally a codeword, however wi formed in
step 1 is never a codeword (Why?). If w is a codeword then the syndrome of wi
is the last row of H (Why?) so this can easily be checked before implementing
Algorithm 3.6.1.
Example 3.7.2 Decode w = 001001001001, 11111110000. Since w has odd
weight, form wO = 001001001001, 111111100000. Then SJ = 100010111110.
Since SJ = b6 + e9 + e12, wO is decoded to 001001000000, 111110100000 and so
w is decoded to 001001000000, 11111010000.
Exercises
3.7.3 Decode each of the following received words that were encoded using C23.
(a) 101011100000,10101011011
(b) 101010000001,11011100010
(c) 100101011000,11100010000
(d) 011001001001,01101101111.
3.7.4 Prove that C23 has distanced= 7.
3.7.5 Find the reliability of C23 transmitted over a BSC of probability p.
3.7.6 Determine whether C23 or C24 has the greater reliability. Use the same BSC
for both.
3.7.7 Use the fact that every word of weight 4 is distance 3 from exactly one
codeword (why?) to count the number of codewords of weight 7 in the Golay
code. (Hint: for any codeword c, the number of words that have weight 4 and are
distance 3 from cis G).)
3.7.8 Use Exercise 3.7.7 to show that C24 contains precisely 759 codewords of
weight 8. (Hint: each word of weight 5 is at distance at most 3 from exactly one
codeword.)
3.7.9 Use Exercises 3.5.1 and 3.7.8 to verify the following weight distribution
table for c24:
weight
number of words

3 Perfect and Related Codes

84

3.7.10 Let w be a received word that was encoded using C23. Append a digiti
to w to form a word wi of odd weight. Show that wi is within distance 3 of a
codeword in C24· (Hint: all words in C24 have even weight.)

3.8

Reed-Muller codes

In this section we briefly consider another important class of codes which includes
the extended Hamming code discussed earlier (see also Chapter 9). The rth order
Reed-Muller code of length 2m will be denoted by RM(r, m), 0 ::=: r ::=: m. We
present a recursive definition of these codes:
(1) RM(O,m) = {00 ... 0, 11 ... 1}, RM(m,m) = K 2rn
(2) RM(r,m) = {(x,x + y) I x E RM(r,m -1), y E RM(r -1,m -1)}, 0 < r <
m.
So RM(m, m) is all words oflength 2m and RM(O, m) is just the all ones word
(and the zero word).
Example 3.8.1
RM(O,O) = {0, 1}
RM(O, 1) = {00, 11},
RM(1, 1) = K 2 = {00,01, 10, 11}
RM(O, 2) = {0000, 1111}, RM(2, 2) = K 4
RM(l, 2) = {(x, x + y) I x E {00, 01, 10, 11}, y E {00, 11}}

Rather than use this description of the code, we will give a recursive construction for the generator matrix of RM(r, m), which we will denote by G(r, m). For
0 < r < m, define G(r,m) by
G(r, m) = [G(r, m- 1)
0

G(r, m- 1)
G (r - 1, m - 1)

J

For r = 0 define
G(O, m) = [11. .. 1]
and for r = m, define
G(

m,m

) =[G(m-1,m)J
0 ... 01

Example3.8.2 ThegeneratormatricesforRM(O, 1) andRM(l, 1) are
G(O, 1) = [11] and G(l, 1) =

[~ ~]

Example 3.8.3 Let m = 2, then the length is 4 = 22 and for r = 1, 2 we have
G( 1 2) = [G(l, 1)
'
0

G(l, 1)]
G(O, 1) '

G(2 2) = [G(1, 2)]
,
0001 .

85

3.8 Reed-Muller codes

Using Example 3.8.2 we have,
11

G(l,2)= [ 01
00

1111]
11] ' G(2 , 2) = [0101
0011

01
11

0001

Example 3.8.4 Form= 3, n = 2 3 = 8, we have
G(2,3) ]
G(3,3)= [ 0000000 1

G(0,3)=(11111111),
G(1,3) = [G(~,2)

G(2, 3) = [ G(~, 2 )

G(l,2)]
G(O, 2) '

G(2, 2)]
G(l,2) .

Thus using Example 3.8.3

1111
0101
[
G(l' 3) = 0011
0000

Ill 1]

0101
0011
1111

.

Exercises
3.8.5 Find the generator matrix G(2, 3).
3.8.6 Find generator matrix G(r, 4), for the codes RM(r, 4) for r = 0, I, 2.
With this recursive definition it is a simple matter to prove via induction the
basic properties of a Reed-Muller code.
Theorem 3.8.7 The rth order Reed-Muller code RM(r, m) defined above has the
following properties:
(1) length n =2m

(2) distanced = 2m-r
(3) dimension k = z.=;·=O ('7)
(4) RM(r- 1, m) is contained in RM(r, m), r > 0

(5) dual code RM(m- 1- r, m), r < m.
Proof: The proofs of these claims all use induction. We leave it as an exercise to
show that this theorem holds for all RM (r, m) codes for m = 1, 2, 3, 4. Also, we
note that these claims are obviously true for r = 0 and r = m.
First we want to show that RM (r m) ~ RM (r, m). We start with

1,

G(l,m) = [G(l,~ -1)

G(l,m-1)]
G(O,m -1) .

86

3 Perfect and Related Codes

Since 1 is the top row of G(1, m - 1) then the all ones vector (1, 1) is the top
row vector in (G(1,m -1), G(l,m -1)). ThusRM(O,m) = {0, 1} is contained in
RM(1,m).
In general since G(r -1,m -1) is a submatrix of G(r,m -1) and G(r2, m- 1) is a submatrix of G(r- 1, m- 1) we have obviously
G(r- 1,m)= [G(r-1,m-1)
0

G(r-1,m-1)]
G(r -2,m -1)

is a submatrix of G(r, m) and thus RM(r -1, m) is a subcode of RM(r,m).
Next we establish the distanced= 2m-r for RM(r, m), using induction on r.
Since RM(r,m) = {(x,x + y) I x E RM(r, m -1), y E RM(r -1, m -1)} and
RM(r -1, m -1) <;; RM(r, m -1) then x + y E RM(r, m -1) and so if xi= y, then,
by our inductive hypothesis, wt(x + y) 2: 2m-I-r. Also wt(x) 2: 2m-I-r. Hence
wt(x,x + y) = wt(x + y) +wt(x) 2: 2·2m-l-r = 2m-r. If x = y, then (x,x + y) =
(y, 0) but y E RM(r -1, m -1) and thus wt(y, 0) = wt(y) 2: 2m-l-(r-l) = 2m-r.
From the definition of G(r, m), we have
dimRM(r,m) = dimRM(r,m -1) +dimRM(r -1,m -1)

Since

(7) = (m~l) + (7-=D and (m 01) = 1 =(~)we have,
dimRM(r,m) =

t (7).
t=O

Finally let
RM(r,m) = {(x,x + y) I x E RM(r,m -1),y E RM(r -1,m -1)}
and let
RM(m- r -1,m) = {(x',x' + y') I x' E RM(m- r -1,m -1),
y' E RM(m -r-2,m -1)}.

By induction the dual of RM(r, m- 1) is RM(m- r- 2, m - 1) and the dual of
RM(r -1, m- 1) is RM(m- r -1, m- 1) thus x · y' = 0, and x' · y = 0. Also

3.8 Reed-Muller codes

87

sinceRM(r -l,m -1) ~ RM(r,m -1), y · y' = 0. Hence

(x, x + y) · (x', x' + y') = (x + y) · (x' + y') + x · x'

= 2(x · x') + x · y' + y · x' + y · y'
=0.
We see that every vector in RM (r, m) is orthogonal to every vector in RM (m - r l,m). Since

dimRM(r,m)+dimRM(m-r-l,m) =

t (7) mfl (7)
+

z=O

z=O

= t(m':_)+
z=O

=

mfl (~)
J=O

t(~) =2m
j=O

]

the RM(m- r -1, m) code is the dual of the RM(r,m) code.

0

Exercises
3.8.8 Show that Theorem 3.8.7 holds for the codes RM(r, m), 1 :::: m :::: 4, constructed in Examples 3.8.1, 3.8.3, 3.8.4 and Exercises 3.8.5, 3.8.6.
We consider the first order Reed-Muller code RM(l, m). Notice that RM(m2, m) has dimension 2m- m- 1 and has distance 4, length 2m and therefore is
an extended Hamming code. By Theorem 3.8.7, RM(l,m) is the dual of this
extended Hamming code. We present a decoding algorithm for this code which
is quite efficient. We postpone a discussion of a decoding algorithm for general
RM(r, m) codes until Chapter 9.
Note that the RM ( 1, m) code is a small code with a large minimum distance, so
a good decoding algorithm is in fact the most elementary: for each received word
w, find the codeword in RM ( 1, m) closest to w. This can be done very efficiently.
Example 3.8.9 Let m = 3, consider the RM(l, 3) code which has length 8 = 23 ,
and 16 = 23+ 1 codewords. The minimum distance is 4. Let
1111
0101
[
3
G(l' ) = 0011
0000

1111]
0101
0011
1111

Note that if w is received andd(w, c)< 2 then we decode w to c but if d(w, c)> 6,
then d ( w, 1 +c) < 2 and we decode w to 1 + c. (Reca111 is a codeword.) For example, if w = 1000 1111 is received then c = 0000 1111 is the nearest codeword.

3 Perfect and Related Codes

88

If w = (10101011) is received and we find c = (01010101) with d(w, c)> 6, then
c + 1 = 10101010 is the nearest codeword. Thus we have to examine at most half
of the codewords in RM ( 1, m).

In fact, there are very efficient matrix methods to compute these distances
which we will present in the next section.

Exercises
3.8.10 Let G(l, 3) be the generator for the RM(l, 3) code, decode the following
received words
(b) 0110 0111
(d) 1100 1110

(a) 0101 1110
(c) 0001 0100

3.8.11 Let G ( 1, 4) be the generator for the RM ( 1, 4) code, decode the following
received words
(a) 1011 0110
(b) 1111 0000

3.9

0110 1001
01011111

Fast decoding for RM(l, m)

In this section we present briefly and without justification a very efficient decoding
method for RM ( 1, m) codes. It utilizes the Fast Hadamard Transform to find the
nearest codeword. First we need to introduce the Kronecker product of matrices.
Define A x B = [aiJ B]; that is, entry aij in A is replaced by the matrix a;1 B.

Example 3.9.1 Let H = [
1

1

[1

-1

0

0

hxH= 0

0

~

_ ~ h = [~

J

~ ~]
-~

and

~J. Then
H

xI,~ ~00:

Now we consider a series of matrices defined as:

fori = 1, 2, ... , m, where H is as in Example 3.9.1.

Example 3.9.2 Let m = 2. Then

HI = h X H X It = h X H
Hi = h X H X h = H X h
(see Example 3.9.1).

l

:
1

The recursive nature of the construction of RM ( 1, m) codes suggests that there
is a recursive approach to decoding as well. This is the intuitive basis for the
following decoding algorithm for RM ( 1, m).

Algorithm3.9.4 (decoding the RM(l,m) code) Suppose w is received and
G (1, m) is the generator matrix for RM (1, m) code.
1. Replace 0 by -1 in w forming w.
2. Compute WI = wH~ and Wi = Wi-! H~ fori = 2, 3, ... , m.
3. Find the position j of the largest component (in absolute value) of Wm.
Let v(j) E Km be the binary representation of j (low order digits first). Then if
the jth component of Wm is positive, the presumed message is (1, v(j)), and if it
is negative the presumed message is (0, v(j)).

90

3 Perfect and Related Codes

Example 3.9.5 Let m = 3, and G(l, 3) be the generator matrix for RM(l, 3) (see
Example 3.8.9). If w = 10101011 is received, convert w tow= (1, -1, 1, -1, 1,
-1, 1, 1). Compute:

WI= wHj = (0,2,0,2,0,2,2,0)
W2 = WIH} = (0,4, 0, 0,2,2, -2, 2)
W3 = w2Hl = (2, 6, -2, 2, -2, 2, 2, -2)
(see Example 3.9.2 for Hj, Hff, Hj). The largest component of w3 is 6 occurring
in position 1. Since v(1) = 100 and 6 > 0, then the presumed message is m =
(1100).
Suppose w = (1 000 1111). Then w = (1, - 1, - 1, -1 , 1, 1, 1, 1) and
WI = wHj = (0, 2, -2, 0, 2, 0, 2, 0)
W2 = WJH32 = (-2,2,2,2,4,0,0,0)
W3 = w2H} = (2, 2, 2, 2, -6, 2, 2, 2).
The largest component of w3 is -6 occurring in position 4. Since v(4) = 001 and
-6 < 0 the presumed message is (0001).
Exercises
3.9.6 Decode the received words in Exercise 3.8.10 using Algorithm 3.9.4 (and
Example 3.9.2).
3.9.7 Compute H~ fori= 1, 2, 3, 4.
3.9.8 Decode the received words in Exercise 3.8.11 using Algorithm 3.9.4 (and
Exercise 3.9.6).

