Abstract
========

> Unfortunately, the computer security and cryptology communities have drifted apart over the last 25 years. Security people don't always understand the available crypto tools, and crypto people don't always understand the real-world problems.

Ross Anderson in [-@anderson2008security]

This guide arose out of the need for system administrators to have an
updated, solid, well researched and thought-through guide for configuring SSL,
PGP, SSH and other cryptographic tools in the post-Snowden age. Triggered by the NSA
leaks in the summer of 2013, many system administrators and IT security
officers saw the need to strengthen their encryption settings.
This guide is specifically written for these system administrators.


As Schneier noted in [-@Sch13], it seems that intelligence agencies and
adversaries on the Internet are not breaking so much the mathematics of
encryption per se, but rather use software and hardware weaknesses, subvert
standardization processes, plant backdoors, rig random number generators and
most of all exploit careless settings in server configurations and encryption
systems to listen in on private communications. Worst of all, most
communication on the internet is not encrypted at all by default (for SMTP,
opportunistic TLS would be a solution).


This guide can only address one aspect of securing our information systems:
getting the crypto settings right to the best of the authors' current
knowledge. Other attacks, as the above mentioned, require different protection
schemes which are not covered in this guide. This guide is not an introduction
to cryptography. For background information on cryptography and cryptoanalysis
we would like to refer the reader to the references in appendix
[Links](#links) and [Suggested Reading](#suggested-reading) at the end of this
document.


The focus of this guide is merely to give current *best practices for
configuring complex cipher suites* and related parameters in a *copy &
paste-able manner*. The guide tries to stay as concise as is possible for such
a complex topic as cryptography. Naturally, it can not be complete. 
There are many excellent guides (@ii2011ecrypt @TR02102 @ENISA2013) and best
practice documents available when it comes to cryptography. However none of
them focuses specifically on what an average system administrator needs for
hardening his or her systems' crypto settings.

This guide tries to fill this gap.


The guide was produced in an open source manner: every step of editing can be
traced back to a specific author via our version control system.
