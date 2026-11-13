Chapter 10

Classical Cryptography
Cryptography is about communication in the presence of adversaries. 1 The bestknown example is that of confidentiality or privacy-maintaining secrecy in communications over an unsecured channel. The cryptographic solution is encryption,
which modifies the data in an attempt to render the communication unintelligible
to all but the intended recipients. Cryptography can be regarded as the science
of mathematical techniques for protecting data (from malicious or unauthorized
actions) by transforming the data itself.
In addition to confidentiality, cryptography can be used to satisfy various information security objectives collectively known as authentication. Data integrity
provides detection of data manipulation, including alteration, delay, and replay of
messages. Message (or data origin) authentication provides proof of the source
of the information. Identification corroborates the identity of an entity. Nonrepudiation is a service which prevents entities from denying previous commitments. A digital analogue of the hand-written signature, specific to the message,
is fundamental to many authentication schemes.
Cryptography is closely connected with cryptanalysis, the study of mathematical and other techniques for defeating information security objectives; together,
these areas are known as cryptology. Cryptanalysis under various assumptions
concerning the capabilities of the adversary is an essential part of applied cryptology, providing some measure of confidence in schemes for which mathematical
proof of security is not available.
Section 10.1 establishes the basic framework used in Chapters 10-12. In classical cryptography, privacy of communications depends on a secret shared between the communicating parties; Section 10.2 examines a few such symmetrickey schemes. The one-time pad is of particular interest, as an example of a simple
scheme that cannot be broken regardless of the computing power of the adversary.
Unfortunately, the one-time pad is rather unwieldy in practice due to the amount
of key material required. Section 10.3 discusses the Data Encryption Standard, the
1 Ronald L. Rivest, from [71].

227

228

10 Classical Cryptography

best-known symmetric-key scheme in history. Rather than the unconditional security of the one-time pad, DES was designed so that it would be "computationally
infeasible" for an adversary to break the scheme.
A basic symmetric-key property is that the secret in the scheme must be shared
with correspondents-the ability to create a secret message cannot be separated
from the ability to interpret such a message. A landmark 197 6 paper by Diffie and
Hellman [27] introduced the concept of public-key cryptography; the separation
of creation and interpretation capabilities is a consequence. Security relies on the
apparent intractability of various computational problems. Chapter 11 contains an
overview of several number-theoretic topics fundamental to public-key schemes.
Chapter 12 discusses public-key concepts and methods.

10.1

Encryption schemes

The following framework will be used in the study of cryptographic tools or primitives.

• An alphabet 5l is a finite set.
• A message space % over 5l consists of strings of symbols from the alphabet. For example, if 5l = {0, 1} then messages consist of strings of Os and
Is; the set of messages of length n is denoted {0, l}n, and the set {0, 1}*
consists of all messages of finite length.
• An encryption scheme or cipher consists of message spaces % and C, a
set '1( (known as the key space), and functions Ek : %--+ C, Dk : C--+ %
fork E '1( with Dk(Ek(m)) = m for all mE%. Ek is called an encryption
function, and Dk is the corresponding decryption function. In this setting,
elements of% are plaintext, Cis called a ciphertext space, and elements of
C are ciphertext.
• In some schemes, keys are written as ordered pairs k = (e, d) where e is
used for encryption and dis used for decryption. In this context, (e, d) is
called a key-pair, and we will write Ee = Ek and Dd = Dk.
Figure 10.1 is a schematic of the basic scenario where Alice wishes to pass
a private message m to Bob in the presence of an adversary Eve. In traditional
symmetric-key ciphers (see Section 10.2), the secure channel is required in order
to pass the keys themselves, and may represent a trusted courier or other reliable method for maintaining privacy. In public-key schemes (see Chapter 12), the
channel represents a method of verifying the authenticity of the public portion of
a key.
Adversaries may be classified by capability. A passive adversary is limited
to eavesdropping on the unsecured portion of the conversation between Alice and
Bob. An active adversary, in addition to eavesdropping, may alter or inject messages, or perhaps even block transmission entirely. Cryptography can hope to pro-

10.1 Encryption schemes

229

encryption
Ee (m) = c
'-------'

decryption
C

UNSECURED CHANNEL

--------~-------

Dd(c)=m

Bob

Alice

Eve

Figure 10.1: Communication usine encryption.

vide secrecy, and detect altered or forged messages. Delivery of messages in the
face of a sufficiently powerful adversary cannot be guaranteed, although encrypted
messages may be sent periodically to discover communication disruptions.
Informally, an encryption system is considered breakable if an adversary can
systematically recover plaintext from ciphertext (or even worse, the key can be
computed). Three kinds of security are noted:
• The system is unconditionally secure if the adversary can gain no knowledge about plaintext (except possibly its length), regardless of the amount
of ciphertext available and the computational resources available to the adversary.
• A system is computationally secure if breaking it is "computationally infeasible" under some prescribed level of available computing resources, and
using the best known cryptanalytic techniques.
• If breaking is provably at least as difficult as solving a mathematical problem believed to be intractable, the system is said to be provably secure.

These intuitive notions of security will be made somewhat more precise in the
following sections.
In 1883, Auguste Kerckhoffs published what Kahn calls the second great book
in cryptology of the "outward-looking kind." 2 Several fundamental principles for
selecting ciphers were listed: the system should be, if not theoretically unbreakable, unbreakable in practice; compromise of the system should not inconvenience
the correspondents; the key should be rememberable without notes and should be
easily changeable; the cryptograms should be transmissible by telegraph; the apparatus or documents should be portable and operable by a single person; the
system should be easy, neither requiring knowledge of a long list of rules nor involving mental strain. The second of these is sometimes called Kerckhoffs' prin2 Kahn [48] credits Naples-born Giovanni Battista Porta as "the first to delineate a coherent image of
cryptology" in a 1563 work, but whose "views no longer sufficed after the invention of the telegraph."

10 Classical Cryptography

230

ciple, and says that security of the cipher should rest with the keys alone; that
is, security is maintained even in the face of an adversary in possession of the
encryption scheme.
Attacks on the communication of Figure 10.1 typically have the objective of
recovering plaintext from ciphertext, or even the key itself, although other more
limited information (such as particular plaintext bits) may be targeted. Traffic
analysis attempts to learn something about the messages by examination of the
pattern of transmission. For example, a sudden flurry of exchanges may be notable
even if the underlying messages cannot be discovered. We are mainly interested
in attacks against the encryption scheme itself.

1. In a ciphertext-only attack, the adversary attempts to recover plaintext or
the key by observing some amount of ciphertext. A scheme vulnerable to
this kind of attack is considered completely insecure.
2. In a known-plaintext attack, the adversary is in possession of some quantity
of plaintext and corresponding ciphertext. The typical case is that the plaintext and ciphertext pairs correspond to some (unknown) key, and the goal is
to recover the key or decrypt some additional ciphertext which is believed
to have been encrypted with the same key.
3. In a chosen-plaintext attack, ciphertext corresponding to plaintext selected
by the adversary is available. A possible scenario is that the adversary has
obtained temporary access to the equipment (but not the key) used for encryption. If the ciphertext may be chosen interactively based on previous
results, the attack is called adaptive.
4. In a chosen-ciphertext attack, plaintext corresponding to ciphertext selected
by the adversary is available.
Other possibilities include implementation-specific and tempest attacks which exploit device properties; timing of computations and power consumption, for example, have been used against certain systems [53, 52]. Bribery, blackmail, and
other "give-me-the-key" attacks have been historically rather successful.

