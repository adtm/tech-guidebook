### `tls / ssl`

When one is browsing the internet, you are using the `HTTP` protocol, which communicates between the client and the receiving end server. `f.e send information when you buy a product`

In some websites, you will see the beginning not as `HTTP`, but `https`. The extra `s` stands for `secure`. Meaning the security layer of your clients `(f.e browser)` and the end servers communication channel. In the context of `ssl and tls` we're talking about communication encryption, that a middle man cannot read and intercept your communication.

```python
Alice ----------- Hacker --------- Bob
[ #1 ] ---------> [ #2 ] -------> [ #3 ]

#1. You send information about your credit card
#2. A hacker reads and changes the information
#3. You have money issues

*msg - message
```

The main point is that when a communication channel is not `secure` all data is sent as plaintext. Meaning `CVC 432` will be sent as it, after encryption it could just become `lkm3#091#` and no one could read it `except the receiving end` (we will get to there)

Nowadays most people associate the `s` to the `ssl` protocol, but in fact, it has become deprecated and the `s` usually stands for `tls` (although most people talk interchangeably between `ssl and tls`). Why `tls` and `ssl` are named differently describing a similar protocol? `SSL v3.0` was introduced in 1996 by `Netscape`, wanting to not associate the name with `Netscape` the name `tls` was introduced with later versions.

### `how it works`

As we talked before, the main idea is to secure the network so no one can see what you are sending to the other party, encoding the information and making it from plaintext to some gibberish `Pay 200$ to ks$20a)3laA#lm!` which can't be understood by everyone, except the receiving party which is able to decrypt `ks$20a)3laA#lm! to Pay 200$`.

There are mainly two ways how to allow this

#### `1. Symmetric Encryption`
Imagine you have a safe where your valuable belonging are stored. You probably don't want everyone to be able to take them out. Therefore you have one key, which is able to lock and unlock the safe.

In the context of encryption it's the same. As an example, you have a text of `Send the money to X address a 9 PM` and your goal is to scramble the text so it cannot be easily decyphered. During the first world war the most common `cipher (encryption function)` was the `caeser cipher`, where the digits are rotated to the right of the alphabet by some `X` shift.

```python
Send the money to X address a 9 PM

# -- with a shift of 7 becomes --

Zluk aol tvulf av E hkkylzz h 9 WT
```

Of course, it isn't the most secure `cipher` as it can be easily decoded by everyone will little of time. Nowadays we have more secure algorithms like `RSA, AES, 3DES`.


#### `2. Asymmetric Encryption`
Let's imagine the same safe but the lock can have *3 states* and *2 types* of keys exist.

<img src="https://www.cloudflare.com/resources/images/slt3lc6tev37/7kVktQFB6hm8LvVzJbtndu/8b55e437dd1a0d3de90ce36c24ba9034/ssl-lock-analogy.svg" alt="Multiple state example" width="400"/>

*Image of Cloudflare*

1. One key can only turn from `left to right` (`private key`)
2. One key can only turn from `right to left` (`public key`)

By having these 2 key options, we can be sure that by locking the lock with the `private key`, only the person with the `public key` can unlock it and vice versa. The name `public key` means that we are not afraid to distribute this key to everyone and is `public`.
The `private key` should be stored securely, because it authenticates that we, and only we have locked the lock.

### `tls handshake`

Now let's get back to the `HTTP(s)` part and where `tls` comes to play. Like mentioned, there are to main ways to encrypt (`lock`) some kind of information and transfering the `key` to be able too safely decrypt.

As you probably understood, having `symmetric encryption` is not a safe solution, cause we have to send the `encryption / decryption key` to the receiving party. And if anyone is looking at the public trafic, they can easily get a hold of it. In the `ssl / tls` protocol a mixture of both `symmetric` and `asymmetric` is used.

```python
Client ------------------ Server
  X          --> #1         X
             <-- #2
```
`#1.` The client sends the supported versions of `SSL and TLS` + supported versions of ciphers (RSA, AES or etc). The server decides which cipher and version of TLS to use. If there are none which are supported, a failure is returned.

`#2.` The server send his SSL certificate, which is signed by a authorized CA, chosen cipher and TLS version.

#### `SSL Certificates`

In order to be trusted by other people, you need to be verified by a `certified trusted party`, similar to when you take a loan from an unknown person, you have the aggreement formed by a legal party, the `bank`.

```python
Client ------------------ Server
  X          --> #1         X
             <-- #2
             --> #3
                           (#4)
```
`#3.` the client confirms that the ssl certificate is trustworthy. This is done by decrypting it with the CA authorities public key and checking the domain name an other information.
Also, the client generates a pre-master key (not getting into the details), which is signed by the servers public key.

`#4.` Because the pre-master key was signed by the public key of the server, only the private key and decrypt it. After doing so, the client and the server now have a shared key which is used to encrypt and decrypt all the communication.

---
##### `P.S Have in mind TLS 1.3 works differently by having less roundtips`
##### `P.P.S This is a generic explanation to have an understanding how data travels without digging into many technical details.`