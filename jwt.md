
### `jwt - JSON Web Token`

JWT's are tokens, which are used to transfer information between multiple parties, f.e the client and the server. They are an stateless alternative for session tokens, which are stored in the server. Having the benefit of not needing to worry about distributed system questions, in memory caches, single point of failures.

## `JWT structure`

The token consists of `3` main parts:
```python
1. Header
2. Payload
3. Signature

# raw form
header.payload.signature
```

### `header`

The `header` or `JOSE header` stores the metadata about the used encryption algorithm, token expiration, key identifier for which key was used in hashing the signature. This information is used for decypting the `signature` in the `JWT`. Of course, the secret / private keys must be stored securely by oneself.
 
```python
{
  "alg": "HS256"
  "typ": "JWT",
  "exp": 1581508843,
  "iat": 1581502642,
  "iss": "site.service"
} 
```

### `payload`
In the payload the information you want to transfer is stored. It can be `roles`, `userId` or anything you want. This information is visible to everyone, therefore you shouldn't store sensitive information.

```python
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

### `signature`

The signature is the guarantee that the information in the payload has not been tampered with. In the server, where we check the `authenticity` of the `JWT` token we do the following operations:


```python
 secret = "<-- your secret from a file or server -->"

 # or other specified "alg"
 expectedSignature = HMAC256(
    base64(header) + "." + base64(payload),
    secret
 )

 return expectedSignature == signature
```
 
The main idea is that the information in the `payload` is encrypted in the `signature`. Therefore when checking if the `JWT` is valid, we check with the signature. If let's say you change the `payload` in the token, it will become in valid, because this change doesn't propogate to the `signature` which has the correct information.`

## `Purpose`

The main purpose of `session tokens` and `JWT's` is an opportunity to send data to the server without being affaid of someone tampering with it. If you would plainly send a `userId` to the server, anyone could just change the `userId` and make the same request, receiving information.

The `JWT` are more commonly used nowadays, as there are less problems with the implementations. Like mentioned, various in memory caches where the token must be stored, availability and partition tolerance. 

Of course, if one would like to `blacklist` access to a specific token or to all of them, some state is required. Therefore having state is not a bad thing, it just adds extra complexity to the system.