10.2

Symmetric-key encryption

Many classical ciphers have the property that a "secret" is shared between parties,
and knowledge of this information allows both parties to encrypt and decrypt.
More precisely, an encryption scheme is said to be symmetric-key if finding Dd
from e and Ee from d require roughly the same resources.
A simple substitution cipher consists of 1-1 mappings k on the source alphabet. Encryption is performed by applying k to each symbol in the message;
i.e., if m = mom1 · · ·, where m; are symbols from the alphabet, then Ek(m) =
Ek(momi · · ·) = k(mo)k(m!) · · ·.

10.2 Symmetric-key encryption

231

Example 10.2.1 The shift cipher is a special case of the substitution cipher. The
key k is a fixed shift 0.::; k < n on the symbols {ao, ... , an-d of the alphabet,
mapping aj ~--+ a(j+k)modn· 3 The popular rot13 encryption function corresponds
to k = 13 on the source alphabet {a, ... , z}, rotating by 13 positions; i.e., the letters
are numbered 0-25, and rot13 adds 13 to each source letter, reducing the result
modulo 26. 4 As a visual clue, lowercase letters will be used for plaintext, and
uppercase for ciphertext. For example, since

rot13(rotate) = rot13(17, 14, 19, 0, 19, 4) = (4, 1, 6, 13, 6, 17) = EBGNGR,
the plaintext 'rotate' is encrypted to ciphertext 'EBGNGR'. This particular shift
also has the property that rot13(rot13(m)) = m; i.e., encryption and decryption
are the same function. Julius Caesar is reputed to have used a shift cipher with
k=3.
The shift cipher is completely insecure against a chosen-plaintext attack. In
fact, a ciphertext-only attack is easily mounted, since the 26 possible keys can be
examined exhaustively.
A simple substitution cipher may resist exhaustive search of the keyspace,
even if the source alphabet is small. However, frequency analysis may be effective if the underlying source text characteristics are known. For example, Figure 10.2 lists single-character frequencies obtained from a modest sampling of
English-language newspapers and novels [3]. Ciphertext from simple substitution respects the frequency distribution of the source. If the source statistics are
roughly according to Figure 10.2, for example, then the most common ciphertext
symbol probably corresponds to 'e'. Similarly, the symbols U. q, x, z} are significantly less frequent in this kind of source, and likely correspond to less frequent
ciphertext symbols. Statistics from digrams (pairs of letters) and trigrams (triples
of letters) can be used in a similar fashion.
Example 10.2.2 Consider the case that the alphabet is the 26letters a-z, and the
substitution cipher is a permutation on these letters, perhaps
k=(abcdefghijklmnopqrstuvwxyz)
T U V WX Y Z B I K E A C D F G H J L M N 0 P Q R S

where the source letters in the first row are mapped to the ciphertext letters in
the second row; e.g., Ek(trek) = MJXE. This particular permutation is easy to
remember, since it is constructed by placing the keyword 'BIKE' under the letter
'h' and then filling in the rest of the letters in order.
3The "mod" notation is reviewed in Chapter II; here, "(} + k) mod n" adds integers j and k as
usual, subtracting n if the result exceeds n - I.
4The source documentation with the GNU Emacs editor notes that "Rotl3 encryption is sometimes
used on USENET.. .for material some might consider offensive."

232

10 Classical Cryptography

13

%

12 7

-

12
11

10
9.1

-

9
8.2

8

-

7.5

r-

7.0

-

7

-

6.1
r-

6
5

4.3

4.0

2.8
r-

3

2.2

1.5

r-

r--

.77

15Ii

a b c d e f g h

2.8
r-

2.4

20

,--__:_

1

0

6.3

~r-

-

-

4

2

6.7

1

2.4

-

1.9

n!O

2.0

-

1.0
,---;

.15

.07

~

J k 1 m n o p q r s t u v w x y z

Figure 10.2: Frequencies of single characters in English text.

The scheme is completely insecure against a chosen-plaintext attack (since
the key can be recovered by encrypting the message m = ab · · · z). Having only
ciphertext available makes things a little more difficult. Although the exhaustive
attack of trying each possible key in turn may be infeasible (since the keyspace is
of size 26! ~ 4 · 1026 ), the scherrie may be subject to frequency analysis.
A simple substitution cipher is sometimes called a monoalphabetic substitution cipher. Attacks based on frequency analysis become infeasible if the message space is over some large alphabet. Small alphabets, such as that in Example
10.2.2, can be expanded by combining several symbols and forming a message
space over this much larger alphabet. The Data Encryption Standard (DES, to be
discussed in Section 10.3.2) in its basic form is a substitution cipher over a source
alphabet of size 264 .
Polyalphabetic ciphers attempt to hide the original source frequencies by mapping each source symbol to one of several possible ciphertext symbols. The Vigenere cipher is a well-known example. Symbols from the source alphabet .9l. are
identified with integers in [0, l.9l.l), and a keyword k = ko · · · kn-1 is selected for
some ki E .9l.. Encryption is performed on n-character blocks of the source by
character-wise addition modulo l.9l.l with the keyword.
As an example, suppose .9l. consists of the symbols a-z numbered 0-25. The
following table illustrates the procedure on a short plaintext using key 'KEY'

10.2 Symmetric-key encryption
plaintext
+key

ciphertext

233

she sells sea shells by the seashore
KEY KEYKE YKE YKEYKE YK EYK EYKEYKEY
CLC CIJVW QOE QRIJVW ZI XFO WCKWFYVC

where the addition is modulo 26. Decryption is performed by subtracting modulo
26 the keyword from the ciphertext.
The simple analysis based on source frequency characteristics is partially
thwarted by the mapping of alphabet symbols to different ciphertext letters, depending on the position in the source; e.g., the source letter 'e' is mapped onto the
set {C, I, 0} in the above example. However, if the key length f. can be discovered,
then monoalphabetic frequency analysis can be used on the submessages formed
from letters that are a multiple of f. apart in the ciphertext. In the above fragment,
the submessage formed from the ciphertext in positions 0, 3, 6, ... , for example,
is obtained by adding (modulo 26) the first letter of the 3-letter key to plaintext
letters from the corresponding positions.
In 1863, Friedrich Kasiski proposed a method (now known as a Kasiski examination) to recover the key length via analysis of the distances separating identical
fragments of ciphertext. 5 Occasionally, identical fragments of source text will
align with the same portion of the key, resulting in identical portions of ciphertext. In the example, this occurs with the 4-character fragment 'ells'. Typically,
there will also be "accidental" matches-identical fragments of ciphertext which
did not result from identical fragments of plaintext-although these tend to be less
likely with longer match lengths. In the case of a "true" match, the key length divides the difference in positions. The possibility of accidental matches means that
it may not be enough to look only at the greatest common divisor of the distances,
and in general the larger and more common factors are examined first as key length
candidates.
Example 10.2.3 A Vigenere cipher has been used on "typical" English source
text which was first processed to remove punctuation and blanks so that the source
consists only of the 26letters a-z. The following ciphertext was produced:
offset
0
50
100
150
200
250
300

