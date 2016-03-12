Webservers
----------

### Apache

Note that any cipher suite starting with EECDH can be omitted, if in
doubt. (Compared to the theory section, EECDH in Apache and ECDHE in
OpenSSL are synonyms [^6])

#### Tested with Versions

Apache 2.2.22, Debian Wheezy with OpenSSL 1.0.1e

Apache 2.4.6, Debian Jessie with OpenSSL 1.0.1e

Apache 2.4.10, Debian Jessie 8.2 with OpenSSL 1.0.1k

Apache 2.4.7, Ubuntu 14.04.2 Trusty with Openssl 1.0.1f

#### Settings

Enabled modules *SSL* and *Headers* are required.

#### Additional settings

You might want to redirect everything to *https://* if possible. In
Apache you can do this with the following setting inside of a
VirtualHost environment:

#### References

Apache2 Docs on SSL and TLS: <https://httpd.apache.org/docs/2.4/ssl/>

#### How to test

See appendix [Tools](#tools).

### lighttpd

#### Tested with Versions

lighttpd/1.4.31-4 with OpenSSL 1.0.1e on Debian Wheezy

lighttpd/1.4.33 with OpenSSL 0.9.8o on Debian Squeeze (note that TLSv1.2
does not work in openssl 0.9.8 thus not all ciphers actually work)

lighttpd/1.4.28-2 with OpenSSL 0.9.8o on Debian Squeeze (note that
TLSv1.2 does not work in openssl 0.9.8 thus not all ciphers actually
work)

lighttpd/1.4.31, Ubuntu 14.04.2 Trusty with Openssl 1.0.1f

#### Settings

Starting with lighttpd version 1.4.29 Diffie-Hellman and Elliptic-Curve
Diffie-Hellman key agreement protocols are supported. By default,
elliptic curve “prime256v1” (also “secp256r1”) will be used, if no other
is given. To select special curves, it is possible to set them using the
configuration options `ssl.dh-file` and `ssl.ec-curve`.

Please read section \[section:DH\] for more information on Diffie
Hellman key exchange and elliptic curves.

#### Additional settings

As for any other webserver, you might want to automatically redirect
*http://* traffic toward *https://*. It is also recommended to set the
environment variable *HTTPS*, so the PHP applications run by the
webserver can easily detect that HTTPS is in use.

#### Additional information

The config option *honor-cipher-order* is available since 1.4.30, the
supported ciphers depend on the used OpenSSL-version (at runtime). ECDHE
has to be available in OpenSSL at compile-time, which should be default.
SSL compression should by deactivated by default at compile-time (if
not, it’s active).

Support for other SSL-libraries like GnuTLS will be available in the
upcoming 2.x branch, which is currently under development.

#### References

HTTPS redirection:
<http://redmine.lighttpd.net/projects/1/wiki/HowToRedirectHttpToHttps>

Lighttpd Docs SSL:
<http://redmine.lighttpd.net/projects/lighttpd/wiki/Docs_SSL>

Release 1.4.30 (How to mitigate BEAST attack)
<http://redmine.lighttpd.net/projects/lighttpd/wiki/Release-1_4_30>

SSL Compression disabled by default:
<http://redmine.lighttpd.net/issues/2445>

#### How to test

See appendix [Tools](#tools).

### nginx

#### Tested with Version

1.4.4 with OpenSSL 1.0.1e on OS X Server 10.8.5

1.2.1-2.2+wheezy2 with OpenSSL 1.0.1e on Debian Wheezy

1.4.4 with OpenSSL 1.0.1e on Debian Wheezy

1.2.1-2.2 bpo60+2 with OpenSSL 0.9.8o on Debian Squeeze (note that
TLSv1.2 does not work in openssl 0.9.8 thus not all ciphers actually
work)

1.4.6 with OpenSSL 1.0.1f on Ubuntu 14.04.2 LTS

#### Settings

If you absolutely want to specify your own DH parameters, you can
specify them via

    ssl_dhparam file;

However, we advise you to read section \[section:DH\] and stay with the
standard IKE/IETF parameters (as long as they are &gt;1024 bits).

#### Additional settings

If you decide to trust NIST’s ECC curve recommendation, you can add the
following line to nginx’s configuration file to select special curves:

You might want to redirect everything to *https://* if possible. In
Nginx you can do this with the following setting:

The variable *\$server\_name* refers to the first *server\_name* entry
in your config file. If you specify more than one *server\_name* only
the first will be taken. Please be sure to not use the *\$host* variable
here because it contains data controlled by the user.

#### References

<http://nginx.org/en/docs/http/ngx_http_ssl_module.html>

<http://wiki.nginx.org/HttpSslModule>

#### How to test

See appendix [Tools](#tools).

### Cherokee

#### Tested with Version

Cherokee/1.2.104 on Debian Wheezy with OpenSSL 1.0.1e 11 Feb 2013

#### Settings

The configuration of the cherokee webserver is performed by an admin
interface available via the web. It then writes the configuration to
`/etc/cherokee/cherokee.conf`, the important lines of such a
configuration file can be found at the end of this section.

General Settings

Network

*SSL/TLS back-end*: *OpenSSL/libssl*

Ports to listen

Port: 443, TLS: TLS/SSL port

Virtual Servers, For each vServer on tab *Security*:

*Required SSL/TLS Values*: Fill in the correct paths for *Certificate*
and *Certificate key*

Advanced Options

*Ciphers*:

*Server Preference*: Prefer

*Compression*: Disabled

Advanced: TLS

SSL version 2 and SSL version 3: No

TLS version 1, TLS version 1.1 and TLS version 1.2: Yes

#### Additional settings

For each vServer on the Security tab it is possilbe to set the Diffie
Hellman length to up to 4096 bits. We recommend to use &gt;1024 bits.
More information about Diffie-Hellman and which curves are recommended
can be found in section \[section:DH\].

In Advanced: TLS it is possible to set the path to a Diffie Hellman
parameters file for 512, 1024, 2048 and 4096 bits.

HSTS can be configured on host-basis in section *vServers* / *Security*
/ *HTTP Strict Transport Security (HSTS)*:

*Enable HSTS*: Accept

*HSTS Max-Age*: 15768000

*Include Subdomains*: depends on your setup

To redirect HTTP to HTTPS, configure a new rule per Virtual Server in
the *Behavior* tab. The rule is *SSL/TLS* combined with a *NOT*
operator. As *Handler* define *Redirection* and use `/(.*)$` as *Regular
Expression* and *https://\${host}/\$1* as *Substitution*.

#### References

Cookbook: SSL, TLS and certificates:
<http://cherokee-project.com/doc/cookbook_ssl.html>

Cookbook: Redirecting all traffic from HTTP to HTTPS:
<http://cherokee-project.com/doc/cookbook_http_to_https.html>

#### How to test

See appendix [Tools](#tools).

### MS IIS

To configure SSL/TLS on Windows Server IIS Crypto can be used. [^7]
Simply start the Programm, no installation required. The tool changes
the registry keys described below. A restart is required for the changes
to take effect.

![IIS Crypto Tool<span
data-label="fig:IISCryptoConfig"></span>](img/IISCryptoConfig.png){width="41.10000%"}

Instead of using the IIS Crypto Tool the configuration can be set using
the Windows Registry. The following Registry keys apply to the newer
Versions of Windows (Windows 7, Windows Server 2008, Windows Server 2008
R2, Windows Server 2012 and Windows Server 2012 R2). For detailed
information about the older versions see the Microsoft knowledgebase
article. [^8]

``` {breaklines=""}
  [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel]
  [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\Ciphers]
  [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\CipherSuites]
  [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\Hashes]
  [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\KeyExchangeAlgorithms]
  [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel\Protocols]
```

#### Tested with Version

Windows Server 2008

Windows Server 2008 R2

Windows Server 2012

Windows Server 2012 R2

Windows Vista and Internet Explorer 7 and upwards

Windows 7 and Internet Explorer 8 and upwards

Windows 8 and Internet Explorer 10 and upwards

Windows 8.1 and Internet Explorer 11

#### Settings

When trying to avoid RC4 (RC4 biases) as well as CBC (BEAST-Attack) by
using GCM and to support perfect forward secrecy, Microsoft SChannel
(SSL/TLS, Auth,.. Stack) supports ECDSA but lacks support for RSA
signatures (see ECC suite B doubts[^9]).

Since one is stuck with ECDSA, an elliptic curve certificate needs to be
used.

The configuration of cipher suites MS IIS will use, can be configured in
one of the following ways:

1.  Group Policy [^10]

2.  Registry [^11]

3.  IIS Crypto [^12]

4.  Powershell

Table \[tab:MS\_IIS\_Client\_Support\] shows the process of turning on
one algorithm after another and the effect on the supported clients
tested using https://www.ssllabs.com.

`SSL 3.0`, `SSL 2.0` and `MD5` are turned off. `TLS 1.0` and `TLS 1.2`
are turned on.

Table \[tab:MS\_IIS\_Client\_Support\] shows the algorithms from
strongest to weakest and why they need to be added in this order. For
example insisting on SHA-2 algorithms (only first two lines) would
eliminate all versions of Firefox, so the last line is needed to support
this browser, but should be placed at the bottom, so capable browsers
will choose the stronger SHA-2 algorithms.

`TLS_RSA_WITH_RC4_128_SHA` or equivalent should also be added if MS
Terminal Server Connection is used (make sure to use this only in a
trusted environment). This suite will not be used for SSL, since we do
not use a RSA Key.

Clients not supported:

1.  Java 6

2.  WinXP

3.  Bing

#### Additional settings

It’s recommended to use Strict-Transport-Security: max-age=15768000 for
detailed information visit the [^13] Microsoft knowledgebase.

You might want to redirect everything to http**s**:// if possible. In
IIS you can do this with the following setting by Powershell:

```
Set-WebConfiguration -Location "$WebSiteName/$WebApplicationName" `
    -Filter 'system.webserver/security/access' `
    -Value "SslRequireCert"
```

#### References

<http://support.microsoft.com/kb/245030/en-us>

<http://support.microsoft.com/kb/187498/en-us>

#### How to test

See appendix [Tools](#tools).

SSH
---

### OpenSSH

#### Tested with Version

OpenSSH 6.6p1 (Gentoo)

OpenSSH 6.6p1-2 on Ubuntu 14.04.2 LTS

#### Settings

**Note:** OpenSSH 6.6p1 now supports Curve25519

#### Tested with Version

OpenSSH 6.5 (Debian Jessie)

#### Settings

#### Tested with Version

OpenSSH 6.0p1 (Debian wheezy)

#### Settings

**Note:** Older |Linux| systems won’t support SHA2. PuTTY (Windows) does
not support RIPE-MD160. Curve25519, AES-GCM and UMAC are only available
upstream (OpenSSH 6.6p1). DSA host keys have been removed on purpose,
the DSS standard does not support for DSA keys stronger than 1024bit
[^14] which is far below current standards (see [Keylengths](#keylengths).
Legacy systems can use this configuration and
simply omit unsupported ciphers, key exchange algorithms and MACs.

#### References

The OpenSSH sshd\_config man page is the best reference:
<http://www.openssh.org/cgi-bin/man.cgi?query=sshd_config>

#### How to test

Connect a client with verbose logging enabled to the SSH server

    $ ssh -vvv myserver.com

and observe the key exchange in the output.

### Cisco ASA

#### Tested with Versions

9.1(3)

#### Settings

    crypto key generate rsa modulus 2048
    ssh version 2
    ssh key-exchange group dh-group14-sha1

Note: When the ASA is configured for SSH, by default both SSH versions 1
and 2 are allowed. In addition to that, only a group1 DH-key-exchange is
used. This should be changed to allow only SSH version 2 and to use a
key-exchange with group14. The generated RSA key should be 2048 bit (the
actual supported maximum). A non-cryptographic best practice is to
reconfigure the lines to only allow SSH-logins.

#### References

[http://www.cisco.com/en/US/docs/security/asa/asa91/configuration/general/admin\_management.html ](http://www.cisco.com/en/US/docs/security/asa/asa91/configuration/general/admin_management.html )

#### How to test

Connect a client with verbose logging enabled to the SSH server

    $ ssh -vvv myserver.com

and observe the key exchange in the output.

### Cisco IOS

#### Tested with Versions

15.0, 15.1, 15.2

#### Settings

    crypto key generate rsa modulus 4096 label SSH-KEYS
    ip ssh rsa keypair-name SSH-KEYS
    ip ssh version 2
    ip ssh dh min size 2048

    line vty 0 15
    transport input ssh

Note: Same as with the ASA, also on IOS by default both SSH versions 1
and 2 are allowed and the DH-key-exchange only use a DH-group of 768
Bit. In IOS, a dedicated Key-pair can be bound to SSH to reduce the
usage of individual keys-pairs. From IOS Version 15.0 onwards, 4096 Bit
rsa keys are supported and should be used according to the paradigm “use
longest supported key”. Also, do not forget to disable telnet vty
access.

#### References

<http://www.cisco.com/en/US/docs/ios/sec_user_services/configuration/guide/sec_cfg_secure_shell.html>

#### How to test

Connect a client with verbose logging enabled to the SSH server

    $ ssh -vvv myserver.com

and observe the key exchange in the output.
