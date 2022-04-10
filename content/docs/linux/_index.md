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
 ```bash
gpg --output private-key.pgp --armor --export-secret-key  6D934E6918FF79E0EE82CA93BF6F8ADD7DDC0A44D
gpg --output public-key.pgp --armor --export  6349BE6918FF79E0EE82CA93BF6F8ADD7DDC0A44D
```
Or, export a key `base64`, and copy it to the clipboard:

```bash
gpg --export-secret-key 6D9BE6918FF79E0EE82CA93BF6F8A234DDC0A44D  > mykey.txt; cat otrust.txt | pbcopy; rm mykey.txt
```

### Delete private and public keys
```bash
gpg --delete-secret-key "User Name"
gpg --delete-key "User Name"
```

## Finding things in Linux
### The `locate` command

This command will go through your entire filesystem and locate every occurrence of that keyword.

``` bash
locate keyword
```
the `locate` uses a database that is usually updated once a day. If you don't find your file you can update the database manually and try again.

``` bash
 updatedb
 locate keyword
```

### The `which` command
The which command locates an a binary in your PATH. If it doesn’t find the binary in the current PATH, it returns nothing.
``` bash 
which java
```

### The find command

You can search in any designated directory and use a variety of parameters.
``` bash
find directory options expression
```
``` bash
find / -type f -name test.txt
```
It also accept wildcards
``` sh
find /home -type f -name "test.*"
```
Find it by name and file size less than 5 kb
```sh
find . -type f  -size -5k  -name "*.txt"
```

- `*` matches multiple characters *at would match: cat, hat, what, and bat.
- `?` matches a single character ?at would match cat, hat, bat but not what.
- `[]` matches character that appear inside the square brackets [c,b] would match cat and bat

### Find content inside files using grep

``` bash
 cat mylogs.log | grep --line-buffered 'content I want'
```
In case you don't have the `--line-buffered` option: 
``` bash
 cat mylogs.log | stdbuf -oL grep 'content I want'
```

## Configuring the Linux swap

``` bash
sudo fallocate -l 4G /swapfile1
sudo chmod 600 /swapfile1
sudo mkswap /swapfile1

#Only if you want to make the swap persist after a reboot
sudo echo "/swapfile1   none    swap    sw    0   0" >>  /etc/fstab
sudo echo "vm.swappiness=10" >> /etc/sysctl.conf
sudo echo "vm.vfs_cache_pressure=50" >> /etc/sysctl.conf

# turn on the swap
sudo swapon /swapfile1

# optional, check if it's working
htop
```

## To count the number of files in a linux folder
``` bash
find . -type f | wc -l
```

