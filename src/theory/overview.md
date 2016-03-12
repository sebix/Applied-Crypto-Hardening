Theory
======

Overview
--------

This chapter provides the necessary background information on why
chapter [Practical recommendations](#practical-recommendations) recommended
*cipher string B*.

We start off by explaining the structure of cipher strings in section
[Architecture](#architecture) and define in [Forward Secrecy](#forward-secrecy).
Next we present *Cipher String A* and *Cipher String
B* in section [Recommended Cipher Suites](#recommended-cipher-suites). This
concludes the section on cipher strings. In theory, the reader should now be
able to construct his or her own cipher string. However, the question why
certain settings were chosen still remains. To answer this part, we need to look
at recommended keylengths, problems in specific algorithms and hash
functions and other cryptographic parameters. As mentioned initially in
section [Related Publications](#related-publications), the ENISA @ENISA2013,
ECRYPT2 @ii2011ecrypt and BSI @TR02102 reports go much more into these topics
and should be consulted in addition.

We try to answer the questions by explaining issues with [random number
generators](#random-number-generators), [keylengths](#keylengths), current
issues in [ECC](#a-note-on-elliptic-curve-cryptography), a note of warning on
[SHA-1](#a-note-on-sha-1)and some comments on [Diffie Hellman key
exchanges](#a-note-on-diffie-hellman-key-exchanges). All of this is important in
understanding why certain choices were made for *Cipher String A and B*.
However, for most system administrators, the question of compatibility
is one of the most pressing ones. Having the freedom to be compatible
with any client (even running on outdated operating systems) of course,
reduces the security of our cipher strings. We address these topics in
[Compatibility](#compatibility). All these sections will allow a
system administrator to balance his or her needs for strong encryption
with usability and compatibility.

Last but not least, we finish this chapter by talking about issues in
[Public Key Infrastructures](#public-key-infrastructures),
Certificate Authorities and on hardening a PKI. Note that these last few topics
deserve a book on their own. Hence this guide can only mention a few current
topics in this area.
