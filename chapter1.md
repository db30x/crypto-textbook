Chapter 1

Introduction to Coding Theory
1.1

Introduction

Coding theory is the study of methods for efficient and accurate transfer of information from one place to another. The theory has been developed for such diverse
applications as the minimization of noise from compact disc recordings, the transmission of financial information across telephone lines, data transfer from one
computer to another or from memory to the central processor, and information
transmission from a distant source such as a weather or communications satellite
or the Voyager spacecraft which sent pictures of Jupiter and Saturn to Earth.
The physical medium through which the information is transmitted is called
a channel. Telephone lines and the atmosphere are examples of channels. Undesirable disturbances, called noise, may cause the information received to differ
from what was transmitted. Noise may be caused by sunspots, lightning, folds in
a magnetic tape, meteor showers, competing telephone messages, random radio
disturbance, poor typing, poor hearing, poor speech, or many other things.
Coding theory deals with the problem of detecting and correcting transmission
errors caused by noise on the channel. The following diagram provides a rough
idea of a general information transmission system.
Communication
Channel

The most important part of the diagram, as far as we are concerned, is the noise,
for without it there would be no need for the theory.
In practice, the control we have over this noise is the choice of a good channel .
to use for transmission and the use of various noise filters to combat certain types
of interference which may be encountered. These are engineering problems. Once
we have settled on the best mechanical system for solving these problems, we can

1

1 Introduction to Coding Theory

2

focus our attention on the construction of the encoder and the decoder. Our desire
is to construct these in such a way as to effect:
1. fast encoding of information,
2. easy transmission of encoded messages,
3. fast decoding of received messages,
4. correction of errors introduced in the channel, and
5. maximum transfer of information per unit time.
The primary goal is the fourth of these. The problem is that it is not generally
compatible with the fifth, and also may not be especially compatible with the
other three. So any solution is necessarily a trade-off among the five objectives.
In our everyday communications among one another we standardly use words,
spoken or written, made from a limited alphabet. We have information to communicate; we encode it into strings of words which we then speak or write. These
are then sent across a channel, the channel normally being the space from mouth
to ear or from pen to paper to eye. The noise might be caused by poor speech,
bad hearing, incorrect grammar, a loud stereo, competing speech, misspelling,
misreading, or a faulty typewriter. The decoder is our reading (or hearing) and
understanding of the received messages.
We have built-in error-correcting devices that that we don't even think about.
Suppose we receive the message "Apt natural. I have a gub." which is a hold-up
note in Woody Allen's "Take the Money and Run." Since our language does not
use all possible words of any given length, we hopefully recognize that "gub" is
not a word. We may safely assume that the transmitted word was close to "gub"
in some sense. So it was more likely to have been "gut" or "gun" or "tub" than
say "firetruck" or "rat." It is only the context of the message though that lets us
choose "gun" as the most likely word. "Apt" is a perfectly good word, but again
from the context we are led to correct it to "act." And if we happen to be literate,
we will also correct "natural" to "naturally," even though this was probably an
error attributed to the source and not to the noise on the channel.
Of these types of errors, we can probably only deal with the first: that is
choosing the most likely word transmitted. The standard method for combating
errors is through redundancy. Many businesses these days commonly add check
digits to identification numbers; these are extra digits that are used to check the
correctness of data or of account numbers. This is probably the most commonly
recognized method of coding in real life. We shall deal with more sophisticated
but similar ideas.

1.2 Basic assumptions
We state some fundamental definitions and assumptions which will apply throughout the text.

1.2 Basic assumptions

3

In many cases, the information to be sent is transmitted by a sequence of zeros
and ones. We call a 0 or a 1 a digit. A word is a sequence of digits. The length
of a word is the number of digits in the word. Thus 0110101 is a word of length
seven. A word is transmitted by sending its digits, one after the other, across a
binary channel. The term "binary" refers to the fact that only two digits, 0 and
1, are used. Each digit is transmitted mechanically, electrically, magnetically, or
otherwise by one of two types of easily differentiated pulses.
A binary code is a set C of words. The code consisting of all words of length
two is
c = {00, 10,01, 11}.
A block code is a code having all its words of the same length; this number is
called the length of a code. We will consider only block codes. So, for us, the
term code will always mean a binary block code. The words that belong to a given
code Co, will be called codewords. We shall denote the number of codewords in
a code C by ICI.
Exercises
1.2.1 List all words of length 3; of length 4; of length 5.

1.2.2 Find a formula for the total number of words of length n.
1.2.3 Let C be the code consisting of all words of length 6 having an even number
of ones. List the codewords in C.
We also need to make certain basic assumptions about the channel. These
assumptions will necessarily shape the theory that we formulate.
The first assumption is that a codeword oflength n consisting of O's and 1'sis
received as a word of length n consisting of O's and 1's, although not necessarily
the same as the word that was sent.
The second is that there is no difficulty identifying the beginning of the first
word transmitted. Thus, if we are using codewords of length 3 and receive
011011001, we know that the words received are, in order, 011, 011, 001. This
assumption means, again using length 3, that the channel cannot deliver 01101 to
the receiver, because a digit has been lost here.
The final assumption is that the noise is scattered randomly as opposed to
being in clumps called bursts. That is, the probability of any one digit being
affected in transmission is the same as that of any other digit and is not influenced
by errors made in neighboring digits. This is not a very realistic assumption for
many types of noise such as lightning or scratches on compact discs. We shall
eventually consider this type of noise.
In a peifect, or noiseless, channel, the digit sent, 0 or 1, is always the digit
received. If all channels were perfect, there would be no need for-coding theory.
But fortunately (or unfortunately, perhaps) no channel is perfect; every channel is
noisy. Some channels are less noisy, or more reliable, than others.

1 Introduction to Coding Theory

4

A binary channel is symmetric if 0 and 1 are transmitted with equal accuracy;
that is the probability of receiving the correct digit is independent of which digit,
0 or 1, is being transmitted. The reliability of a binary symmetric channel (BSC)
is a real number p, 0 =::: p =::: 1, where pis the probability that the digit sent is the
digit received.
If p is the probability that the digit received is the same as the digit sent, then
1 - p is the probability that the digit received is not the digit sent. The following
diagram may clarify how a BSC operates:

ogP

o

p

1

p

1

