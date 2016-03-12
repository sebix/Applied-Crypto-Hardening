### HTTP Public Key Pinning (HPKP)

Much like HTTP Strict Transport Security (HSTS), HTTP Public Key Pinning (HPKP) is a Trust On First Use (TOFU) mechanism. It protects HTTPS websites from impersonation using certificates issued by compromised certificate authorities. The data for Pinning is supplied by an HTTP-Header sent by the WebServer. 

#### HPKP Header Directives

HPKP provides two different types of headers:

  * Public-Key-Pins
      * Public-Key-Pins-Report-Only

HPKP header can be parametrized by following directives:

  * `pin-sha256="<YOUR\_PUBLICKEY\_HASH=>"`
  * `max-age=<number-of-seconds>`
      * `includeSubdomains`
      * `report-uri="<https://YOUR.URL/TO-REPORT>"`

**pin-sha256** is a required directive. It can and should be used several (at least two) times for specifying the public keys of your domain-certificates or CA-certificates. Operators can pin any one or more of the public keys in the certificate-chain, and indeed must pin to issuers not in the chain (as, for example, a Backup Pin). Pinning to an intermediate issuer, or even to a trust anchor or root, still significantly reduces the number of issuers who can issue end-entity certificates for the Known Pinned Host, while still giving that host flexibility to change keys without a disruption of service. OpenSSL can be used to convert the Public Key of an X509-Certificate as follows:

    openssl x509 -in <certificate.cer> -pubkey -noout | 
     openssl rsa -pubin -outform der | 
     openssl dgst -sha256 -binary | 
     openssl enc -base64
    writing RSA key
    pG3WsstDsfMkRdF3hBClXRKYxxKUJIOu8DwabG8MFrU=

This piped usage of OpenSSL first gets the Public-Key of `certificate.cer`, converts it do DER (binary) format, calculates an SHA256 Hash and finally encodes it Base64. The output (including the ending Equal-Sign) is exactly whats needed for the `pin-sha256="<YOUR\_PUBLICKEY\_HASH=>"` parameter. To generate the Hash for a prepared Backup-Key just create a Certificate-Signing-Request and replace `openssl x509` by `openssl req -in <backup-cert.csr> -pubkey -noout` as first OpenSSL command.
Instead of using OpenSSL even WebServices like <https://report-uri.io/home/pkp_hash/> can be used to get a suggestion for the possible Public-Key-Hashes for a given WebSite.

**max-age** is a required directive (when using the *Public-Key-Pins* header). This directive specifies the number of seconds during which the user agent should regard the host (from whom the message was received) as a Known Pinned Host.

**includeSubdomains** is an optional directive. This directive indicates that the same pinning applies to this Host as well as *any subdomains of the host's domain name*. Be careful - you need to use a Multi-Domain/Wildcard-Certificate or use the same Pub/Private-Keypair in all Subdomain-Certificates or need to pin to CA-Certificates signing all your Subdomain-Certificates.

**report-uri** is an optional directive. The presence of a report-uri directive indicates to the Browser that in the event of Pin Validation failure it should post a report to the report-uri (HTTP-Post is done using JSON, Syntax see RFC-7469 Section 3[^rfc7469-3]). There are WebServices like <https://report-uri.io/> out there which can be used to easily collect and visualize these reports.

#### HPKP Client Support

HPKP is supported[^caniuse-pkp] by these web browsers:

  * Firefox version &ge; 35
  * Chrome version &ge; 38
  * Android Browser &ge; 44
  * Opera version &ge; 25 

Currently (16. Oct 2015) there is no HPKP Support in: Apple Safari, Microsoft Internet Explorer and Edge.

#### HPKP Considerations
Before enabling HPKP it is recommended to consider following:

  * Which Public-Keys to use for Pinning (Certificat + Backup-Certificate, CAs, Intermediate-CAs)
	  * Proper value of \emph{max-age}. Start testing with a short Period, increase Period after deployment.
	  * Be careful when using \emph{includeSubdomains}, are all your subdomains covered by the defined Public-Key-Hashes?

The administrators are advised to watch for expiration of the SSL certificate and handle the renewal process with caution. If a SSL certificate is renewed without keeping the Public-Key (reusing the CSR) for an HPKP enabled domain name, the connection to site will break (without providing override mechanism to the end user).

#### Testing HPKP
HPKP can be tested either using locally or through the Internet. 

There is a handy Bash-Script which uses OpenSSL for doing several SSL/TLS-Tests available at <https://testssl.sh/>

    wget -q https://testssl.sh/testssl.sh
    wget -q https://testssl.sh/mapping-rfc.txt
    chmod 755 ./testssl.sh
    ./testssl.sh https://your-domain.com

    # Sample Output, just HSTS and HPKP Section (Full report is quite long!):
    Strict Transport Security    182 days=15724800 s, includeSubDomains
    Public Key Pinning # of keys: 2, 90 days=7776000 s, just this domain
               matching host key: pG3WsstDsfMkRdF3hBClXRKYxxKUJIOu8DwabG8MFrU

For local testing it is possible to utilize Google Chrome Web browser, just open the Chrome net-internals-URL: <chrome://net-internals/#hsts>.

For Mozilla Firefox there is an Plug-In provided by the "Secure Information Technology Center Austria" available: <https://demo.a-sit.at/firefox-plugin-highlighting-safety-information/>

Testing over the Internet can be conducted by Qualys SSL Labs test <https://www.ssllabs.com/ssltest/>. *Public Key Pinning (HPKP)* information is located in the *Protocol Details* section. 

There is also a fast online HPKP-only check at <https://report-uri.io/home/pkp_analyse>.

#### References

  * OWASP: Certificate and Public Key Pinning: <https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning>
  * HPKP Browser Compatibility List: <http://caniuse.com/\#feat=publickeypinning>
  * RFC 7469: Public Key Pinning Extension for HTTP - Examples: <https://tools.ietf.org/html/rfc7469\#section-2.1.5>

[^rfc7469-3]: https://tools.ietf.org/html/rfc7469#section-3

[^caniuse-pkp]: <http://caniuse.com/\#feat=publickeypinning>
