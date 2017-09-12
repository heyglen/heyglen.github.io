---
layout: post
title: "Certificates and Openssl: Convert PFX to PEM"
---

# Extract the Keys

```bash
openssl pkcs12 -in keys.pfx
```

This outputs the

 * Private key
 * Public key
 * Signing CA public keys

# Private Key 

The private key is a base64 encoded string between some delimiters

If the key is encrypted:

```
-----BEGIN ENCRYPTED PRIVATE KEY-----
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
-----END ENCRYPTED PRIVATE KEY-----
```

decrypt it with:

```
openssl rsa -in encrypted_private_key.rsa
```

your private key:

```
-----BEGIN RSA PRIVATE KEY-----
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
-----END RSA PRIVATE KEY-----
```

# The Public Key (Certificate)

Which public key matches the private key? Check the subject:

```
subject=/C=XX/ST=XXX/L=XXX/O=XXX/OU=XXXX/CN=hostname.example.com
issuer=/DC=com/DC=EXAMPLE/CN=CA
-----BEGIN CERTIFICATE-----
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
-----END CERTIFICATE-----
```

![Private Key](/public/img/master-key.png){:class="img-responsive"}

