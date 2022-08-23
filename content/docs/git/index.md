---
title: "Git"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Git

I consider git on of the most important software ever created. It enabled people to work geographically distributed even without internet connection, with automated backups (every repo is potentially a repository backup). Because of git, software could evolve faster than ever.

## To update the local list (cache) of remote branches

``` bash
# To show all local and remote branches that (local) Git knows about:
git branch -a
# And to update it:
git remote update origin --prune
# Now you can see everything up to date with the origin
git branch -a

```
 
## git patch

To generate a patch from the last 10 commits:
``` bash
git format-patch -10 --stdout > patch-ddmmyyy.patch
```

## git stash

Everything is stashed in a stack. To reference it you can do `stash@{n}` where n is the offset in the stack.

To stash you can do: 

``` bash
# stashes everything 
git stash
# list what is stashed
git stash list
# apply the most recent stash and delete it
git stash pop # it's the same as doing git stash pop stash@{0}
# apply the second most recent stash and delete it
git stash pop stash@{1}
# apply the stashed content and does not delete it like pop does
git stash apply # it's the same as doing git stash apply stash@{0}
# you can set a description to stashed content
git stash save "message"
# you can view a summary of a stash
git stash show
# or to show the full diff
git stash show -p 
```

### Creating a branch from a stash


``` bash
git stash branch add-stylesheet stash@{1}
```

#### Cleaning up the stash

```  bash
git stash clear 
#or 
git stash drop # it will drop the stash@{0}
```

## git-crypt
Git crypt is just awesome. **Not perfect**, but awesome. It is very useful, despite of its limitations.

### Before starting it

Make sure you have the public gpg key installed:
``` bash
# List gpg keys
gpg --list-keys 

# List gpg secret keys
gpg --list-secret-keys 

```

### Importing keybase keys to gpg
``` bash
# import keybase keys to gpg
keybase pgp export | gpg --import

# import private keys to gpg
keybase pgp export -s | gpg --allow-secret-key-import --import

```

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

 ## Changing the author of the last commit
 ``` bash
 git commit --amend --author="Eduardo Ivan Pichler <eduardo.pichler@myemail.com>" --no-edit
 ```

 ## Changing the Author for global and local repositories
 ``` bash
git config --global user.name "Eduardo Ivan Pichler"
git config --global user.email "eduardo.pichler@myemail.com"
 ```
 Or: 
  ``` bash
git config --local user.name "Eduardo Ivan Pichler"
git config --local user.email "eduardo.pichler@myemail.com"
 ```