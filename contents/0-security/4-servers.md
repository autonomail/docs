The security of our offering comes primarily from how we have architected our system. If you follow [our guidelines](/docs/security/privacy) then even an attacker who compromises our server would still not be able to read your emails or know your password.

But we still do everything can to secure our servers and ensure they don't get compromised.

All our servers run up-to-date versions of Ubuntu Linux (a solid open source operating system) and are firewalled to prevent unwanted intrusions. We monitor them 24/7 to ensure that they are functioning properly.

We use industry-grade encryption to ensure that no one can eavesdrop on your communication with our servers. You will see that our website URL has a `https://` prefix and your browser will most probably display a padlock symbol to you to indicate that the connection is secure.

Our email servers are located in countries well known for their [strict data privacy laws](http://nomadcapitalist.com/2013/12/15/top-5-best-countries-host-website-data-privacy/) so that law enforcement agencies and governments cannot access any data without formally obtained legal warrants.

---

### Technical details

All servers run Ubuntu 12.04+ LTS and Docker. All incoming HTTP connections must be over TLS. We have Perfect Forward Secrecy (PFS) enabled on all connections.

You can view an [live report on our server connection security](https://www.ssllabs.com/ssltest/analyze.html?d=autonomail.com).

_We will have more details on our server setup when we launch._
