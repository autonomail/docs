We're all too used to hearing about [passwords](http://www.reuters.com/article/2014/05/21/us-ebay-password-idUSBREA4K0B420140521) [being](http://en.wikipedia.org/wiki/2012_LinkedIn_hack) [stolen](http://www.theguardian.com/technology/2014/jan/10/hackers-stole-personal-information-target) from the servers of established, reputable companies who claim to have "secured" their systems.

Unlike most other companies protecting your privacy and keeping your personal data secure is one of Autonomail's core principles.

For this reason we operate a [zero-knowledge proof](http://en.wikipedia.org/wiki/Zero-knowledge_proof)-based authentication system. What this means is 
that **your password is never stored on or sent to our server**. We are able to authenticate you without needing to do that. Even if our server is compromised your password will not be. 

### Password recovery 

Since we never see your password we cannot help you recover it in case you lose it.

We also derive [encryption keys](/docs/security/email-encryption) based on your password. So losing your password will also lose you the ability to send and receive emails encrypted with your encryption existing keys.

Once we have identified you through other means (e.g. payment information) we will be able to restore access to your account wherein you will be able to set a new password and then generate new encryption keys to continue using our service.

Since losing your password would be very inconvenient we recommend you use an open source password managers such as  [KeePass](http://keepass.info/) or [Clipperz](https://clipperz.is/) to remember it.

---

### Technical details

We use the [Secure Remote Password](http://srp.stanford.edu/design.html) protocol for zero-knowledge authentication. The implementation 
we use is provided by [SJCL](http://bitwiseshiftleft.github.io/sjcl/).

Other third-party service which use zero-knowledge authentication often also provide a web-based login feature for users who don't have the client app to hand. We do not provice such a feature as we consider it to be too much of a security risk; it is very difficult to ensure that Javascript downloaded by your browser when you load a page from the internet hasn't been maliciously tampered 
with.