ciphertext
UPVZB BVUPN KKFOL OGAKU FBTKF LFXUJ VIPZV KFZXO FIDLO ONLUP
KKFUZ OMQFQ MQXKU AFIUP VVVVK KFDFL DMFIU PVVFI ZVTMU XDBZY
FVVYF ZTHBA ZQHEY LTXVU JVXFM IDRSQ EJNCI PVZZQ HQEYJ BZQHB
YHTWL OUWND OLVUJ VREZA JHTWW VPTZW VLVDM TROPV XWIMN KJBVE
FITKV XRQEL FZOBY HSMND TVFOJ DZQHB YLOOZ QTQXK UISLS LNLUP
RESWB HOEZQ HERVC MRWJV XWIMR LSISR WMIHF TZQHN CXUBV UJVXF
JZTOJ VXGJA REMMU GPEEG PEEWP BYHXI KHS

The spaces in the ciphertext were added for readability, and are not represented in
the offsets.
5 Kahn [48] writes that Kasiski died in 1881 "almost certainly without realizing that he had wrought
a ·revolution in cryptology."

10 Classical Cryptography

234

The 3-character fragment 'ZQH' appears in several places, three of which are
underlined. The corresponding offsets are 110, 138, and 226, respectively. It
is likely that the keyword length .€ divides the difference of any two of these
offsets. The differences 138- 110 = 28 = 22 7 and 226- 110 = 22 29 suggest
.€ I (2 2 7, 22 29) = 22 . The case .€ = 1 corresponds to a simple shift.
At this stage, the cryptanalyst may examine the frequency distributions for
each candidate key length, looking for those which have the characteristics of the
expected source language. The frequencies are tabulated corresponding to each
position of the key; e.g., for the candidate keylength .€ = 2, the frequencies are
tabulated for the submessage formed from the characters in the even offsets, then
from the odd offsets. This gives, for example,
submessage

c offsets

frequencies of letters in submessage (sorted)

2 0,2,4, ...
1, 3, 5, ...
4 0,4,8, ...
1, 5, 9, ...
2,6, 10, ...
3, 7, 11, ...

19 16 12 12 11 10 9 8 8 7 6 6 55 55 54 4 3 2 2 2 I 0 0
14 14 13 10 10 10 9 9 9 8 8 7 6 6 5 5 5 3 3 3 2 2 2 2 1 0
12 10 10 7 6 6 6 5 5 4 2 2 2 1 I 1 1 1 1 1 0 0 0 0 0 0
10 9 8 7 5 555554332221110000000
12 11 8 8 7 555442222211110000000
9 9 8 8 7 555433333222110000000

For a given source distribution, we expect that each line for the correct key length
should reflect the source frequencies. In this example, the frequencies suggest that
.€ = 4 is more likely than .€ = 2, although the limited amount of ciphertext hinders
the analysis.
In short, the analysis so far suggests that .€ = 4 is a likely candidate. The
frequency analysis of Figure 10.2 shows that 'e' is significantly more prevalent
than any other letter in typical English text. We suspect that one of the higherfrequency ciphertext letters, in each of the .€ = 4 lines, corresponds to plaintext
letter 'e'. In this particular example, choosing 3 or 4 (or perhaps a few more)
ciphertext letters from each of the lines in the .€ = 4 case appears likely to yield
the mapping from plaintext letter 'e'.
The following table lists the 4 most frequent ciphertext letters corresponding
to the higher frequencies in each line of the .€ = 4 case, along with the appropriate
key letters which map 'e' to each of these ciphertext letters:
common ciphertext
letters for C = 4
F J UP
B V MI
VZRX
KQHL

(c-'e')mod26

corresponding
key letters
B F QL
XR I E
R V NT

G MD H

At this stage, an exhaustive search using keywords built from the likely key letters
could be performed by machine. In the example, this requires 44 = 256 decryptions (using keywords such as 'BXRG').

10.2 Symmetric-key encryption

235

If the keyword was chosen from a dictionary, then it may be especially easy to
locate. In the example, this restriction leads to words such as 'LEND' and 'BIRD',
and the second of these yields:
ciphertext
- key
plaintext

UPVZB BVUPN KKFOL OGAKU FBTKF LFXUJ VIPZV KFZXO FIDLO ONLUP
BIRDB IRDBI RDBIR DBIRD BIRDB IRDBI RDBIR DBIRD BIRDB IRDBI
thewa terof thegu lfstr etche doutb efore hergl eamin gwith

which appears to be English text. The complete decryption, with punctuation and
case restored, is:
The water' of the Gulf stretched out before her, gleaming with the million
lights of the sun. The voice of the sea is seductive, never ceasing,
whispering, clamoring, murmuring, inviting the soul to wander in abysses of
solitude. All along the white beach, up and down, there was no living thing
in sight. A bird with a broken wing was beating the air above, reeling,
fluttering, circling disabled down, down to the water. 6

The 'ZQH' ciphertext fragments used in the Kasiski test correspond to plaintext 'ing'. Longer repeated fragments, such as 'NLUP' (corresponding to 'with')
and 'ZQHBY' (corresponding to 'ingth'), each appearing twice, could have been
chosen. We conveniently avoided fragments such as 'PVZ' (appearing at positions
I and 135) which are accidental matches and correspond to 'hew' and 'mur', respectively. These fragments suggest keylengths dividing 134, which would have
been incorrect.
More challenging examples of cryptanalysis of a Vigenere cipher may be
found in Kahn [48] and Stinson [86]. In particular, the indices of coincidence
defined in [86] provide a more methodical approach to finding the keylength and
the keyword candidates.
The simple substitution and Vigenere ciphers are examples of block ciphers,
since they transform a message by applying a fixed mapping to blocks consisting of a fixed number of characters. They are memoryless in the sense that the
mapping of a block does not depend on its position in the message. Stream ciphers allow the mapping to depend on the position within the message, and are
sometimes called state ciphers.
Encryption schemes are typically defined for a binary alphabet .J2L = {0, 1},
and block (and stream) ciphers are defined on some fixed number of bits, called
the blocklength. Messages over an alphabet .J2L = {ao, ... , an-I} can be regarded
as written over a binary alphabet by associating a j ++ j and writing j as a binary number with pog 2 n l bits. In the case of a 26-letter alphabet as in Example
10.2.2, each letter is replaced by a 5-digit binary number, with 'g' ++ 6 = 00110,
for example. The encryption and decryption functions in these examples are then
considered to operate on blocks of length 5.
6From The Awakening by Kate Chopin.

236

10 Classical Cryptography

