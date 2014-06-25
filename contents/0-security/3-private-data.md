We expect that you may need to access your email from multiple devices (e.g. your desktop when you're at your desk and your mobile when you're on the move).

In order to make this as painless as possible we need to store certain data related to your account on our server so that every device can access it. Such data includes your [encryption keys](/docs/security/email-encryption) and your [keychain](/docs/client/keychain).

We don't want to put this data at risk when it is stored on your device, and nor do we want anybody who is able to access our servers (including ourselves) to be be able to access this data. So...

**We encrypt your private data before storing it on your device or sending it to our server.**

The encryption key for this is different to your email encryption keys and generated through a different algorithm. It is derived from your password (which we [never send to our server](/docs/security/passwords)). 

We store it locally on each device so that you can still access it without an internet connection. If there is an internet connection then we refresh what is stored locally with what's on the server. So any changes you make to your settings on one device will be reflected on your other devices.

---

### Technical details

The private data is encrypted using AES-256 in GCM mode.

The key used for encryption is derived as follows:

1. Use the [Fortuna PRNG](https://www.schneier.com/fortuna.html) to generate a 256-bit `salt`. 
1. Derive the key: input `salt` + user's password into [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) + [SHA-512](https://en.wikipedia.org/wiki/SHA512) for `n` iterations, where `n` is chosen such that the total key derivation 
process lasts 1 second. On a typical machine (e.g. Macbook Air 2012) `n` is well above 10,000.
1. Take the first 256 bits of the derived key and use as the encryption key.

_We use the [SJCL](http://bitwiseshiftleft.github.io/sjcl/) implementations of both AES, SHA-512, and PBKDF2._

The private data encrypted ciphertext and the `salt` which was used as input to the key derivation process are both backed up to the Autonomail server. The `n` iteration count is only stored locally on the device and never sent to the server - the user can view it at any time by visiting [settings](/docs/client/multiple-devices) page in the app.

