Tools
=====

SSL & TLS
---------

### Server checks via the web

  * [ssllabs.com](https://ssllabs.com) offers a great way to check your
webserver for misconfigurations. See <https://www.ssllabs.com/ssltest/>.
Furthermore, ssllabs.com has a good best practices tutorial, which
focuses on avoiding the most common mistakes in SSL.

  * SSL Server certificate installation issues
<https://www.sslshopper.com/ssl-checker.html>

  * Check SPDY protocol support and basic TLS setup <http://spdycheck.org/>

  * XMPP/Jabber Server check (Client-to-Server and Server-to-Server)
<https://xmpp.net/>

  * Luxsci SMTP TLS Checker <https://luxsci.com/extranet/tlschecker.html>

  * Does your mail server support StartTLS? <https://starttls.info/>

  * <http://checktls.com> is a tool for testing arbitrary TLS services.

  * TLS and SSH key check <https://factorable.net/keycheck.html>

  * <http://tls.secg.org> is a tool for testing interoperability of HTTPS
implementations for ECC cipher suites.

  * <http://www.whynopadlock.com/> Testing for mixed SSL parts loaded via
http that can totally lever your HTTPS.

  * Browser checks

  * Check your browser’s SSL capabilities:
<https://cc.dcsec.uni-hannover.de/> and
<https://www.ssllabs.com/ssltest/viewMyClient.html>.

  * Check Browsers SSL/TLS support and vulnerability to attacks:
<https://www.howsmyssl.com>

### Command line tools

  * <https://sourceforge.net/projects/sslscan> connects to a given SSL
service and shows the cipher suites that are offered.

  * <http://www.bolet.org/TestSSLServer/> tests for BEAST and CRIME
vulnerabilities.

  * <https://github.com/drwetter/testssl.sh> checks a server’s service on
any port for the support of TLS/SSL ciphers, protocols as well as some
cryptographic flaws (CRIME, BREACH, CCS, Heartbleed).

  * <https://github.com/iSECPartners/sslyze> Fast and full-featured SSL
scanner

  * <https://github.com/jvehent/cipherscan> Fast TLS scanner (ciphers,
order, protocols, key size and more)

  * <http://nmap.org/> nmap security scanner

  * <http://www.openssl.net> OpenSSL s\_client

Key length
----------

  * <http://www.keylength.com> comprehensive online resource for comparison
of key lengths according to common recommendations and standards in
cryptography.

RNGs
----

  * [ENT](http://www.fourmilab.ch/random/) is a pseudo random number
generator sequence tester.

  * [HaveGE](http://www.issihosts.com/haveged/) is a tool which increases
the Entropy of the Linux random number generator devices. It is based on
the HAVEGE algorithm. <http://dl.acm.org/citation.cfm?id=945516>

  * [Dieharder](http://www.phy.duke.edu/~rgb/General/dieharder.php) a random
number generator testing tool.

  * [CAcert Random](http://www.cacert.at/random/) another random number
generator testing service.

Guides
------

  * See:
<https://www.ssllabs.com/downloads/SSL_TLS_Deployment_Best_Practices.pdf>.