In most cases it may be hard to estimate the actual value of p for a given channel.
However the actual value of p does not influence significantly the form of the
theory.
We will call one channel more reliable than another if its reliability is higher.
Note that if p = 1, then there is no chance of a digit being altered in transmission.
Hence the channel is perfect and of no interest to us. Nor is a channel with p = 0
of any interest. Any channel with 0 < p =::: 1/2 can be easily converted into a
channel with 1/2 =::: p < 1. Henceforth we will always assume that we are using a
BSC with probability p satisfying 1/2 < p < 1. (The case where p = 1/2 is raised
in Exercise 1.2.6.)
Exercises
1.2.4 Explain why a channel with p = 0 is uninteresting.
1.2.5 Explain how to convert a channel with 0 < p ::::: 1/2 into a channel with
1/2::::: p < 1.

1.2.6 What can be said about a channel with p = 1/2?

1.3

Correcting and detecting error patterns

We consider now the possibilities of correcting and detecting errors. In this section we intend to develop an intuitive understanding of the concepts involved in
correcting and detecting errors, while a formal approach is adopted in later sections.
Suppose a word is received that is not a codeword. Clearly some error has occurred during the transmission process, so we have detected that an error (perhaps
several errors) has occurred. If however a codeword is received, then perhaps no
errors occurred during transmission, so we cannot detect any error.

1.3 Correcting and detecting error patterns

5

The concept of correcting an error is more involved. As in the introduction
when we were inclined to correct 'gub' to 'gun' rather than to 'rat', we appeal to
intuition to suggest that any received word should be corrected to a codeword that
requires as few changes as possible. (In a later section we show that the probability
that such a codeword was sent is at least as great as the probability that any other
codeword was sent.) To consolidate these ideas, we shall discuss some particular
codes. Notice that our assumption that no digits are lost or created in transmission
precludes decoding 'gub' to 'firetruck'.
Example 1.3.1 Let C1 = {00, 01, 10, 11}. Every received word is a codeword and
so C 1 cannot detect any errors. Also C 1 corrects no errors since every received
word requires no changes to become a codeword.
Example 1.3.2 Modify C1 by repeating each codeword three times. The new
code is
c2 = {oooooo,01010I, 101010,1111111.

This is an example of a repetition code. Suppose that 110101 is received. Since
this is not a codeword we can detect that at least one error has occurred. The
codeword 010101 can be formed by changing one digit, but all other codewords
are formed by changing more than one digit. Therefore we expect that 010101
was the most likely codeword transmitted, so we correct 110101 to 010101. (A
codeword that can be formed from a word w with the least number of digits being
changed is called a closest codeword; this idea is formalized later.) In fact if any
of the codewords, c E C2, is transmitted and one error occurs during transmission,
then the unique closest codeword to the received word is c; so any single error
results in a word that we correct to the codeword that was transmitted.
Example 1.3.3 Modify C 1 by adding a third digit to each codeword so that the
number of 1's in each codeword is even. The resulting code is
C3 = {ooo,oii, IOI, 110}.

The added digit is called a parity-check digit. Suppose 010 is received, then since
010 is not a codeword we can detect that an error has occurred. Each of the
codewords 110, 000 and 011 can be formed by changing one digit in the received
word. In later sections we distinguish between how we treat received words that
are closest to a unique codeword (and so is the single most likely codeword sent)
as was the case in Example 1.3.2, and received words that are closest to several
codewords as in this example. It suffices at this stage to observe that it seems more
sensible to correct 010 to one of 110, 000 or 0 II rather than to 101.

Exercises
1.3.4 Let C be the code of all words of length 3. Determine which codeword was
most likely sent if 001 is received.

6

1 Introduction to Coding Theory

1.3.5 Add a parity check digit to the codewords in the code in Exercise 1.3 .4, and
use the resulting code C to answer the following questions.
(a) If 1101 is received can we detect an error?
(b) If 1101 is received what codewords were most likely to have been transmitted?
(c) Is any word of length 4 that is not in the code, closest to a unique codeword?
1.3.6 Repeat each codeword in the code C defined in Exercise 1.3.4 three times
to form a repetition code of length 9. Find the closest codewords to the following
received words:
(a) 001000001

(b) 011001011
(d) 100000010

(c) 101000101

1.3.7 Find the maximum number of codewords of length n = 4 in a code in which
any single error can be detected.
1.3.8 Repeat Exercise 1.3.7 for n = 5, n = 6, and for general n.

1.4 Information rate
After the last section it is apparent that the addition of digits to codewords may
improve the error correction and detection capabilities of the code. However,
clearly the longer the codewords, the longer it takes to transmit each message. The
information rate (or just rate) of a code is a number that is designed to measure
the proportion of each codeword that is carrying the message. The information
rate of a code C of length n is defined to be (for binary codes)
1

-log2 1CI.
n
Since we may assume that 1 S IC I S 2n, it is clear that the information rate ranges
between 0 and 1; it is 1 if every word is a codeword and 0 if IC I = 1.
For example, the information rates of the codes C1, C2 and C3 in the previous
section are 1, 1/3 and 2/3 respectively. Each of these information rates seems
sensibly related to their respective codes, since the first 2 digits of the 6 in each
codeword in C2 can be considered to carry the message, as can the first 2 digits of
the 3 in each codeword in C3.

Exercises
1.4.1 Find the information rate for each of the codes in Exercises 1.3.4, 1.3.5 and
1.3.6.

1.5 The effects of error correction and detection

1.5

7

The effects of error correction and detection

To exemplify the dramatic effect that the addition of a parity-check digit to a code
can have in recognizing when error occur, we consider the following codes.
Suppose that all 2 11 words of length 11 are codewords; then no error is detected. Let the reliability of the channel be p = 1 - 10- 8 and suppose that digits
are transmitted at the rate of 107 digits per second. Then the probability that a
word is transmitted incorrectly is approximately 11p 10 (1 - p), which is about
11/108 . So about
11 107
108 · U = .1 words per second
are transmitted incorrectly without being detected. That is one wrong word every
10 seconds, 6 a minute, 360 an hour, or 8640 a day! Not too good.
Now suppose that a parity-check digit is added to each codeword, so the number of 1's in each of the 2048 codewords is even. Then any single error is always detected, so at least 2 errors must occur if a word is to be transmitted incorrectly without our knowledge. The probability of at least 2 errors occurring is
1- p 12 -12p 11 (1- p) which can be approximated by Ci}p 10 (1- p) 2 which for
p = 1- 10-8 is about 1~16. Now approximately 1~16 . \~ = 5.5 X 10-9 words per
second are transmitted incorrectly without being detected. That is about one error
every 2000 days!
So if we are willing to reduce the information rate by lengthening the code
from 11 to 12 we are very likely to know when errors occur. To decide where
these errors have actually occurred, we may need to request the retransmission of
the message. Physically this means that either transmission must be held up until
confirmation is received or messages must be stored temporarily until retransmission is requested; both alternatives may be very costly in time or in storage space.
It may also be that retransmission is impractical, such as with the Voyager mission
and when using compact discs. Therefore, at the expense of further increase in
wordlength, it may well be worth incorporating error-correction capabilities into
the code. Introducing such capabilities may also make encoding and decoding
more difficult, but will help to avoid the hidden costs in time or space mentioned
above.
One simple scheme to introduce error-correction is to form a repetition code
where each codeword is transmitted three times in succession. Then if at most one
error is made per 33 digit codeword, at least two of the three transmissions will be
correct. Since the comparisons of the three 11 digit words is relatively simple, the
only real trade-off for being able to correct one error is an information rate of 1/3
instead of 1.
Still 113 is only 113. Perhaps we could do better. We will see later that it
is possible to add only 4 extra digits to each 11 digit codeword and still be able
to correct any single error. This produces a code with information rate 11/15, a

