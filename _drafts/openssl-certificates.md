---
layout: post
title: Openssl Certificates
---

Lets generate a certificate signing request (CSR) using openssl

# Create a RSA Key

```bash
openssl genrsa -des3 -out private_key.pem 2048
```

# Create the Certificate Signing Request

```bash
openssl req -key private_key.key -out certificate_signing_request.csr
```

# Create the Certificate Signing Request

```bash
openssl req -key private_key.key -out certificate_signing_request.csr
```
