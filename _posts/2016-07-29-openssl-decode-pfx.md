---
layout: post
title: "Openssl pfx⟶pem"
---

You have a certificate & key in a pfx file and need the host certificate and key in pem base64 format.

# Verify PEM Format

PEM format is encoded in base64. If you open the file in a text editor, base64 looks something like this:

```
-----EXAMPLE TEXT-----
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
...etc...
-----EXAMPLE TEXT-----
```

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
-----END ENCRYPTED PRIVATE KEY-----
```

**OR**

```
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,1234567890ABCDEF

ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
-----END RSA PRIVATE KEY-----
```

decrypt it with:

```
openssl rsa -in encrypted_private_key.rsa
```

your unencrypted private key:

```
-----BEGIN RSA PRIVATE KEY-----
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
-----END RSA PRIVATE KEY-----
```

# The Certificate (Public Key)

Where many certificates output? It is the CA chain together with the certificate matching the key. Which certificate matches the key? Check the subject:

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


