Introduction
============

Audience
--------

Sysadmins. Sysadmins. Sysadmins. They are a force-multiplier.

Related publications
--------------------

Ecrypt II @ii2011ecrypt, ENISA’s report on Algorithms, key sizes and
parameters @ENISA2013 and BSI’s Technische Richtlinie TR-02102 @TR02102
are great publications which are more in depth than this guide. However,
this guide has a different approach: it focuses on *copy & paste-able
settings* for system administrators, effectively breaking down the
complexity in the above mentioned reports to an easy to use format for
the intended target audience.

How to read this guide
----------------------

This guide tries to accommodate two needs: first of all, having a handy
reference on how to configure the most common services’ crypto settings
and second of all, explain a bit of background on cryptography. This
background is essential if the reader wants to chose his or her own
cipher string settings.

System administrators who want to copy & paste recommendations quickly
without spending a lot of time on background reading on cryptography or
cryptanalysis can do so, by simply searching for the corresponding
section in chapter [Practical recommendations](#practical-recommendations).

It is important to know that in this guide the authors arrived at two
recommendations: *Cipher string A* and *Cipher string B*. While the
former is a hardened recommendation the latter is a weaker one but
provides wider compatibility. *Cipher strings A and B* are described in
[Recommended Cipher Suites](#recommended-cipher-suites).

However, for the quick copy & paste approach it is important to know
that this guide assumes users are happy with *Cipher string B*.

While chapter [Practical recommendations](#practical-recommendations) is
intended to serve as a copy & paste reference, chapter [Theory](#theory)
explains the reasoning behind *cipher string B*. In particular, section
[Cipher Suites](#cipher-suites) explains how to choose individual cipher
strings. We advise the reader to actually read this section and
challenge our reasoning in choosing *Cipher string B* and to come up
with a better or localized solution.

= \[ellipse, draw, minimum height=2em, text width=4.5em, text badly
centered, inner sep=0pt\] = \[diamond, draw,aspect=2, text width=10em,
text badly centered, node distance=8em, inner sep=0pt\] = \[rectangle,
draw,inner sep=0pt, text width=17em, text centered, rounded corners,
minimum height=4em\] = \[draw, very thick, -latex’\] =\[near
start,color=black\]

TODO: tikz image

Disclaimer and scope
--------------------

> "A chain is no stronger than its weakest link, and life is after all a chain"

William James
> "Encryption works. Properly implemented strong crypto systems are
> one of the few things that you can rely on. Unfortunately, endpoint security is
> so terrifically weak that NSA can frequently find ways around it."

Edward Snowden, answering questions live on the Guardian's website
[-@snowdenGuardianGreenwald]

This guide specifically does not address physical security, protecting
software and hardware against exploits, basic IT security housekeeping,
information assurance techniques, traffic analysis attacks, issues with
key-roll over and key management, securing client PCs and mobile devices
(theft, loss), proper Operations Security[^1], social engineering
attacks, protection against tempest @Wikipedia:Tempest attack
techniques, thwarting different side-channel attacks (timing–, cache
timing–, differential fault analysis, differential power analysis or
power monitoring attacks), downgrade attacks, jamming the encrypted
channel or other similar attacks which are typically employed to
circumvent strong encryption. The authors can not overstate the
importance of these other techniques. Interested readers are advised to
read about these attacks in detail since they give a lot of insight into
other parts of cryptography engineering which need to be dealt with.[^2]

This guide does not talk much about the well-known insecurities of
trusting a public-key infrastructure (PKI)[^3]. Nor does this text fully
explain how to run your own Certificate Authority (CA).

Most of this zoo of information security issues are addressed in the
very comprehensive book “Security Engineering” by Ross
Anderson @anderson2008security.

For some experts in cryptography this text might seem too informal.
However, we strive to keep the language as non-technical as possible and
fitting for our target audience: system administrators who can
collectively improve the security level for all of their users.

> "Security is a process, not a product."

Bruce Schneier

This guide can only describe what the authors currently *believe* to be
the best settings based on their personal experience and after intensive
cross checking with literature and experts. For a complete list of
people who reviewed this paper, see the . Even though multiple
specialists reviewed the guide, the authors can give *no guarantee
whatsoever* that they made the right recommendations. Keep in mind that
tomorrow there might be new attacks on some ciphers and many of the
recommendations in this guide might turn out to be wrong. Security is a
process.

We therefore recommend that system administrators keep up to date with
recent topics in IT security and cryptography.

In this sense, this guide is very focused on getting the cipher strings
done right even though there is much more to do in order to make a
system more secure. We the authors, need this document as much as the
reader needs it.

### Scope

In this guide, we restricted ourselves to:

* Internet-facing services
* Commonly used services
* Devices which are used in business environments (this specifically
excludes XBoxes, Playstations and similar consumer devices)
* OpenSSL

We explicitly excluded:

* Specialized systems (such as medical devices, most embedded systems,
industrial control systems, etc.)
* Wireless Access Points
* Smart-cards/chip cards

Methods
-------

For writing this guide, we chose to collect the most well researched
facts about cryptography settings and let as many trusted specialists as
possible review those settings. The review process is completely open
and done on a public mailing list. The document is available (read-only)
to the public Internet on the web page and the source code of this
document is on a public git server, mirrored on GitHub.com and open for
public scrutiny. However, write permissions to the document are only
granted to vetted people. The list of reviewers can be found in the
section “”. Every write operation to the document is logged via the
“git” version control system and can thus be traced back to a specific
author. We accept “git pull requests” on the github mirror[^4] for this
paper.

Public peer-review and multiple eyes checking of our guide is the best
strategy we can imagine at the present moment[^5].

We invite the gentle reader to participate in this public review
process.
