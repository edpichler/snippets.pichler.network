---
title: "Linux"
weight: 1
# bookFlatSection: true
# bookToc: false
# bookHidden: true
# bookCollapseSection: true
# bookComments: false
# bookSearchExclude: false
---

## Automatically confirming a command in Linux

``` bash
yes | ./script
```

In crontab you can do something like:

``` bash
@monthly yes | ./script
```
## OpenSSL

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

## GPG (GNU Privacy Guard)

### Generate a gpg key pair
```bash
gpg --full-generate-key
```

### List private and public keys
```bash
gpg --list-secret-keys
gpg --list-keys
```

### Export GPG keys
 ``` bash
gpg --output private-key.pgp --armor --export-secret-key  6D934E6918FF79E0EE82CA93BF6F8ADD7DDC0A44D
gpg --output public-key.pgp --armor --export  6349BE6918FF79E0EE82CA93BF6F8ADD7DDC0A44D
```

### Delete private and public keys
```bash
gpg --delete-secret-key "User Name"
gpg --delete-key "User Name"
```