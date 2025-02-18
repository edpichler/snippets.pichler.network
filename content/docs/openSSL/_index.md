---
title: "OpenSSL"
weight: 10
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# OpenSSL

### Generating self-signed SSL certificates to be used in Nginx

The same command works on [macOS X](../macOS+X/). 

``` bash
openssl req -x509 -nodes -days 36500 -newkey rsa:2048 \
 -keyout private-selfsigned.key -out public-selfsigned.crt
```

### Discovering the sha256 of a file using openssl:
```bash
openssl sha256 OperaSetup.zip
```

###  Converting a certificate from/to DER and PEM formats

`.cer`: binary DER format

`.crt`: base64 PEM format

``` bash 
openssl x509 -inform DER -in certificate.cer -outform PEM -out certificate.crt
```
or 
``` bash
openssl x509 -inform PEM -in certificate.crt -outform DER -out certificate.cer
```