For messages m and m' over {0, 1} of the same length, bitwise addition modulo
2 will be written m EBm'; e.g., 1100EB 1010 = 0110. Programmers know this operation as "exclusive or," sometimes written XOR, and amounts to the schoolchild's
paradise where addition is never complicated by carry.
Example 10.2.4 The Vemam cipher is a stream cipher on .9L = {0, 1}. Keys also
consist of messages over .91., and encryption maps m r--:> c = m EB k. By the usual
properties of modular arithmetic, it is easy to see that the plaintext is recovered
from c by c EB k = m EB k EB k = m EB 0 = m; i.e., decryption is the same as encryption.
If the key digits (known as the keystream) are the result of independent Bernoulli trials with probability 1/2 (e.g., independent tosses of a fair coin) and are
used only once, then the cipher is known as a one-time pad, and is unconditionally
secure against a ciphertext-only attack. The intuitive idea is that every message (of
the same length as the ciphertext) maps to the ciphertext for some key, and there
is no reason to favor one key over another. Shannon's work in the 1940s [80]
made these notions precise, showing that, roughly speaking, any unconditionally
secure symmetric-key encryption scheme requires a key as long as the message.
(A more precise statement on the requirements for keys can be given in terms of
the entropy associated with the message space.)
In some early applications of the Vernam cipher, the key information was written on notepads and pages were discarded after a single use, leading to the name
"one-time pad." Ciphertext obtained from the same key would enable an adversary to learn something about the underlying messages; e.g., if c = m EB k and
c 1 = m' EBk then
cEBc' = (mEBk)EB(m'EBk) =mEBm'
which may allow cryptanalysis (in particular, m = m' is immediately discovered).
The Codebreakers has a few amusing accounts of early attempts to manage
the quantity of key material required, and the problem continues to limit the application of the one-time pad. According to Peter Wright of British Intelligence
[101], the Russians ran short of key material during the early years of wwn, and
duplicate sets of the pads were distributed to different embassies in the West.
The "VENONA codebreak" exploited this misuse of the scheme, examining vast
amounts of ciphertext for matches across several channels used by the Russian
government. 7
Widely-used ciphers such as DES sacrifice the unconditional security of the
one-time pad for more manageable schemes that are hoped to be computationally
7As part of a series on the "Cold War Experience" in 1998, the Cable News Network (CNN)
reported that "The VENONA project at the U.S. National Security Agency cracked many supposedly
secure Soviet messages, including some that employed one-time pads. The project lasted from 1943
to 1980, and some of its records were declassified in 1995." Unfortunately, CNN neglected to note that
the scheme failed the fundamental single-use requirement, and claimed that the one-time pad "can also
be cracked by painstaking trial and error, trying all of the millions of possible combinations."

10.2 Symmetric-key encryption

237

secure. The requirement of randomly-generated keys should also be noted. Common "random number generators" produce a sequence of numbers completely
determined by an initial seed chosen from a finite set. Although they possess
many of the features useful in simulations, they are not random in the sense of
unpredictability required in cryptography. 8
Exercises
10.2.5 The following ciphertext was obtained under a Vigenere cipher of the type
discussed in Example 10.2.3:
offset
0
50
100
150

ciphertext
VHVVG NRWGA EGCLJ RVHVO GAUHT OWWJE FSROJ LVIFQ KNKKG IIDPG
VUJAM HLUJW CLCRY EUWJE DVGLM HUBFW JTFEG CFPGV LOPEI DDLVW
QOLUE ALVGM VVJAC OCTKD EKKKG MRVBE BHRLR QPEUW QMFUT ONLPD
RBNIX KVBLM

Recover the key word of the cipher, given that the underlined triple of letters
represents a very common three-letter word, and that the overscored letters 'AE'
represents the combination 'an' at the beginning of a three-letter word.
10.2.6 Write a program to implement a substitution cipher of the form in Example 10.2.2 (the authors used awk). The program should accept a keyword and
alignment position as arguments. Encrypt a sample of at least 300 characters of
English-language text, and then perform a cryptanalysis using frequency information and knowledge of the scheme used (but proceed as if the keyword itself is
unknown).
10.2.7 This exercise follows the outline given in Example 10.2.3, providing sufficient information to recover the plaintext without the use of a computer. Readers
with access to software tools may wish to develop a more realistic exercise.

The following intercepted ciphertext is known to have been produced by a Vigenere cipher:
offset
0
50
100
150
200
250
300
350
400

ciphertext
TUIRD SFOGK YLBVL OORXX RVDPL SHRSB POCBT TLQPG AOMHM SVONM
HDHDN TRTCX RYCJL NHGHT BRIIM HHQWB NHGTI ERDAX BHWCZ IQGEB
RHRQR TKSGX VRZJM IRBXG BXQWT RHGIU ELXXG GDIIA OUWIB EVAPR
DHQXW EWCTQ THBSF AUHXT LOOLN NWWAM HHOHB AQUPF EVGRA EGIAX
DICGL ESHTF BHFGX MRJXG GPOGM IDZAT WZCJE DESPL IJBPE AGWEE
OPOIL ALREX OSZTP OXZSM ANSXM TRATT NWVPM WKOIA ASDTG EGPTY
OUSRH UORHM AUHJI AJOXG IQOHM AWSRH UQQXE MHSIB NJCCP EGBTL
DDMBK LLRTY EQRTW BHWYB NJGJL ERTBB LLHPK YICGV EWCRK AFYSH
WQCCM HHGIN DHBIF OYSBX NWHWX CUIHA IQUDY TKSRH UQHTK RHJDE

8Goldberg and Wagner [39] discovered that random number generation in early versions of the
Netscape web browser was significantly more predictable than advertised, compromising security of
transactions. The episode was rather embarrassing for Netscape, which had refused to allow public
scrutiny of its algorithms.

10 Classical Cryptography

238

Repeated fragments of 'MHH' are located at positions 74, 179, and 404 (other
choices are possible, including the longer fragment 'SRHU').
(a) If e is the length of the (unknown) key, then we may suspect that e I 17974 = 3. 5 · 7 and e I 404- 179 = 32 52 . If these repeated fragments of ciphertext align with identical portions of plaintext, show that € E {1, 3, 5, 15}.
(b) Suppose the candidate e = 15 is considered unlikely. The relative frequencies (from 830 total ciphertext letters) for the remaining candidates, along
with corresponding ciphertext letters, are given by:

I £ I offsets I
I

0, 1, ...

3

0,3, ...
1,4, ...
2,5, ...

5

0,5, ...
1,6, ...
2, 7, ...
3,8, ...
4,9, ...

frequencies of letters in submessage
60 53 45 424040 39 39 39 39 38 37 33 32 30 2727 26 25 25 2120 17 13 13 10
H T R E 0 WB D G I S A X L Q c p u MN F V K J y z
2219 18 18161413 13 13 13 13 1111 9 9 9 8 8 8 8 6 5 5 4 3 I
H E B R T 0 A G I S WC U D Q X F L N p MK V Z J y
22 17 16 15 15 14 14 13 13 11 11 10 10 10 9 9 9 9 8 8 8 7 6 6 4 3
H T G D W0 X A E Q R I K S B C N V L p u MJ y F z
20 1616 161615 15 12 1212 12 111110 1010 10 9 8 7 7 6 6 4 3 2
T H I L R D S B M 0 WA p E G Q X F N C u v y J z K
24 15 15 13 12 12 1111 7 7 6 5 5 4 4 3 3 3 3 2 1 0 0 0 0 0
E A N T 0 R I S B D u H L c G M p wy v F J K Q X z
22 15 14 14 13 12 12 10 8 8 6 6 6 5 4 3 2 2 1 1 I 1 0 0 0 0
H Q D U WL R 0 K V F G J p X s I y B E N z A c M T
18 15 13 13 12 11 9 9 9 8 7 7 6 5 5 4 3 3 2 2 2 2 1 0 0 0
S 0 H WQ C G I R B A F z D v M T u J p X y K E L N
24 14 13 12 12 11 11 10 9 8 7 7 6 6 5 2 2 2 2 2 I 0 0 0 0 0
T P I C X D H G A E R wB s J L N Q v y u F K M 0 z
18 17 15 13 13 10 9 9 8 7 7 6 5 4 4 4 3 3 3 3 2 2 1 0 0 0
M B X L T G E H K F N A R I WY 0 p v z D U Q c J s

Explain why this summary suggests that e = 5 is more likely than the others,
under the assumption that the original message has single-letter frequencies
corresponding roughly to Figure 10.2.
(c) Find the corresponding key letter candidates for several of the popular ciphertext letters in the case e = 5, wh~re the mapping is from the distinguished plaintext letter 'e'.
(d) It is suspected that the correspondents choose keywords from a dictionary.
Find the keyword, and decrypt at least part of the ciphertext. It may be
helpful to note that some of the submessages contain a distinguished (highfrequency) ciphertext letter.

10.2.8 Data compression removes some redundancy in the data. If compression
is to be used in conjunction with encryption, does it make more sense to compress
and then encrypt, or vice versa?

10.3

Feistel ciphers and DES

This section examines in brief a class of ciphers which includes DES, the bestknown symmetric-key cipher. We're mainly interested in the "structural" features

10.3 FeisteJ ciphers and DES

239

and vulnerabilities of these ciphers; readers interested in the details of DES should
consult the references listed in Section 10.4.
Two ciphers are considered, the New Data Seal (NDS) and the Data Encryption Standard (DES), each built on a Feistel cipher. Feistel ciphers are block
ciphers, although a larger framework (such as DES) is commonly used in modes
in which the overall scheme is a stream cipher. Among typical design criteria
for block ciphers, the most fundamental is the requirement that the keyspace be
sufficiently large so that exhaustive search is infeasible. Similarly, the blocksize
should be large enough to frustrate the building of a significant database from observed ciphertext. The goal of confusion is to complicate the relationship between
key and ciphertext, and diffusion attempts to spread the structure present in the
plaintext so that a ciphertext bit depends on many plaintext bits. Efficiency in
terms of speed, memory, and other computer resources may be essential, and the
scheme may need to be suitable for hardware implementation.
A Feistel cipher maps messages of length 2n bits to ciphertext of the same
length, where n EN. The input to the scheme is the message itself and a key k.
Encryption proceeds through a series of r rounds.
• A key-scheduling algorithm determines sub keys
k1, ... , kr from a specified key k. Each sub key determines a function

mo

r.

!k; : {0, l}n ---+ {0, 1

• A 2n-bit message m is split into n-bit left and
right halves, written m = (mo, m1). The rounds
can be written
1: (mo,m1)~--+(m1,m2 = mo EB /k 1 (m1))
2: (m 1, m2)~--+ (m2, m3 = m 1 EB /kz(m2))
____ ..._
--

The output interchanges the left and right halves
of the last round, giving c = (mr+l,mr).

'

mr

I

c=mr+tmr

'

mr+l

• The interchange allows decryption to use the
FeisteJ twisted ladder
same form of the r rounds, but with the subkeys
in reverse order. To see this, define Cj = mr+1-j· Then c = (co,CJ) and the
assignment in round r is easily solved for mr-1, giving
C2

= mr-1 = mr+1 EB fkr (m,) = CQ EB fkr (CJ)

which is of the form found in round 1.

10 Classical Cryptography

240

The use of rounds in this fashion is attractive, as it offers the possibility of
using relatively simple (if not elegant) mappings at each stage, reducing resource
requirements. Multiple rounds (r = 16 in DES) add confusion and diffusion. The
key space, of course, must be sufficiently large to prevent exhaustive search under
some specified level of computing ability granted to the adversary.

10.3.1

The New Data Seal

NDS is a relatively simple Feistel cipher, in part because the key schedule consists
of a single key. Unfortunately, this turns out to be an Achilles' heel, leading to a
chosen-plaintext attack considered in this section.
We consider the case that n = 64 (so that messages are 2n = 128 bits in length)
and r = 16 rounds. The key is a mapping k: {0, 1} 8 ~ {0, 1} 8 . The necessary (but
not sufficient) requirement of a keyspace sufficiently large to prevent exhaustive
search is clearly satisfied, as the number of such keys is (2 8) 28 = 2204 8.
The system contains two fixed (non-secret) maps, So, S1 : {0, 1} 4 ~ {0, 1} 4 ,
and the key schedule consists of the single key k, used on each round. To compute
fk (m;) for a 64-bit half-message m;:
1. Break m; into eight 8-bit bytes, and let m7 denote the byte formed from the
first bit of each byte of m;.
2. Break each byte of m; into two 4-bit nibbles, applying So to the left half and
S1 to the right half.
3. Interchange the nibbles of the jth byte of the SoS1-output in the case that
the jth bit of k(m7) is 1.
4. A fixed (non-secret) permutation is applied to the 64 bits of output.
A schematic for the round appears in Figure 10.3. The final permutation prevents
a reduction to eight independent smaller schemes.

Chosen-plaintext attack on NDS
The use of the same key in every round of NDS is a weakness which can be
exploited iil a chosen-plaintext attack. The key itself can be recovered, completely
breaking the scheme.
Let T = h be the transformation corresponding to a single round of NDS,
i.e., T(m;-J,m;) = Tk(m;-J,m;) = (m;,m;-J ffi/k(m;)), and let F = T 16 denote
all 16 rounds. The essential observation is that F and T commute: FT(m) =
T 16T(m) = TT 16 (m) = T F(m).
Under Kerckhoffs' principle, we assume that the adversary knows everything
about the scheme except the key k. Fix q E {0, 1} 8 . The key is recovered if we can
find k (q) for each such q. The attack proceeds as follows.

10.3 Feistel ciphers and DES

241

m*

i

1IF1
k

II

011

I Sj I
I I I I
o'~ o I
So

~:::

I

p ermutation

Figure 10.3: The function f in one round of NDS.
1. Embed q in a message m = (mo, m 1) so that m j = q. The adversary obtains
the ciphertext (m16, mn) = F(m) corresponding to the chosen-text m.

2. Let k denote one of the 2 8 8-bit possibilities for k(q), and let f = T;/m) be
the mapping of the message by one round under the candidate k.
3. If k = k(q), then f = T(m) and
F(i') = FT(m) = T F(m) = T(m16,m17) = (mn, ?)

and hence the left-half of F(T) agrees with the right-half of F(m). The
adversary obtains ciphertext F(i') corresponding to the chosen-text T; if
the right half of F(m) agrees with the left half of F(i'), then f is considered
a match for T(m), and k is accepted as a likely value of k(q). At most
2 8 = 256 values of k must be checked in order to obtain a match.
The procedure is applied for each q E {0, 1} 8 , yielding a candidate for k using at
most 28 (2 8 + 1) = 65792 chosen-plaintexts.
The candidate for the key k may be incorrect. In step 3, f could be a "match"
without actually having the value T(m). However, if the cipher was designed to
add confusion and diffusion, then we may assume that it is unlikely that more than
one choice of k leads to a match at this stage.
We'd also like k = k(q) whenever f = T(m) at step 3. The only unknown
part of the single-round calculation T(m) is the interchange condition k(mn on
the output of the So, S1 transformations. If the output of So and S1 agree on one
of the bytes of m1, then the corresponding bit of k(mj) cannot be determined
from an examination of T (m). Hence, in addition to the requirement that m j = q,
m should be chosen so that output of So and S1 differ in each byte of m 1. (The
inability to choose such an m may indicate a system vulnerable to simpler attacks.)

10 Classical Cryptography

242

Toy example

Consider an NDS-like cipher with n = 4 and r = 3 rounds. Messages are 2n =
8 bits, and the key is a mapping k : {0, 1} 2 --+ {0, 1} 2 . (Each of the 3 subkeys
is k. The keyspace has size (2 2 ) 22 = 256.) Let So be the identity, and let S1
perform bitwise complement. The permutation writes the bits in reverse order. A
schematic similar to Figure 10.3 has two pairs of So, S1 boxes, each box accepting
1-bit of then= 4 bits of m;.
Suppose the key k is defined by
k(OO) = lO

k(IO) = 11

k(Ol) = 00

k(l1) = 10

and the message to be encrypted is m = (mo, m 1) = (0111, 1100). The value of
m2 = mo EEl j(m1) is calculated as
SoS1

k

permute

EBmo

m 1 = 1100--+ 1001 --+OliO --+ 0110--+ 0001 = m2.

The other rounds are similar, giving
(mo, m1) = (0111, 1100) ~---+ (1100, 0001) ~---+ (0001, 1101)
~---+ (1101,0011) =

(m3,m4) = F(m).

We illustrate the chosen-plaintext attack to recover k(q) for q = 10.
1. We wish to choose m = (mo, m 1) so that m7 = q and so that the output of So
and S1 on halves of m 1 differ. We may select m = (0 111, 11 00), obtaining
F(m) = (1101,0011).
2. Build a table of possible one-round encryptions f, one entry for each guess
k of k(q). Then F(f) corresponding to each chosen-plaintext f is obtained.

k
ml

SoSI
apply k
permute
EBmo
T

F(T)

00
llOO
1001
1001
1001
1110
(1100, ll10)
(0000, 1011)

01
llOO
1001
1010
0101
0010
(1100, 0010)
(1100, 0100)

10
1100
1001
0101
1010
1101
(1100, 1101)
(0011, 1000)

11
llOO
1001
OllO
0110
0001
(1100, 0001)
(0011, 1011)

3. f is considered a match whenever the left-half of F(f) agrees with the
right-half of F(m) = (1101, 0011). Unfortunately, two matches have been
obtained, corresponding to underlined entries in the table. At this stage, the
possibilities for k(q) have been narrowed to k = 10 or k = 11.
The example illustrates that the proposed attack may fail to isolate the key value.
Additional plaintexts could be tried; e.g., m = (0101, 1100) determines k = 11 as
the correct value of k(IO).

10.3 Feistel ciphers and DES

243

Exercises
10.3.1 Given that c = (mr+ 1, mr) = ( 1111, 0100) is an output of the cipher in the
toy example, find the corresponding message m.
10.3.2 Follow the procedure outlined in the toy NDS example to find k(OO).
10.3.3 Define two functions fo, f 1 : {0, 1} 4 ~ {0, 1} 4 by
fo(XJ,X2,X3,X4) = (x2 E13x4, l,XJX2, 1 EBx3)
/J(XJ,X2,X3,X4) = (1,x, EBx3,X4,x2).

Let F be the toy Feistel cipher defined as follows:
• n = 4 (so that messages are 2n = 8 bits long), and there are r = 2 rounds;
• the key of F is a pair (k,, k2) of binary digits;
• in round i E { 1, 2}, the function A is used.

A message m is encrypted to ciphertext c under the key (0, 1). If c = mr+lmr =
10101011, then find m.

10.3.2 The Data Encryption Standard
DES is the result of a 1973 solicitation for encryption standards by the National
Bureau of Standards (NBS, now NIST). Developed at IBM and modified jointly
with the US National Security Agency (NSA), the cipher is also known as the
Data Encryption Algorithm. DES is based on a 16-round Feistel cipher, acting
on 64-bit input. The key schedule produces a 48-bit subkey k; for each of the
rounds from a given 56-bit key k. There are eight fixed S-box mappings which are
fundamental to the security ofthe system, shown in Figure 10.4.
The adoption of DES as a standard in 1977 was a milestone in the use of
cryptography. DES would become the most widely-used symmetric-key scheme.
The standard was controversial, in part because not all of the design principles
were made public. The secrecy invited speculation that the scheme may contain a
"trap door" which permits the NSA to decrypt messages.
The size of the keyspace has been criticized heavily. The description of the
scheme would be known to everybody; security rested entirely with the key. The
standard specifies a keylength of 64 bits, of which 8 are parity, giving an effective key length of 56 bits. An exhaustive search of the keyspace by a determined
adversary with specialized hardware was a concern.
In 1977, Diffie and Hellman [28] proposed a DES exhaustive search machine
at an estimated cost of US$20 million, searching the entire keyspace in a day.
In 1993, Michael Wiener presented a detailed design for a machine which could
search the entire key space in 7 hours for $1 million [93]; a 1997 version of the $1
million machine would perform the search in a little over an hour [94].

244

10 Classical Cryptography

64-bit input
8x6 bits

S-boxes

output

(a) DES structure

(b) Feistel inner function

Figure 10.4: Schematics for the Data Encryption Standard.

In January 1997, RSA Laboratories launched the "DES Challenge," offering
prizes for uncovering the DES key from three known-plaintext pairs. The DESCHALL group claimed the prize in June 1997, using exhaustive search with a
large-scale distributed network of computers connected by the internet. This was
a volunteer effort, requiring 96 days with an estimated 70000 machines involved
at some stage. Approximately 25% of the keyspace had been searched when the
key was found. In 1998, a similar contest was solved in 40 days by distributed.net,
searching 88% of the key space. Near the close of the contest, they recorded a peak
rate of over 34 · 109 keys per second by nearly 1400 active teams.
In 1998, the Electronic Frontier Foundation (EFF) claimed a DES Challenge
in 56 hours using a $0.2 million custom-built machine. In January 1999, distributed.net and the EFF combined forces, claiming a Challenge in a little over
22 hours, testing 245 · 109 keys per second when the key was found. There had
been concern that US Government officials deliberately overestimated the costs of
such a machine and underestimated the applicability of known-plaintext attacks,
in order to protect other interests. As Diffie noted, by publishing the complete
software and hardware details for their relatively inexpensive cracker [34], it is no
longer "a question of whether DES keys can be extracted by exhaustive search; it
is a question of how cheaply they can be extracted and for what purposes." The
DES Challenge permitted known-plaintext attacks, but effective exhaustive search
is possible if partial information concerning structure or content-type of the plaintext is known.
Multiple encryption (discussed below) can be used with DES to defeat exhaustive search, although there is a speed penalty (see also Exercise 10.3.5). DES

10.3 Feistel ciphers and DES

245

is entrenched, however. As Whitfield Diffie remarked in the forward to the EFF
cracker [34], "The most convincing argument that DES is insecure would not outweigh the vast investment in DES equipment that has accumulated throughout the
world. People will continue using DES whatever its shortcomings, convincing
themselves that it is adequate for their needs."
A possible DES replacement, the Advanced Encryption Standard (AES; see
Section 10.4) was in the process of selecting an algorithm in 2000. Apparently, it
will address the fundamental weakness of DES, the ability to effectively mount an
exhaustive search of the keyspace. In [9], a group of well-known cryptographers
recommended a 75-bit key to protect 1996 data and a 90-bit key for 20-year security, noting that "the cost of very strong encryption is not significantly greater
than that of weak encryption."
Multiple encryption
Block ciphers such as DES may be applied in succession, in an attempt to obtain a cipher with a larger keyspace. Double encryption with a single cipher, for
example, takes the form E (m) = Ek2 Ek 1 (m) for given keys k1, k2. Multiple encryption need not enhance security-in fact, it need not even enlarge the effective
keyspace. If an encryption scheme is closed under composition in the sense that
given k1, k2 there exists k3 E '1( so that Ek 2 Ek 1 = Ek3 , then double encryption offers no additional security. As an example, the simple substitution cipher over all
permutations k of some alphabet (as in Example 10.2.2) has k3 = k2 o k1.
For key k, DESk is a permutation on {0, 1} 64 . The keyspace defines at most
256 (out of a possible 264 !) such permutations. This collection of permutations is
not closed under composition, and multiple encryption is often used with the hope
of defeating exhaustive search of the key space.
In the case of double-DES, an unsophisticated exhaustive search tries all (2 56 ) 2
= 2 112 keys. However, double encryption is vulnerable to a meet-in-the-middle attack which reduces the effort to 257 DES operations at the cost of 256 storage. If
at least one known-plaintext pair (m, c) is available, where c = DESk2 DESk 1 (m)
for unknown keys k1, k2, then the attack to recover the keys is:
1. Form a table of (i, DES; (m)) for all keys i, sorted by the second entry.

2. For each possible key j, determine if DESj 1 (c) appears in the table; if
so, then DESj 1 (c) = DES;(m) for some i, and hence c = DESjDES;(m)
and (i, j) is a candidate for (k1, k2). Additional known-plaintext pairs (if
available) are used at this stage to discard spurious matches.
If at least two known-plaintext pairs are available, the attack is expected to determine the keys.
In practice, triple-DES is used, having a keyspace of size (2 56 ) 3 = 2 168 . Given
keys k1, k2, k3, the encryption is E(m) = Ek 3 Ek 2 Ek 1 (m), where Ek denotes DESk

246

10 Classical Cryptography

or DESi: 1 . The two-key triple-DES variant uses Ek2 =DESk; 1 and k3 = k1; choosing k2 = k1 gives DES. The meet-in-the-middle attack on triple-DES is no longer
feasible, requiring on the order of 2 112 DES operations. However, there are attacks on two-key triple-DES (requiring large numbers of either chosen-plaintext
pairs or DES operations) showing it to be weaker than triple-DES [63].

Modes of operation
Block ciphers are often used in modes which process the plaintext in fragments
(often the size of a block). Two such modes are considered in this section. DES
will be used in the examples, but the ideas apply to any block cipher.
Let m = m1m2 · · · be a message, where each m; is a block (64 bits in DES).
Electronic codebook (ECB) mode applies DES to each block, obtaining c; =
DESk(m;). It has the advantage of simplicity, and bit errors in a ciphertext block
affect decryption of that block only. A disadvantage is that identical messages
(under the same key) map to the same ciphertext, possibly leaking information to
an adversary.
mz
Cipher-block chaining (CBC) chooses an initial
block co and encrypts as c; = DESk(m; EBc;-1) fori :=::
1. Decryption is performed by m; = DESi: 1 (c;) EB Ci-1·
The first block of plaintext can be chosen at random to
k
prevent identical plaintexts from always giving the same
output. Unlike ECB mode, there are chaining dependencies causing CJ to depend on CJ-1 (which depends
Cipher-block chaining
on all preceding plaintext blocks).
In ECB mode, errors introduced (outside of the encryption process) into ciphertext c affect only the decryption of the damaged blocks. The chaining in CBC
propagates an error in c1, affecting the recovery of both m 1 and m J+I· In fact,
an adversary with the ability to modify cJ can force bit errors into the recovery of
m J+l by changing the corresponding bits of c 1 (Exercise 10.3.4).

Applications to authentication
Symmetric-key systems can be used to provide authentication. We consider two
specific examples, the first employing cipher-block chaining to provide message
authentication, and the second providing identification (entity authentication) using a password scheme built from DES. The difference between entity and message authentication can be subtle, although the latter tends to involve an actual
message rather than simply a claim of identity.
Message authentication Suppose Bob and Alice are exchanging messages (perhaps by email) over an unsecured channel. On receipt of a message purported

10.3 Feistel ciphers and DES

247

to be from Alice, Bob should be able to convince himself that the message is
genuine. At least two kinds of attacks are of concern: impersonation and forgery.
One possibility is to use a symmetric-key scheme such as DES. Alice and
Bob share a secret key, and Alice encrypts the message m, sending c = Ek(m).
Bob decrypts c with the secret key, accepting the message if it "makes sense." To
prevent replay attacks-messages resent by an adversary--:-Alice could add time
or sequence information to m before calculating c. If the cipher is such that it is
difficult to create meaningful messages without knowledge of the key, then some
protection from forgery is provided; however, some ciphers may allow selective
changes to go undetected. For example, if a block cipher is applied in ECB mode,
then the adversary could re-order, substitute, or delete ciphertext blocks. The onetime pad allows an adversary to force bit changes by changing the corresponding
bits in the ciphertext.
A more robust scheme can be developed using cipher-block chaining. A block
cipher E is selected and the message is written m = m1 · · ·m 1 where each mi has
bitlength matching the blocksize of the cipher (if E =DES, then the blocks are of
length 64 bits). The block cipher is applied in CBC mode, giving Ci = Ek(mi EEl
Ci-I), 1 ::; i ::; t. Alice sends the message m along with c1 , known as a message
authentication code (MAC). Bob computes the CBC-MAC (as Alice did), and
accepts the message as genuine if this value agrees with the received MAC. To
defeat exhaustive search of the key space, a second key k' may be selected and twokey triple-encryption performed on the last block (delivering EkE;,\c1 ) rather
than c1 as the MAC).
Identification Conventional password schemes rely on a secret shared between
a user and the system; access to a system resource is granted when the user reveals
the secret itself. We consider the traditional password mechanism used on many
Unix computer systems: to gain access, a userid and password pair are presented
to the system; access is granted if the system determines from its own stored
information that the password corroborates the identity of the user.
A password of up to 8 characters is used in forming the key k for a modified DES encryption function. Each character provides 7 bits of the 56-bit key;
zero bits are added if the password is less than 8 characters. An additional 12
bits (known as the salt) obtained from the system clock at the time of password
creation is used to modify the expansion E in Figure 10.4, specifying one of
2 12 = 4096 variations. The system calculates mi = DESk(mi-I), 1 :S i :S 25,
where DES* denotes the modified DES scheme and mo is the 64-bit zero value.
The 12-bit salt and 64-bit m2s (known as the hashed password) are stored on the
system, traditionally in /etc/passwd (a file). When subsequently presented with a
userid and password, the system performs the same calculations, granting access
if the result agrees with the stored value.
The calculation is known as the Unix crypt password algorithm. An adversary

10 Classical Cryptography

248

in possession of an entry from /etc/passwd can mount a known-plaintext attack,
since the message input to the algorithm is known. Even if exhaustive search is infeasible, an attack using a dictionary of passwords which are known to be favored
by users may be successful. The salt makes the dictionary attack more difficult,
since 4096 variations of each password are possible. The salting procedure was
also designed so that off-the-shelf DES hardware could not be used for password
cracking.
Adversaries often obtain the /etc/passwd file itself (on many systems, all users
could read this file), and perform various kinds of attacks, perhaps in the privacy
of their own chambers. Recovery of some passwords is a significant threat, even
by adversaries with relatively modest computing power. 9 Systems that rely on
this mechanism have countered by forcing users to choose better passwords (in an
attempt to defeat dictionary attacks), and have moved the hashed passwords to a
separate file which requires special privileges to read.
The password scheme is an example of weak authentication. The user receives
no explicit corroboration of the identity of the system; if the channel is unsecured,
then an adversary may be able to impersonate the system in addition to simple
eavesdropping. We will return to authentication in Chapter 12.

Exercises
10.3.4 (Errors in DES ciphertexts) Suppose that t plaintext blocks m 1, ... , m 1 are
encrypted using DES producing ciphertexts CJ, ... , c1 , respectively.
(a) Suppose that one ciphertext, say ci, is transmitted incorrectly. Explain
briefly the procedure to determine the number and location of blocks which
will be decrypted incorrectly in ECB mode and CBC mode.
(b) Suppose CBC mode has been used and an adversary interchanges the ciphertext blocks c3 and C6. When the ciphertext is received, how many plaintext
blocks will be in error?
(c) In CBC mode, show how an adversary forces specific bit errors in m J+l by
adjusting ci.
10.3.5 This problem concerns proposals to protect DES from exhaustive key
search. The key is k = (k1,k2) where k1 E {0, 1} 56 and k2 E {0, 1} 64 . Let
m E {0, 1}64 be the plaintext message.
(a) Show that encryption defined by Ek(m) = DESk 1 (m) ffik2 does not increase
the work needed to break the encryption scheme using exhaustive search.
That is, show how to break this scheme using on the order of 256 DES
operations. You may assume that you have a moderate number of pairs
(mi,Ci = Ek(mi)).
9Feldmeier and Karn [32] report that their 1989 dictionary attacks typically recovered over 30%
of the passwords on a given system. The authors present a fast crypt algorithm, used in passwordcracking attacks where the hashed passwords have been obtained.

10.4 Notes

249

(b) Does encryption defined by Ek(m) = DESk 1 (m EB k2) make exhaustive
search less feasible?
A "DESX" extension suggested by Rivest is discussed in the proceedings of
CRYPTO '96 [49]. For key k = (k1, k2, k3), the encryption function is Ek(m) =
k3 EB DESk 1 (m EB k2). The inexpensive XOR pre- and post-processing may be used
with existing DES machinery.

10.3.6 Suppose that Eve has obtained three pairs (m1,q), (m 2,c2), and
(m3, c3) which were encrypted by Alice using triple-DES with E(m) =
DESk3 DESk2 DESk 1 (m). Devise a meet-in-the-middle attack that recovers Alice's
key (k1, k2, k3) in roughly 2 112 operations.
10.3.7 (DES complementation property) Let m denote the bitwise complement
of a bit string m. If c = DESk(m), then an examination of the description of
DES establishes that c = DESr(m). Does it appear that this property can be used
to improve the running time of exhaustive key search under a known-plaintext
attack? Under a chosen-plaintext attack?
10.3.8 The CBC-MAC described provides data integrity but not confidentiality.
Consider the following proposal to add secrecy. The MAC is appended to message
m = m1 · · ·m 1 , giving m' = mmt+1· Then m' itself is encrypted in CBC mode
(using the same initial co and key), giving C! ·· ·Ct+J, where Ci = Ek(mi EBci-d,
1 ::::; i ::::; t + 1; the first t of these calculations are identical to those performed in
obtaining the MAC.
The method appears attractive, obtaining the ciphertext directly from the calculations for the MAC. Show that the scheme results in a final ciphertext block
Ct+1 = Ek(mt+l EB c 1 ) which is independent of the plaintext and the ciphertext.
Explain why this means that the added encryption has compromised the authentication, and illustrate how an adversary may exploit this fundamental flaw.

10.4

Notes

The opening sentence of this chapter is from Rivest [71], an excellent introduction to cryptography. Simmons [81] contains contributions from several authors,
including The First Ten Years of Public Key Cryptography by Diffie [26]. The
Handbook ofApplied Cryptography by Menezes, van Oorshot, and Vanstone [63]
is recommended for its well-organized and comprehensive coverage.
Kahn's The Codebreakers is often cited for its complete (and lively) nontechnical history of cryptology up to 1967; a few notes on more recent developments
appear in a revised 1996 edition [48]. Garfinkel [37] discusses some of the political, legal, privacy, and policy questions concerning cryptography, along with the
history of the Pretty Good Privacy (PGP) application. The textbooks of Stinson
[86] and Koblitz [50] are natural choices to extend the material introduced here.

250

10 Classical Cryptography

Extensive material from the VENONA project at the National Security Agency
is available via http://www.nsa.gov. The historical introduction begins with·the
following paragraph.
On 1 February 1943, the U.S. Army's Signal Intelligence Service, a forerunner of the National Security Agency, began a small, very secret program, later
codenamed VENONA. The object of the VENONA program was to examine
and possibly exploit, encrypted Soviet diplomatic communications. These
messages had been accumulated by the Signal Intelligence Service (later renamed the U.S. Army Signal Security Agency and commonly called "Arlington Hall" after the Virginia location of its headquarters) since 1939 but had
not been studied previously. Miss Gene Grabeel, a young Signal Intelligence
Service employee, who had been a school teacher only weeks earlier, started
the project.
The description of the New Data Seal is from Beker and Piper [3]. Details of
DES may be found in [63, 86, 3]. Early commentary on the "DES controversy"
appears in a series of IEEE Spectrum articles beginning with [87]. Some design
criteria for the S-boxes may be found in Coppersmith [23], Stinson [86], and
Schneier [76]. RSA's DES Challenges are described on http://www.rsa.com. The
non-profit Electronic Frontier Foundation maintains a web page on various civil
liberties issues at http://www.eff.org; on-line versions of Cracking DES [34] are
available.
Use of DES CBC has been standardized by the International Organization for
Standards (ISO 9797) and the American National Standards Institute (ANSI X9.9)
for authentication purposes [63]. ANSI X9.9 is widely used by banks in financial
applications. Triple-DES was approved by ANSI in November 1998 as a standard (ANSI X9.52). In 1999, the National Institute of Standards and Technology
(NIST) was in the process of considering triple-DES for a US Government Federal
Information Processing Standard (FIPS 46-3; see http://csrc.nist.gov/cryptval).
The draft announcement contained:
Additionally, knowing that the DES' security life was nearing an end, NIST
has been working with industry and the cryptographic community to develop
an Advanced Encryption Standard (AES) for the 21st century. On January
2, 1997, NIST announced the initiation of an effort to develop the AES (62
FR 93). It is intended that the AES will specify an unclassified, publicly
disclosed encryption algorithm capable of protecting sensitive government
information well into the next century. Unfortunately, since it takes a substantial amount of time to gain confidence in a new encryption algorithm, the
AES is not expected to be a fully developed PIPS for some time to come.
Information on NIST's multi-year effort to develop the AES can be obtained
at http://www.nist.gov/aes.
Recently claims have been made of a special-purpose hardware based attack
on the DES. In light of this most recent attack, NIST can no longer support

10.4 Notes
the use of the DES for many applications. As with other security tools, encryption must balance cost against risk. The recent brute force exhaustion
attack by a "cracking machine" costing $250000 took 56 hours to crack a
single message. With this special-purpose technology, the average time of
cracking per message would be twice that, since only a quarter of all keys
were tested. In some cases this kind of attack may not pose an immediate
or significant threat-for example where short-term protection of perishable
information is desired. However, advances in technology are likely to further
reduce the average cracking time. Therefore, NIST recommends the following:
• For existing systems, develop a prudent transition strategy to move to
Triple DES. This strategy should match the strength of the protective
measures against the associated risk. Critical systems should receive
priority.
• When building new systems, use Triple DES to protect sensitive, unclassified data.
These recommendations are reflected in the proposed draft FIPS 46-3 ... by
recognizing Triple DES, as described in ANSI X9.52, as a FIPS approved
algorithm.

251

This page intentionally left blank

