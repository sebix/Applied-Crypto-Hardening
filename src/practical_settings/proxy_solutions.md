Intercepting proxy solutions and reverse proxies
------------------------------------------------

<span>Proxies</span> Within enterprise networks and corporations with
increased levels of paranoia or at least some defined security
requirements it is common **not** to allow direct connections to the
public internet.

For this reason proxy solutions are deployed on corporate networks to
intercept and scan the traffic for potential threats within sessions.

For encrypted traffic there are four options:

Block the connection because it cannot be scanned for threats.

Bypass the threat-mitigation and pass the encrypted session to the
client, which results in a situation where malicious content is
transferred directly to the client without visibility to the security
system.

Intercept (i.e. terminate) the session at the proxy, scan there and
re-encrypt the session towards the client (effectively MITM).

Deploy special Certificate Authorities to enable Deep Packet Inspection
on the wire.

While the latest solution might be the most “up to date”, it arises a
new front in the context of this paper, because the most secure part of
a client’s connection could only be within the corporate network, if the
proxy-server handles the connection to the destination server in an
insecure manner.

Conclusion: Don’t forget to check your proxy solutions SSL-capabilities.
Also do so for your reverse proxies!

### Bluecoat

#### Tested with Versions

SGOS 6.5.x

BlueCoat Proxy SG Appliances can be used as forward and reverse proxies.
The reverse proxy feature is rather under-developed, and while it is
possible and supported, there only seems to be limited use of this
feature “in the wild” - nonetheless there are a few cipher suites to
choose from, when enabling SSL features.

##### Only allow TLS 1.0,1.1 and 1.2 protocols:

 

    $conf t
    $(config)ssl
    $(config ssl)edit ssl-device-profile default
    $(config device-profile default)protocol tlsv1 tlsv1.1 tlsv1.2
      ok

##### Select your accepted cipher-suites:

 

    $conf t
    Enter configuration commands, one per line.  End with CTRL-Z.
    $(config)proxy-services
    $(config proxy-services)edit ReverseProxyHighCipher
    $(config ReverseProxyHighCipher)attribute cipher-suite
    Cipher#  Use        Description        Strength
    -------  ---  -----------------------  --------
          1  yes            AES128-SHA256      High
          2  yes            AES256-SHA256      High
          3  yes               AES128-SHA    Medium
          4  yes               AES256-SHA      High
          5  yes       DHE-RSA-AES128-SHA      High
          6  yes       DHE-RSA-AES256-SHA      High
                   [...]
         13  yes          EXP-RC2-CBC-MD5    Export

    Select cipher numbers to use, separated by commas: 2,5,6
      ok

The same protocols are available for forward proxy settings and should
be adjusted accordingly: In your local policy file add the following
section:

    <ssl>
        DENY server.connection.negotiated_ssl_version=(SSLV2, SSLV3)

Disabling protocols and ciphers in a forward proxy environment could
lead to unexpected results on certain (misconfigured?) webservers (i.e.
ones accepting only SSLv2/3 protocol connections)

### HAProxy

HAProxy can be used as loadbalancer and proxy for TCP and HTTP-based
applications. Since version 1.5 it supports SSL and IPv6.

#### Tested with Versions

HAProxy 1.5.11 with OpenSSL 1.0.1e on Debian Wheezy

#### Settings

#### Additional Settings

##### Enable Support:

 

    	bind *:443 ssl crt server.pem npn "http/1.1,http/1.0"

Append the npn command in the frontend configuration of HAProxy.

##### Enable OCSP stapling:

  HAProxy supports since version 1.5.0 OCSP stapling. To enable it you
have to generate the OCSP singing file in the same folder, with the same
name as your certificate file plus the extension .ocsp. (e.g. your
certificate file is named server.crt then the OCSP file have to be named
server.crt.oscp)

To generate the OCSP file use these commands:

    openssl x509 -in your.certificate.crt -noout -ocsp_uri # <- get your ocsp uri
    openssl ocsp -noverify -issuer ca.root.cert.crt -cert your.certificate.crt -url "YOUR OCSP URI" -respout your.certificate.crt.ocsp

Reload HAProxy and now OCSP stapling should be enabled.

Note: This OCSP signature file is only valid for a limited time. The
simplest way of updating this file is by using cron.daily or something
similar.

##### Enable :

  Get certificate informations:

    openssl x509 -in server.crt -pubkey -noout | openssl rsa -pubin -outform der | openssl dgst -sha256 -binary | base64

Then you append the returned string in the HAProxy configuration. Add
the following line to the backend configuration:

    rspadd Public-Key-Pins:\ pin-sha256="YOUR_KEY";\ max-age=15768000;\ includeSubDomains

Reload HAProxy and HPKP should now be enabled.

Note: Keep in mind to generate a backup key in case of problems with
your primary key file.

#### How to test

See appendix \[cha:tools\]

### Pound

#### Tested with Versions

Pound 2.6

#### Settings

### stunnel

#### Tested with Versions

stunnel 4.53-1.1ubuntu1 on Ubuntu 14.04 Trusty with OpenSSL 1.0.1f,
without disabling Secure Client-Initiated Renegotiation

stunnel 5.02-1 on Ubuntu 14.04 Trusty with OpenSSL 1.0.1f

stunnel 4.53-1.1 on Debian Wheezy with OpenSSL 1.0.1e, without disabling
Secure Client-Initiated Renegotiation

#### Settings

#### Additional information

Secure Client-Initiated Renegotiation can only be disabled for stunnel
versions &gt;= 4.54, when the renegotiation parameter has been added
(See changelog).

#### References

stunnel documentation: <https://www.stunnel.org/static/stunnel.html>

stunnel changelog: <https://www.stunnel.org/sdf_ChangeLog.html>

#### How to test

See appendix \[cha:tools\]
