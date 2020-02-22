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

There are mainly two ways how to allow this:
1. Symmetric Encryption
2. Asymmetric Encryption


