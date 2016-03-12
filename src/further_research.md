Further research
================

The following is a list of services, software packages, hardware devices
or protocols that we considered documenting but either did not manage to
document yet or might be able to document later. We encourage input from
the Internet community.

  * Lync
  * Wi-Fi APs, 802.1X
  * Tomcat
  * SIP
  * SRTP
  * DNSSec (mention BCPs)
  * DANE
  * TOR
  * S/Mime (check are there any BCPs? )
  * TrueCrypt, LUKS, FileVault
  * AFS
  * Kerberos
  * NNTP
  * NTPs tlsdate
  * BGP / OSPF
  * LDAP
  * seclayer-tcp
  * Commerical network equipment vendors
  * RADIUS
  * Moxa , APC, und co... ICS . Ethernet to serial
  * rsyslog
  * v6 spoofing (look at work by Ferndo Gont, Marc Heuse, et. al.)
  * tinc
  * racoon
  * l2tp
  * telnets
  * ftps
  * DSL modems (where to start?)
  * UPnP, natPmp
  * SAML federated auth providers [^51]
  * Microsoft SQL Server
  * Microsoft Exchange
  * HAProxy[^52]
  * HTTP Key Pinning (HTKP)
  * IBM HTTP Server
  * Elastic Load Balancing (ELB)[^53]

Software not covered by this guide
----------------------------------

telnet: Usage of telnet for anything other than fun projects is highly
discouraged

Simple Network Management Protocol (SNMP): Remote Management Software
should not be available from a routed network. There is an inestimable
number of problems with these implementations. Popular vendors regularly
have exploits or DDoS problems with their embedded remote management and
are suffering from SNMP stacks.[^54] Tunneling these services over SSH
or stunnel with proper authentication can be used if needed.

Puppet DB: A Proxy or a tunnel is recommended if it needs to be facing
public network interfaces.[^55]

rsync: Best use it only via SSH for an optimum of security and easiest
to maintain.
