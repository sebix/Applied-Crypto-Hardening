A note on Diffie Hellman Key Exchanges
--------------------------------------

A common question is which Diffie Hellman (DH) Parameters should be used
for Diffie Hellman key exchanges[^49]. We follow the recommendations in
ECRYPT II @ii2011ecrypt [chapter 16]

Where configurable, we recommend using the Diffie Hellman groups defined
for IKE, specifically groups 14-18 (2048–8192 bit MODP @rfc3526). These
groups have been checked by many eyes and can be assumed to be secure.

For convenience, we provide these parameters as PEM files on our
webserver[^50].
