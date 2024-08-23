---
title: "git-crypt"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# git-crypt
Git crypt is a great tool, even though not perfect.

### Before starting it

Make sure you have the public gpg key installed:
``` bash
# List gpg keys
gpg --list-keys 

# List gpg secret keys
gpg --list-secret-keys 

```
If you don't have you can [generate one with GPG](/docs/shell_unix/#generating-a-gpg-key-pair).

### Encrypting a git repository folder

``` bash
#initiate it
git-crypt init

#add the public key of the user
git-crypt add-gpg-user 5A3700C672440657ACF09DEFB146A056E9BACD36
```

Configuring the secret files:

In the `.gitattributes` file you must do:

``` text
# Example 
secretfile filter=git-crypt diff=git-crypt
*.key filter=git-crypt diff=git-crypt
secretdir/** filter=git-crypt diff=git-crypt

src/main/resources/*.yml filter=git-crypt diff=git-crypt
```
### Showing encrypted files
 ``` bash
 git-crypt status -e
 ```