1 Introduction to Coding Theory

8

valuable improvement provided that the extra encoding and decoding costs are not
prohibitive.
It is our task, then, to design codes with reasonable information rates, low
encoding and decoding costs and some error-correcting or error-detecting capabilities that make the need for retransmission unlikely.

1.6 Finding the most likely codeword transmitted
Suppose that we have an overall view of the transmission process, knowing both
the codeword v that is transmitted and the word w that is received. For any given
v and w, let ¢p(v, w) be the probability that if the codeword vis sent over a BSC
with reliability p then the word w is received. Since we are assuming that noise is
distributed randomly, we can treat the transmission of each digit as an independent
event. So if v and w disagree in d positions, then we have n - d digits correctly
transmitted and d incorrectly transmitted and thus,

Example 1.6.1 Let C be a code oflength 5. Then for any v inC, the probability
that v is received correctly is

Let 10101 be in C. Then

and if p = .9 then
¢.9(10101,01101) = (.9)\.1) 2 = .00729.

Exercises
1.6.2 Calculate ¢.97(v, w) for each of the following pairs of v and w:
(a) v=01101101,w=10001110
(b) v=1110101,w=1110101
(c) v = 00101, w = 11010
(d) v = 00000, w = 00000
(e) v = 1011010, w = 0000010
(f) v = 10110, w = 01001
(g) v=111101,w=000010.
In practice we know w, the word received but we do not know the actual
codeword v that was sent. However each codeword v determines an assignment
of probabilities ¢p(v, w) to words, w. Each such assignment is a mathematical
model and we choose the model (that is, the codeword v) which agrees most with

1.6 Finding the most likely codeword transmitted

9

observation-in this case, which makes the word received most likely. That is,
assume v is sent when w is received if
c/Jp(v, w) = max{c/Jp(u, w) : u E C}.

The following theorem provides a criterion for finding such a codeword v.
Theorem 1.6.3 Suppose we have a BSC with 1/2 < p < 1. Let VJ and v2 be
codewords and w a word, each of length n. Suppose that VJ and w disagree in d1

positions and v2 and w disagree in d2 positions. Then
c/Jp(VJ, w) S c/Jp(v2, w) if and only if d1 ?:. d2.
Proof: We have already established that c/Jp(VJ, w) s_ c/Jp(v 2 , w) iff pn-d1 (1p)di s_ pn-d2(1- p)d2 iff ( l~p)d2-d1 s_ 1 iff d2 S d1 (since l~p > 1).
D

This formally establishes the procedure for correcting words which until now
we had adopted as being an intuitively sensible procedure: correct w to a codeword which disagrees with w in as few positions as possible, since such a codeword is the most likely to have been sent, given that w was received.
Example 1.6.4 If w = 00110 is received over a BSC with p = .98, which of the
codewords 01101, 01001, 10100, 10101 was the most likely one sent?
v
01101
01001
10100
10101

d (number of disagreements with w)

3

4
2
3

+-- smallest d

Using the above table, Theorem 1.6.3 says that 10100 was the most likely codeword sent. Note that we don't need to know the precise value of p in order to
apply Theorem 1.6.3; we only need to know that p > 1/2.

Exercises
1.6.5 Suppose that w = 0010110 is received over a BSC with reliability p = .90.
Which of the following codewords is most likely to have been sent?
1001011,1111100,0001110,0011001,1101001.
1.6.6 Which of the 8 codewords in the code of Exercise 1.3.6 is most likely to
have been sent if w = 101000101 is received?
1.6.7 If C = {01000,01001,00011, 11001} and a word w = 10110 is received,
which codeword is most likely to have been sent?
1.6.8 Repeat Exercise 1.6.7 after replacing C with {010101, 110110,101101,
100110, 011001} and w with 101010.

1 Introduction to Coding Theory

10

1.6.9 Which of the codewords 110110, 110101, 000111, 100111, 101000 is most
likely to have been sent if w = 011001 is received.
1.6.10 In Theorem 1.6.3 we assume that 1/2 < p < 1. What would change in the
statement of Theorem 1.6.3 if we replace this assumption with
(a) 0 < p < 1/2,

1.7

(b) p = 1/2?

Some basic algebra

A problem that we shall need to address is that of finding an efficient way of finding the closest codeword to any received word. If the code has many codewords
then it is impractical to compare each received word w to each codeword in turn
to find which codeword disagrees with w in as few positions as possible. For example, if the code contains 2 12 codewords (as was used for the Voyager mission)
then such a decoding procedure could never hope to keep up with the incoming
transmission. To overcome this problem, we need to introduce some structure into
our codes.
Let K = {0, 1} and let Kn be the set of all binary words of length n. Define
addition and multiplication of the elements of K as follows:
0 + 0 = 0, 0 + 1 = 1' 1 + 0 = 1' 1 + 1 = 0
0. 0 = 0, 1 . 0 = 0, 0. 1 = 0, 1 . 1 = 1.
Define addition for the elements of Kn componentwise, using the addition defined
on K to add each component. For example, let

v = 01101 and w = 11001 then v + w = 10100.
Clearly the addition of two binary words of length n results in a binary word of
length n, so Kn is closed under addition.
Using linear algebra terminology, we refer to an element of K as scalar. Then
scalar multiplication of Kn is defined componentwise. Since the only scalars are
0 and 1, the only scalar multiples of a word ware 0 · w, which is the element of
Kn with 0 in every component, and 1 · w, which is w. We refer to the element of
Kn with 0 in all components as the zero word. Clearly Kn is closed under scalar
multiplication.
With these definitions of addition and scalar multiplication, it can be shown
that Kn is a vector space. That is, for any words of length n, u, v and w and for
any scalars a and b:
1. v+w E Kn

