Abstract

   This document proposes a method to provide end to end security to
   Attributes in RADIUS Messages.


Introduction

   In a typical scenario, there could be multiple RADIUS proxies between
   NAS and RADIUS server. RADIUS ensures hop to hop integrity by using
   pre-shared secret between hops. End to end connection between NAS and
   RADIUS server is through the set of transitive links involving
   proxies. RADIUS over TLS can be deployed to have the security between
   hop to hop only. Incase of Roaming scenarios, the NAS, proxies, and
   home server will typically be managed by different administrative
   entities. And Proxies have complete access to messages exchanged
   between NAS and RADIUS Server.


Problem Scenario

   No end to end security in RADIUS. Security Mechanism like Radius over
   TLS provides only Hop to Hop security. TLS involves additional
   overhead of certificate maintenance and cost. As the proxy has the
   right to modify the message, that introduces following security
   threats [RFC2607].

   Message editing
   Attribute editing
   Theft of password
   Theft and modification of accounting data
   Replay attacks
   Connection hijacking
   Fraudulent accounting.

   Both NAS and RADIUS Server are not aware of the changes made in the
   message by the proxies and assumed to be in trusted relationship with
   proxies in Proxy chaining scenarios. This can cause havoc in roaming
   environments.


Solution

   Introduce Security Attribute 'X' as a container to hold set of
   sensitive attributes that need to be protected. Sensitive attributes
   such as vlan, password,.. can be placed as sub TLVs. The attributes
   that should not be modified by proxy can be also be placed in this
   vendor specific attribute as sub-TLVs.

   Configure pre-shared secret 'K' that is known only to NAS/RADIUS
   Client and RADIUS Server.

   Configure algorithm 'E' at NAS/RADIUS Client and RADIUS Server.
   Algorithm can be any standard algorithm such as AES.

   Configure Hashing algorithm 'H' at NAS/RADIUS Client and RADIUS
   Server. Algorithm can be any standard algorithm such as SHA-1.

   Hash the contents of Security Attribute X using the algorithm H and
   append the hash as sub TLV. This ensures integrity of the message
   that needs to be protected.

   Encrypt the contents of Security Attribute X using the algorithm E
   with the key K. This ensures confidentiality of the message that
   needs to be protected.

   Only the Security Attribute 'X' will be sent in the RADIUS Message
   and the rest of parameters(K, E and H) will be configurations made at
   NAS/RADIUS Client and RADIUS Server. 


Advantages

   Rogue Proxy cannot decrypt the protected Security Attribute contents.
   Hence, security in Roaming scenarios is assured.

   Concept of un-modifiable or protected attributes is introduced. With
   this, it is possible to exchange sensitive attributes safely between
   NAS/RADIUS Client and RADIUS Server.

   Even if any Rogue proxy removes the Security Attribute, RADIUS server
   or NAS can potentially drop the message as the Security Attribute is
   expected in the message with respect to configuration.

   Both NAS and RADIUS Server can safely assume the trust relationships
   with proxies.

   End to end security is accomplished with this mechanism.

   Compared to TLS, this proposal ensures end to end security with less
   cost and maintenance overhead.

   Sensitive contents are protected from man in the middle.


References

  Normative References

   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119, March 1997.

   [RFC2865]  Rigney, C., Willens, S., Rubens, A., and W. Simpson,
              "Remote Authentication Dial In User Service (RADIUS)",
              RFC 2865, June 2000.

   [RFC3575]  Aboba, B., "IANA Considerations for RADIUS (Remote
              Authentication Dial In User Service)", RFC 3575,
              July 2003.

   [RFC5226]  Narten, T. and H. Alvestrand, "Guidelines for Writing an
              IANA Considerations Section in RFCs", BCP 26, RFC 5226,
              May 2008.

   [RFC6158]  DeKok, A. and G. Weber, "RADIUS Design Guidelines",
              BCP 158, RFC 6158, March 2011.

   [RFC6929]  DeKok, A. and A. Lior, "Remote Authentication Dial In User
              Service (RADIUS) Protocol Extensions", RFC 6929,
              April 2013.

  Informative References

   [RFC2607] Abobam B., Vollbrecht J., "Proxy Chaining and Policy 
             Implementation in Roaming", RFC2607, June 1999.

   [RFC6613] DeKok, A., "RADIUS over TCP", May 2012.

   [RFC2868] Zorn, G., Leifer, D., Rubens A., Shriver, J.,
             Holdrege, M., Goyret, I, "RADIUS Attributes for Tunnel
             Protocol Support", June 2000

   [RFC6929] DeKok, A. and Lior , A., "Remote Authentication Dial-In
             User Service RADIUS) Protocol Extensions", April 2013.

   [RFC5080] Nelson, D. and DeKok. A., "Common Remote Authentication
             Dial In User Service (RADIUS) Implementation Issues and
             Suggested Fixes"

   [RFC2867] Zorn, G., Aboba, B. and Mitton, D., "RADIUS Accounting
             Modifications for Tunnel Protocol Support", June 2000.

   [RFC5997] DeKok. A., "Use of Status-Server Packets in the
             Remote Authentication Dial In User Service (RADIUS)
             Protocol", August 2010.
