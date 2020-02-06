
`jwt` - JSON Web Token

### `jwt structure`

JWT is an open standard for transmitting information as JSON object between two parties. The information can be securely decrypted because it is digitally signed.

```
    header.payload.signature
```

`JOSE Header`
The header has metadata about the encryption operations which are applied to the whole JWT token. For example, it can describe the algorithm which is used to encrypt the token.

```
<!-- Header -->
{
  "alg": "HS256", // the algorithm used
  "typ": "JWT"
}

(base64Url - compressed without spaces)
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```

The header can contain other claims (key-value parameters), but the most common ones are `listed below`.
A side note, you can have the `alg: "none"` which would tell that the JWT token is not encrypted by any algorithm, thus being able to be decrypted.

```
{
    "exp": timestamp // expiration
    "iat": timestamp // issued
    "iss": string // issuer
}
```

`Payload`
The payload is the part where usually user specific information is stored.

```
<!-- Payload -->
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}

(base64Url - compressed without spaces)
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9
```

`Signature`

```
<!-- Signature -->

data = base64UrlEncoder(header) + '.' + base64UrlEncoder(payload)
signature = hashFunction(data, secret)

```
 