2. (u+v)+w=u+(v+w)
3. v + 0 = 0 + v = v, where 0 is the zero word

1.8 Weight and distance

11

4. for some v' E Kn, v + v' = v' + v = 0

5. v+w = w+v
6. avE Kn
7. a(v+w)=av+aw
8. (a+b)v=av+bv

9. (ab)v = a(bv)
10. 1v = v.

Exercises
1.7.1 Show that if vis a word in Kn then v + v = 0.
1.7.2 Show that if v and ware words in Kn and v + w = 0 then v = w.
1.7.3 Show that if u, v and ware words in Kn and u + v = w then u + w = v.
Notice that if v is sent over a BSC and w is received then 0 occurs in a component of v + w if the corresponding component of v was correctly transmitted and
a 1 occurs if the component was incorrectly transmitted. v + w is called the error
pattern, or the error. For example if v = 10101 is transmitted and w = 01100
is received then errors occurred in the 1st, 2nd and 5th components. The error
pattern is v + w = 11001.

1.8 Weight and distance
We introduce two important terms. Let v be a word of length n. The Hamming
weight, or simply the weight, of vis the number of times the digit 1 occurs in v. We
denote the weight of v by wt(v). For example, wt(110101) = 4 and wt(OOOOO) =
0.
Let v and w be words of length n. The Hamming distance, or simply distance,
between v and w is the number of positions in which v and w disagree. We denote
the distance between v and w by d(v, w). For example, d(OIOll, 00111) = 2 and
d(10110, 10110) = 0.
Note that the distance between v and w is the same as the weight of the error
pattern u = v + w:
d(v, w) = wt(v + w).
For example, if v = 11010 and w = 01101, we have d(v, w) = d(11010, 01101) =
4, and wt(v + w) = wt(11010 + 01101) = wt(IOlll) = 4. Thus the probability
formula in Section 1.6 can be re-expressed as
c/>p(V, w) = pn-wt(u)(l- p)wt(u),

where u is the error pattern u = v + w. We refer to c/>p(v, w) as the probability of
the error pattern u = v + w.

12

1 Introduction to Coding Theory

Exercises
1.8.1 Compute the weight of each of the following words, and the distance between each pair of them: VJ = 1001010, vz = 0110101, v3 = 0011110, and
v4=vz+v3.

1.8.2 Let u = 01011, v = 11010, w = 01100. Compare each of the following pairs
of quantities:
(a) wt(v + w) and wt(v) + wt(w),
(b) d(v, w) and d(v, u) + d(u, w).
We now list a number of facts concerning weight and distance. Here u, v, and
w are words of length n and a is a digit.

1. 0:::: wt(v) :::: n.
2. wt(v) = 0 if and only if v = 0.
3. 0:::: d(v, w) :::: n.

4. d(v, w) = 0 if and only if v = w.
5. d(v, w) = d(w, v).
6. wt(v+w):Swt(v)+wt(w).
7. d(v, w):::: d(v, u) +d(u, w).

8. wt(av)=a·wt(v).
9. d(av, aw) =a· d(v, w).
Most of these facts are immediately clear from the definitions of weight and
distance. In Exercise 1.8.2, the reader constructed examples of facts 6 and 7. To
construct proofs, try using the basic relation d ( v, w) = wt( v + w) and Exercises
1.7.1, 1.7.2, and 1.7.3 as necessary.

Exercises
1.8.3 Construct an example in K 5 of each of the nine rules above.
1.8.4 Prove each of the nine rules above.
These facts will be used as needed and without comment in the following
sections.

1.9 Maximum likelihood decoding
We are now ready to give more precise formulations of two basic problems of
coding theory. Let us assume we are at the receiving end of a BSC and we want
to receive a message from the transmitter at the other end. The transmitter is,
of course, one we have ourselves previously designed. In fact, the design of the
transmitter is one of the basic problems.

1.9 Maximum likelihood decoding

13

There are two quantities over which we have no control. One is the probability
p that our BSC will transmit a digit correctly. The second is the number of pos-

sible messages that might be transmitted. The actual messages are not nearly as
important as the number of possible messages. For example, only two messages
were necessary before Paul Revere set off on his famous midnight ride.
Recall that for any setS, we denote by lSI the number of elements inS. Thus
IKn I = 2n from Exercise 1.2.2.
The two basic problems of coding then are:
1.9.1 Encoding We have to determine a code to use for sending our messages.
We must make some choices. First we select a positive integer k, the length of each
binary word corresponding to a message. Since each message must be assigned a·
different binary word of length k, k must be chosen so that IMI :::: IKk I = 2k. Next
we decide how many digits we need to add to each word of length k to ensure that
as many errors can be corrected or detected as we require; this is the choice of
the codewords and the length of the code, n. To transmit a particular message, the
transmitter finds the word of length k assigned to that message, then transmits the
codeword of length n corresponding to that word of length k.
1.9.2 Decoding A word w in Kn is received. We now describe a procedure,
called maximum likelihood decoding, or MLD, for deciding which word v inC
was sent. There are actually two kinds of MLD.
1. Complete Maximum Likelihood Decoding, or CMLD. If there is one and
only one word v in C closer to w than any other word in C, we decode w
as v. That is, if d(v, w) < d(v1, w) for all VJ inC, VJ f= v, then decode w
as v. If there are several words in C closest to w, i.e., at the same distance
from w, then we select arbitrarily one of them and conclude that it was the
codeword sent.
2. Incomplete Maximum Likelihood Decoding, or IMLD. Again, if there is a
unique word v in C closest to w, then we decode w as v. But if there
are several words in C at the same distance from w, then we request a
retransmission. In some cases we might even ask for a retransmission if the
received word w is too far away from any word in the code.
We will use IMLD for the examples and exercises in this section, and throughout most of the rest of the text. We emphasize that MLD does not always work;
in particular, if too many errors were made in transmitting across the BSC, then
MLD fails.
The codeword v in C closest to the received word w is the v for which the
distance d(v, w) is least and hence, by Theorem 1.6.3, has the greatest associated
probability f/Jp(v, w) among all codewords, so is the most likely codeword sent.
Example 1.6.4 demonstrates this. Since d(v, w) = wt(v + w), the weight of the

1 Introduction to Coding Theory

14

error pattern u = v + w, Theorem 1.6.3 may be restated as follows:
</Jp(VJ, w):::; </Jp(vz, w) iff wt(VJ + w) 2: wt(vz + w);

