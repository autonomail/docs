Autonomail offers true **end-to-end encryption** for email.

This means that you when you send an email to someone we try to scramble it 
such that no one else but you and the recipient can read the contents of the 
email - this is what we mean by _encryption_.

Furthermore, we scramble it before it even leaves your device. And likewise, we 
only unscramble emails you receive once they have reached your device, not 
before. Thus when your emails are in transit - i.e. not on your device - they 
are encrypted (scrambled). This is what we mean by _end-to-end_.

We will now explain the nuts and bolts of this process...

---

### What is encryption?

> Encryption is a process of preventing information from being seen by anyone other than the people for whom the information is intended.

So how does this work in practice? Usually it involves taking the information you want to protect (let's call 
this _plaintext_) and passing it through a mathematical algorithm to produce some scrambled output (let's call this _ciphertext_).

The 
person receiving the message would then pass the _ciphertext_ through the inverse of the original mathematical algorithm to reproduce the 
original _plaintext_. 

For example, let's say we have a simple encryption algorithm which shifts all letters forward by 1. Here is how the process would work:

1. Alice wishes to send Bob the plaintext `hello`. 
1. Alice passes this plaintext through the encryption algorithm _(shift each letter forward in the alphabet by 1)_ to produce the ciphertext `ifmmp`. 
1. Alice then sends this ciphertext to Bob. 
1. Bob passes the received ciphertext `ifmmp` through the decryption algorithm _(shift each letter backward in the alphabet by 1)_ to get the original plaintext `hello`.

The only problem with the above encryption algorithm is that if Eve - our spy - knows it then she can intercept and decrypt the message sent from Alice to Bob.

To get around this Alice and Bob can improve their encryption algorithm. Instead of shifting letters forward by 1 why not shift by any 
arbitrary number - the _encryption key_ - which they both agree on beforehand? 

Let's say they both agree on a key of 3. Here is how the process will look:

1. Alice wishes to send Bob the plaintext `hello`. 
1. Alice passes this plaintext through the encryption algorithm _(shift each letter forward 3 times)_ to produce the ciphertext `khoor`. 
1. Alice then sends this ciphertext to Bob. 
1. Bob passes the received ciphertext `khoor` through the decryption algorithm _(shift each letter back 3 times)_ to get the original plaintext `hello`.

If Eve were to intercept the message she would need to know the key in order to successfully decrypt the ciphertext. If she doesn't she will have to try and guess it, meaning she would have to try all the possibilities until she gets something which works - this is known 
as a _brute force_ search. 

The level of security provided any good encryption process comes down to how long it would take to brute force the 
key even if one has enormous computing power at hand to automate the task. 

Our simple encryption scheme above is too easy to break, so in 
practice most encryption algorithms apply a far more complicated set of mathematical transformations, and use much larger key values 
(think numbers on the scale of 2^256 and higher!).

With a large enough key we can make a brute force hard enough that we dissuade an attacker from even trying.

### Public key cryptography

Our above encryption algorithm involves a secret key that is shared between Alice and Bob. Such algorithms are known as _symmetric_ encryption algorithms because both participants use the same key.

But with such algorithms there is always a problem. How do we get Alice and Bob to agree on a key beforehand without anyone else knowing?

In the ideal scenario they would meet in a dark, sound-proof room with a locked door and gently whisper the key into each other's ears.

But this isn't an ideal scenario. This is the internet and Alice and Bob probably don't know each other and are probably separated by thousands of miles. Yet they need to be able to communicate securely.

Luckily there is another class of encryption algorithms which can help us - _asymmetric_ or _public key_ algorithms.

A _public key_ encryption algorithm usually involves the use of 2 keys per participant:

* _Private_ key - this is a secret key that you don't give to anyone
* _Public_ key - this is a key that you can and should give to everyone

The above 2 keys are usually generated through a mathematical algorithm and are thus mathematically related to each other.

The key point: **data encrypted with the private key can only be decrypted by the corresponding public key, and vice versa**.

Thus, if you encrypt data with your private key you can only use the corresponding public key to decrypt it. And vice versa.

Back to Alice, Bob and Eve, this would be the new process:

1. Alice wishes to send Bob the plaintext `hello`. 
1. Bob generates a public and private key pair.
1. Bob sends his public key to Alice.
1. Alice encrypts the plaintext with Bob's _public_ key to produce the ciphertext.
1. Alice sends the ciphertext to Bob.
1. Bob decrypts the recieved ciphertext with his _private_ key to get back the original plaintext `hello`.

Now let's say Eve intercepts step 3, where Bob sends hi public key to Alice. If Eve modifies the public key before it reaches the Alice her 
modification will be detected later on, as Bob will not be able to decrypt the messages sent to him by Alice. 

Since Eve does not know Bob's private key she cannot decrypt messages sent by Alice to Bob. However Eve still knows Bob's public key, 
which means she can still send him a message pretending to be Alice.

How can we prevent Eve from being able to do this? we need a way for Bob to verify that the message did indeed come from Alice and that 
it hasn't been tampered with. This is where _digital signatures_ come into play.

### Digital signatures

A digital signatures in an email is similar in concept to a hand-written signature in a letter. It allows the receiver to verify that the email content hasn't been tampered and that the email is actually from the person who signed it. 

Digital signature are not only unique to the person signing but also to the document being signed. If the signed 
document is modified even slightly then the signature verification will fail. 

Most importantly, forging a digital signature is considered to be as difficult as brute-forcing an encryption key, thus making them reliable indicators of identity.

Digital signatures for email are generated by running the email body through a mathematical algorithm - known as a _hashing_ algorithm - which then outputs a unique number representing that email body.

Here is how our communication between Alice and Bob might now look:

1. Alice wishes to send Bob the plaintext `hello`. 
1. Alice generates a public and private key pair.
1. Alice sends her public key to Bob.
1. Bob generates a public and private key pair.
1. Bob sends his public key to Alice.
1. Alice hashes the plaintext to produce a unique hash.
1. Alice encrypts the unique hash with her private key to produce the digital signature.
1. Alice attaches the digital signature to the end of the plaintext to produce the signed plaintext.
1. Alice encrypts the signed plaintext with Bob's public key to produce the ciphertext.
1. Alice sends the ciphertext to Bob.
1. Bob decrypts the received ciphertext with his private key to produce the signed plaintext.
1. Bob detaches the digital signature from the signed plaintext to produce the plaintext.
1. Bob hashes the plaintext to produce a unique hash.
1. Bob decrypts the detached digital signature using Alice's public key and compares the result to the unique hash just calculated.
1. If both hashes match then Bob can be sure that the message came from Alice and that it hasn't been modified.

With the above process Eve can longer decrypt messages sent to Bob nor send messages pretending to be Alice without being detected.


Note: **a message does not need to be encrypted in order to be digitally signed**. If you need to email somebody 
who does not have email encryption enabled can still digitally sign your messages to them so that they can 
at least verify their authenticity and integrity. 

### How Autonomail does it

Autonomail uses an implementation of the [OpenPGP standard](http://tools.ietf.org/html/rfc4880) for encrypting and digitally signing email messages. OpenPGP's process is rougly similar to 
the encryption process we've outlined above with a few differences for efficiency purposes.

We also automate as much of the process as we can:

* **We automatically generates a public and private key pair for you when you first sign up.** We store your generated keys within a [keychain](/docs/client/keychain). This keychain also contains public keys for people you have communicate with. Your 
keychain itself is encrypted and [stored securely](/docs/server/private-data).

* **We automatically encrypt all outgoing email messages where you have the recipient's public key.** When receiving someone's public key for the first time we recommend that you double check with them to make 
sure that it is indeed their public key and not a fraudulent one. 

* **We automatically digitally sign all outgoing emails.** Not everyone you communicate with will have email encryption enabled. Nevertheless we at least digitally sign all your emails so that recipients can verify their integrity and authenticity.

---

### Technical details

The specific PGP implementation we use is an [Emcripten port of Gnu Privacy Guard (GnuPG) 2](). The 
random data generator is seeded with output from the [Fortuna PRNG](https://www.schneier.com/fortuna.html) implementation in [SJCL](http://bitwiseshiftleft.github.io/sjcl/). 
The entropy sources for this include 
`window.crypto.getRandomValues()`, mouse movements,  keyboard strokes and accelerometer input. 