that is, the most likely codeword sent is the one with the error pattern of smallest
weight.
Thus the strategy in IMLD is to examine the error patterns v + w for all codewords v, and pick the v which yields the error pattern of smallest weight.
Example 1.9.3 Suppose IMI = 2, and we select n = 3 and C = {000, 111}. If
v = 000 is transmitted, when will IMLD conclude this correctly, and when will
IMLD incorrectly conclude that 111 was sent? We construct Table 1.1 as follows.
Received
w

000
100
010
001
110
101
011
111

Error Pattern
OOO+w 111+ w

000*
100*
010*
001*
110
101
011
111

111
011
101
110
001*
010*
100*
000*

Decode
v

000
000
000
000
111
111
111
111

Table 1.1: IMLD table for Example 1.9.3.
The first column of Table 1.1 lists all possible words which might be received.
This is all of K 3 . The second and third columns list the error patterns v + w for
each word v in the code C. Since IMLD will select the error pattern of smallest
weight, we have put an asterisk beside the entry in column two or three of least
weight. In the last column we record the word v in the code C corresponding to
the column in which the asterisk was placed. This is the word v which IMLD will
conclude was sent for each possible word received. Thus IMLD will conclude
correctly that 000 was sent if 000, 100, 010, or 001 is received (first four rows of
Table 1.1). And IMLD will conclude incorrectly that 111 was sent if 110, 101,
011, or 111 was received (last four rows of Table 1.1 ).
Example 1.9.4 Suppose IMI =3, and we select C = {0000, 1010, 0111}withn =
4. We construct the IMLD Table 1.2, just as in Example 1.9.3 above, except
that if two or more entries in the error pattern columns have the same smallest
weight, then we do not place an asterisk in that row and record nothing (indicated
by-) in the decoding column for v. This means, for IMLD, that we request a
retransmission whenever there is a tie for smallest error pattern weight.

1.9 Maximum likelihood decoding
Received
w

0000
1000
0100
0010
0001
1100
1010
1001
0110
0101
0011
1110
1101
1011
0111
1111

15

Error Pattern

0000 + w
0000*
1000
0100*
0010
0001*
1100
1010
1001
0110
0101
0011
1110
1101
1011
0111
1111

1010+w
1010
0010
1110
1000
1011
0110
0000*
0011
1100
1111
1001
0100*
0111
0001*
1101
0101

0111 + w
0111
1111
0011
0101
0110
1011
1101
1110
0001*
0010*
0100*
1001
1010*
1100
0000*
1000*

Decode
v

0000
-

0000
-

0000
-

1010
-

0111
0111
0111
1010
0111
1010
0111
0111

Table 1.2: IMLD table for Example 1.9.4.

Exercises
1.9.5 Let IMI = 2, n = 3, and C = {001, 101}. Ifv = 001 is sent, when will IMLD
conclude this correctly, and when will IMLD incorrectly conclude that 101 was
sent?
1.9.6 Let IMI = 3 and n = 3. For each word win K 3 that could be received, find
the word v in the code C = {000, 001, 110} which IMLD will conclude was sent.
1.9.7 Construct the IMLD table for each of the following codes.
(b) C = {000,001,010,011}
(a) C = {101, 111,011}
(c) C = {0000,0001, 1110}
(d) C = {0000, 1001,0110, 1111}
(e) C = {00000, 11111}
(f) C = {00000, 11100,00111, 11011}
(g) c = {00000, 11110,01111, 10001}
(h) c = {000000, 101010,010101, 111111}

Recall that we have to choose nand C (1.9.1). Some choices are better than
others. We list three important criteria for measuring good choices:
1. Longer words take more time to transmit and decode, so n should not be
too large; that is, the information rate should be as close to 1 as possible.
2. With many messages being received per second, if IC I is large, say a few
thousand or so, the procedure for IMLD described in this section would be
too time consuming to implement. Fortunately, certain clever choices of C
admit much slicker and faster methods for IMLD.

1 Introduction to Coding Theory

16

3. If many errors are made in transmission, MLD will not work. That is, the
word MLD will conclude was sent will not be the same as the actual word
sent. So C should be chosen so that the probability that MLD will work is
very high. (We will consider this probability in the next section.)
Thus we claim that the main goal of coding theory is to find sets C of words
which are adequate when judged by the criteria above. Most of the rest of our
efforts will be devoted to this goal.

1.10 Reliability of MLD
Suppose n and C have been chosen. We now give a procedure for determining
the probability ep(C, v) that if vis sent over a BSC of probability p then IMLD
correctly concludes that v was sent.
Find the set L(v) of all words in Kn which are closer to v than to any other
word in C. Then ep(C, v) is the sum of all the probabilities </Jp(v, w) as w ranges
over L(v). That is,
ep(C, v) =

L </Jp(V, w).

WEL(v)

Note that L (v) is precisely the set of words in K n for which, if received, IMLD
will correctly conclude that v was sent. We can find L(v) from the IMLD table
constructed as in the last section. In each row of the table where v is decoded in
the last column, the word w in the first column of that row is in L ( v). And these
are all the words in L ( v).
Also observe that ep(C, v) is the sum over the words win L(v) of the probabilities of the error patterns v + w occuring during transmission.
eP can be used to compare two codes, judging them by the third criterion in
the previous section. However, it should be noted that ep(C, v) as defined ignores
the possibility of retransmission, when the received word is equidistant from two
codewords. This does lead to some anomalies (such as ep(Kn, v) > ep(C, u), for
any v in Kn and u inC, where Cis the parity check code formed from Kn), but
is a reasonable first approximation for a measure of reliability. Certainly eP ( C, v)
is a lower bound for the probability that v is decoded correctly.
Example 1.10.1 Suppose p = .90, !MI = 2, n = 3, and C = {000, 111}, as in
Example 1.9.3. If the word v = 000 is sent, we compute the probability thatiMLD
will correctly conclude this after one transmission. From Table 1.1, v = 000 is
decoded in the first four rows, so the set L(OOO) (words in K 3 closer to v = 000
than to 111) is
L(OOO) = {000, 100,010, 001}.

1.10 Reliability of MLD

17

Thus,
8p(C, 000) = cf>p(OOO, 000) +cf>p(OOO, 100) +cf>p(OOO, 010) +cf>p(OOO, 001)

= p3 + p2(1- p) + p2(1- p) + p2(1- p)
=p3+3p2(1-p)

= .972 (assuming p = .9).
If v = Ill is transmitted, we compute the probability that IMLD correctly concludes this after one transmission. First,

L(lll) = {110, 101,011, 111},
so
8p(C, 111) = ¢p(lll, 110) + ¢p(lll, 101) + ¢p(lll, 011) + ¢p(lll, Ill)

= p2(1- p) + p2(1- p) + p2(1- p) + p3
= 3 p2 (1 - p) + p3

= .972 (assuming p = .9).
Exercises
1.10.2 Suppose p = .90, IMI = 2, n = 3, and C = {001, 101}, as in Exercise 1.9.5.
(a) If v = 001 is sent, find the probability that IMLD will correctly conclude this
after one transmission.
(b) Repeat part (a) for v = 101.
Both answers in Exercise 1.10.2 are 8p(C, v) = .900. Comparing this to the
results in Example 1.10.1, we conclude that since .900 < .972, the code C =
{000, 111} is better than the code C = {001, 101}, at least when judged by the
third criterion in the last section. Our method provides a procedure, (although
somewhat inefficient when n is large) for determining when the probability that
IMLD works is high. Fortunately, most of the codes we design later on are structured so that the calculation of this probability is much easier.
Example 1.10.3 Suppose p = .90, IMI = 3, n = 4, and C = {0000, 1010, 0111},
as in Example 1.9.4. For each v inC, we compute 8p(C, v).
(a)

v = 0000

L(OOOO) = {0000,0100,0001}, (from Table 1.2)
ep(C, v) = cf>p(OOOO, 0000) + cf>p(OOOO, 0100) + cf>p(OOOO, 0001)

= p4 + p3(1- p) + p3(1- p)

= p 4 +2p 3 (1- p) = .8019

1 Introduction to Coding Theory

18

(b)

v = 1010
£(1010) = {1010, 1110, 1011}
Bp(C, v) = ¢p(1010, 1010) +cf>p(1010, 1110) +cf>p(1010, 1011)
= p4 + p3(1- p) + p\1- p)
= p 4 + 2p 3(1- p) = .8019
(c)
v = 0111

£(0111) = {0110,0101,0011, 1101,0111, 1111}
Bp(C, v) = ¢p(0111, 0110) +cf>p(0111, 0101) + ¢p(0111, 0011)

+cf>p(0111, 1101)+cf>p(0111,0111)+cf>p(0111, 1111)
= p3(1- p) + p\1- p) + p\1- p) + p2(1- p)2 + p4 + p3(1- p)
= p4 +4p3(l- p) + p2(l- p)2 = .9558.
Examining the three probabilities, we see that the probability that IMLD will conclude correctly that 0111 was sent is not too bad. However the probability that
IMLD will conclude correctly that either 0000 or 1010 was sent is horrible. Thus,
at least by the third criterion in the last section, C = {0000, 1010,0111} is not an
especially good choice for a code.

Exercises
1.10.4 Suppose p = .90 and C = {000,001, 110}, as in Exercise 1.9.6. If v =
110 is sent, find the probability that IMLD will correctly conclude this, and the
probability that IMLD will incorrectly conclude that 000 was sent.
1.10.5 For each of the following codes C calculate Bp(C, v) for each v inC using
p = .90. (The IMLD tables for these codes were constructed in Exercise 1.9.7.)
(b) C={000,001,010,011}
(a) C={101,111,011}
(c) C = {0000,0001, 1110}
(d) C = {0000, 1001,0110, 1111}
(e) C = {00000, 11111}
(f) C = {00000, 11100,00111, 11011}
(g) c = {00000, 11110,01111, 10001}
(h) c = {000000, 101010,010101, 111111}

1.11

Error-detecting codes

We now make precise the notion of when a code C will detect errors. Recall that
if v inC is sent and win Kn is received, then u = v + w is the error pattern. Any

1.11 Error-detecting codes

19

word u in Kn can occur as an error pattern, and we wish to know which error
patterns C will detect.
We say that code C detects the error pattern u if and only if v + u is not a
codeword, for every v in C. In other words, u is detected if for any transmitted codeword v, the decoder, upon receiving v + u can recognize that it is not a
codeword and hence that some error has occurred.
Example 1.11.1 Let C = {001, 101, 110}. For the error pattern u = 010, we calculate v + 010 for all v inC:
001 + 010 = 011, 101 +010 = 111, 110 +010 = 100.
None of the three words 011, 111, or 100 is in C, so C detects the error pattern
010. On the other hand, for the error pattern u = 100 we find
001 + 100= 101,101 + 100= 001, 110+ 100= 010.
Since at least one of these sums is in C, C does not detect the error pattern 100.
Exercises
1.11.2 Let C = {001, 101, 110}. Determine whether C will detect the error patterns (a) 011, (b) 001, and (c) 000.
1.11.3 For each of the following codes C determine whether or not C detects u:
(a) C = {00000, 10101,00111, 11100}
(i) u = 10101

(ii) u = 01010

(iii) u = 11011

(b) c = {1101, 0110, 1100}
(i) u = 0010

(ii) u = 0011

(iii) u = 1010

(c) C = {1000,0100,0010,0001}
(i) u = 1001

(ii) u = 1110

(iii) u = 0110

1.11.4 Which error patterns will the code C = Kn detect?
1.11.5 (a) Let C be a code which contains the zero word as a codeword. Prove
that if the error pattern u is a codeword, then C will not detect u.
(b) Prove that no code will detect the zero error pattern u = 0.
The table constructed for IMLD can be used to determine which error patterns
a code C will detect. The first column lists every word in Kn. Hence the first
column can be reinterpreted as all possible error patterns, in which case the "error
pattern" columns in the IMLD table then contain the sums v + u, for all v in C. If
in any particular row none of these sums are codewords in C, then C detects the
error pattern in the first column of that row.

20

1 Introduction to Coding Theory

Example 1.11.6 Consider the code C = {000, 111} with IMLD Table 1.1. All
possible error patterns u are in the first column. For a given u, all sums v + u as
v ranges over C are in the second and third columns of the row labeled by u. If
none of these entries are in C (that is, neither is 000 or 111), then C detects u.
Thus C detects the error patterns 100, 010, 001, 110, 101, and 011, as can be seen
by inspecting rows 2 through 7 of the table, but not the error patterns 000 or 111.

Exercises
1.11.7 Determine the error patterns detected by each code in Exercise 1.9.7 by
using the IMLD tables constructed there.
An alternative and much faster method for finding the error patterns that code
C can detect is to first find all error patterns that C does not detect; then all remaining error patterns can be detected by C. Clearly, for any pair of codewords v
and w, if e = v + w then e cannot be detected, since v + e = w, which is a codeword. So the set of all error patterns that cannot be detected by C is the set of all
words that can be written as the sum of 2 codewords.
Example 1.11.8 Consider the code {000, 111}. Since

000+000=000,000+ 111 =Ill and Ill+ 111 =000,
the set of error patterns that cannot be detected is {000, 111}. Therefore all error
patterns in K 3 \ {000, 111} can be detected.
Example 1.11.9 Let C = {1000, 0100, 1111}. Since 1000+ 1000 = 0000, 1000+
0100 = 1100, 1000 + 1111 = 0111 and 0100 + 1111 = 1011, the set of error patterns that cannot be detected by Cis {0000, 1100, 0111, 1011 }. Therefore all error
patterns in K 4 \ {0000, 1100, 0111, 1011} can be detected.

Exercises
1.11.10 Find the error patterns detected by each of the following codes and compare your answers with those Exercises 1.11. 7.
(a) C={101,111,011}
(b) C={000,001,010,011}
(c) C={0000,0001,1110}
(d) C={0000,1001,0110,1111}
(e) C = {00000, 11111}
(f) C = {00000, 11100,00111, 11011}
(g) c = {00000, 11110,01111, 10001}
(h) c = {000000, 101010,010101, 111111}
There is also a way of determining some error patterns that code C will detect without any manual checking. First we have to introduce another number
associated with C.
For a code C containing at least two words the distance of the code C is
the smallest of the numbers d(v, w) as v and w range over all pairs of different

1.11 Error-detecting codes

21

codewords in C. Note that since d(v, w) = wt(v + w), the distance of the code
is the smallest value of wt( v + w) as v and w, v f. w range over all possible
codewords.
The distance of a code has many of the properties of Euclidean distance; this
correspondence may be useful to assist in understanding the concept of the distance of a code.

Example 1.11.11 Let C = {0000, 1010, 0111}. Then d(OOOO, 1010) = 2, d(OOOO,
0111) = 3, and d(1010, 0111) = 3. Thus the distance of Cis 2.
Exercises

1.11.12 Find the distance of each of the following codes.
(b) C = {000,001,010,011}
(a) C = {101, 111,011}
(c) C = {0000,0001, 1110}
(d) C = {0000, 1001,0110, 1111}
(e) C = {00000, 11111}
(f) C = {00000, 11100,00111, 11011}
(g) c = {00000, 11110,01111, 10001}
(h) c = {000000, 101010,010101, 111111}

1.11.13 Find the distance of the code formed by adding a parity check digit to
Kn.

Now we can state a theorem which helps to identify many of the error patterns
a code will detect.

Theorem 1.11.14 A code C of distanced will at least detect all non-zero error
patterns of weight less than or equal to d - 1. Moreover, there is at least one error
pattern of weight d which C will not detect.
Notice that C may detect some error patterns of weight d or more, but does
not detect all error patterns of weight d.

Proof:
Then

Let u be a nonzero error pattern with wt(u) :::: d- 1, and let v be in C.
d(v, v + u) = wt(v + v + u) = wt(u) <d.

Since C has distance d, v + u is not in C. Therefore C detects u. From the
definition of d, there are codewords v and win C with d(v, w) =d. Consider the
error pattern u = v + w. Now w = v + u is in C, so C will not detect the error
pattern u of weight d.
D
A code is an t error-detecting code if it detects all error patterns of weight at
most t and does not detect at least one error pattern of weight t + 1. So, in view of
Theorem 1.11.14, if a code has distanced then it is ad- 1 error-detecting code.

Example 1.11.15 The code C = {000, 111} has distanced= 3. By Theorem
1.11.14, C detects all error patterns of weight 1 or 2, and C does not detect the

1 Introduction to Coding Theory

22

only error pattern of weight 3, 111. The only error pattern not covered by Theorem
1.11.14 is 000. But by Exercise 1.11.5 we know that 000 is not detected.
Theorem 1.11.14 does not prevent a code C from detecting error patterns of
weight d or greater. Indeed, C usually will detect some such error patterns.
Example 1.11.16 The code C = {001, 101, 110} has distanced= 1. Since d1 = 0, Theorem 1.11.14 does not help us determine which error patterns C will
detect. But it does tell us that there is at least one error pattern of weight d = 1
which C will not detect. As we saw in Example 1.11.1, such an error pattern is
100. Note, however, that C does detect the error pattern 010 of weight d = 1.

Exercises
1.11.17 The code C = {0000, 1010,0111} has distanced= 2. Using Exercise
1.11.5, show that the error pattern 1010 is not detected. Show that this is the only
error pattern of weight 2 that C does not detect. Find all error patterns that C
detects.
1.11.18 Find all error patterns which the code C3 of Example 1.3.3 will detect.
Note that C3 is a single error-detecting code.
1.11.19 For each code C in Exercise 1.11.12 find the error patterns which Theorem 1.11.14 guarantees C will detect.
1.11.20 Let C be the code consisting of all words of length 4 which have even
weight. Find the error patterns C detects.

1.12 Error-correcting codes
If a word v in a code C is transmitted over a BSC and if w is received resulting
in the error pattern u = v + w, then IMLD correctly concludes that v was sent
provided w is closer to v than to any other codeword. If this occurs every times
the error pattern u occurs, regardless of which codeword is transmitted, then we
say that C corrects the error pattern u. That is, a code C corrects the error pattern
u if, for all v in C, v + u is closer to v than to any other word in C. Also, a code
is said to be an t error-correcting code if it corrects all error patterns of weight at
most t and does not correct at least one error pattern of weight t + 1.

Example 1.12.1 Let C = {000, 111}.

(a) Take the error pattern u = 010. For v = 000,
d(OOO, v + u) = d(OOO, 010) = 1 and
d(111, v+ u) = d(111, 010) = 2.

1.12 Error-correcting codes

23

And for v = 111,
d(OOO, v + u) = d(OOO, 101) = 2

d(111,v+u)=d(111,101)=1.
Thus C corrects the error pattern 010.
(b) Now take the error pattern u = 110. For v = 000,
d(OOO, v + u) = d(OOO, 110) = 2 and
d (111, v + u) = d (111, 11 0) = 1.

Since v + u is not closer to v = 000 than to 111, C does not correct the error
pattern 110.
The IMLD table can be used to determine which error patterns a code C will
correct. In each error pattern column of the table, all possible error patterns (which
means each word in Kn) occurs once and only once (for if the error pattern u occurs twice in a column for some codeword v, then u occurs in rows corresponding
to distinct received words, say WI and w2; thus u = v +WI = v + w2, which is
impossible for WI =I= w2). Also, an asterisk is placed beside the error pattern u
in the column corresponding to a codeword v in the IMLD table precisely when
v + u is closer to v than it is to any other codeword. Therefore an error pattern u
is corrected if an asterisk is placed beside u in every column of the IMLD table.
Example 1.12.2 For the code C = {000, 111}, the IMLD table is in Table 1.1. In
every row of the table where the error pattern 010 occurs (rows 3 and 6), IMLD
correctly concludes which word v was sent. Also, in at least one row (row 4)
where the error pattern 110 occurs, if 111 is sent and 001 is received, IMLD
incorrectly concludes that 000 was sent. Note that this code corrects the error
patterns 000, 100, 010, and 001 which receive an asterisk each time they occur.
So C is a 1 error correcting code.
Example 1.12.3 Let C = {0000, 1010, 0111}. The IMLD table for Cis Table 1.2.
The code C will not correct the error pattern u = 1010. This error pattern occurs
in the rows where w = 0000, 1010, and 1101. In only one case, where w = 1101,
does IMLD correctly conclude which word v was sent. Note that the error pattern
u = 1010 receives an asterisk only in the column for v = 0111 and not in the other
two columns. C does correct the error patterns 0000, 0100 and 0001.
Example 1.12.4 Let C = {001, 101, 110}. Does C correct the error pattern u =
100? We construct only the three rows of the IMLD table where 100 appears.
Since u = v + w and we know u and v, we can find the received words from
w = u + v. Notice that u = 100 does not receive an asterisk in every column in
the following table, soC does not correct 100.

1 Introduction to Coding Theory

24

Received
w

101
001
010

001+w
100
000*
011

Error Pattern
101 + w 110+w

000*
100
111

Decode
v

011
111
100*

101
001
110

Exercises
1.12.5 Let C = {001, 101, 110}. Does C correct the error pattern u = 100? What
about u = 000?
1.12.6 Prove that the same error pattern cannot occur more than once in a given
row of an IMLD table.
1.12.7 Prove that the zero error pattern is always corrected.
1.12.8 Which error patterns will the code C = Kn correct?

The distance of a code can be used to devise a test for error-correcting which
avoids at least some of the manual checking from the IMLD table. The next
theorem gives the test. Recall that the symbol Lx J denotes the greatest integer
less than or equal to the real number x. For example, L5/2J = 2, L3J = 3, and
L1/2J = o.
Theorem 1.12.9 A code of distanced will correct all error patterns of weight less
than or equal to L(d -1)/2J. Moreover, there is at least one error pattern of weight
1 + L(d- 1)/2J which C will not correct.
Proof: Let u be an error pattern of weight wt(u) s (d- 1)/2. Let v and w be
codewords inC with w =f. v. We want to show that d(v, v + u) < d(w, v + u).
d(w, v +u) +d(v+u, v) 0:::: d(w, v)

o:=:d
d(w, v + u) + wt(u) 0:::: 2wt(u) + 1
d(w,v+u)o:=:wt(u)+1
0:::: d(v, v

+ u) + 1

since wt(u) = d(v + u, v), and 2wt(u) + 1 s d.
Therefore C corrects u. Now let v and w be codewords with d(v, w) =d.
Form an error pattern u by changing d - 1 - L(d- 1) /2J of the d 1's in v + w to
O's. Then
d(v,v+u)=wt(u)=1+L(d-1)/2J, and
d(w, v +u) = wt(w + v+u) = d(v +w, u)

= d- (1 + L(d -1)/2J).

1.12 Error-correcting codes

25

If dis odd, say d = 2t + 1, then

d (v, v + u) = wt( u) = 1 + (2t) /2 = 1 + t, and
d (w, v + u) = 2t + 1 - ( 1 + t) = t,

so d ( v, v + u) > d ( w, v + u). And if d is even, say d = 2t, then
d(v,v+u)=l+Lt-1/2J =t, and
d (w, v + u) = 2t - t = t.
In either case, d(v, v + u) :=::: d(w, v + u), so v + u is not closer to v than to the
codeword w. Thus C does not correct the error pattern u.
D
In view of this theorem it is clear that any code of distance d is a L(d - 1) /2 J
error-correcting code.
Example 1.12.10 The code C = {000, Ill} has distanced= 3. Since L(d1)/2J = 1, Theorem 1.12.9 ensures that C corrects all error patterns of weight
0 or 1. As we observed in Example 1.12.1, C does correct error patterns 000, 100,
010, and 001. The error pattern 110 has weight 1 + L(d -l)/2J = 2, and we saw
that C does not correct 110.

Theorem 1.12.9 does not prevent a code C of distanced from correcting error
patterns of weight greater than L(d - 1) /2 J .
Example 1.12.11 Let C = {001, 101}. Then d = 1. The error pattern u = 011 has
weight 2, which is greater than 1 + L(d -l)/2J = 1. As the following piece of the
IMLD table shows, C does correct u = 011.
w

010
110

001+ w
011*
111

101+ w
111
011*

v
001
101

Exercises
1.12.12 For each of the following codes C: (i) determine the error patterns that C
will correct (the IMLD tables for these codes were constructed in Exercise 1.9.7),
and (ii) find the error patterns that Theorem 1.12.9 guarantees that C corrects.
(b) C = {000, 001,010, 011}
(a) C = {101, 111, 011}
(c) C = {0000,0001, 1110}
(d) C = {0000, 1001,0110, 1111}
(e) C = {00000, 11111}
(f) C = {00000, 11100,00111, 11011}
(g) c = {00000, 11110,01111, 10001}
(h) c = {000000, 101010,010101, 111111}
1.12.13 Use the technique described in Example 1.12.11 to decide whether or not
the following error patterns are corrected by the accompanying code.

I Introduction to Coding Theory

26

(a) C = {000000, 100101,010110,001111,110011,101010,011001, 111100}

(i) u = 001000

(ii) u = 000010

(iii) u = 100100

(b) C= {1001011,0110101, 1110010, 1111111}
(i) u = 0100000

(ii) u = 0101000

(iii) u = 1100000

1.12.14 For each code in Exercise 1.12.12, find an error pattern of weight L(d1) /2 J + 1 that C does not correct.
1.12.15 Let C be the code consisting of all words of length 4 having even weight.
Determine the error patterns that C will correct.

u,

u,

1.12.16 Let
and u2 be error patterns of length n, and assume that
and u2
agree at least in the positions where a 1 occurs in u i· Prove that if a code C will
correct u2, then C will also correct u J.
As we have observed, error patterns of small weight are more likely to occur
than error patterns of lar~e weight (Theorem 1.6.3). Therefore, in designing codes,
we should concentrate on being able to correct, or at least to detect, error patterns
of small weight.

