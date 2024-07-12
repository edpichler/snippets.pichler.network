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

I consider git on of the most important software ever created. It enabled people to work geographically distributed even without internet connection, with automated backups (every repo is potentially a repository backup). Because of this little tool, software could evolve faster than ever. Yes, we had other version control systems before git, but only with git (and mercurial) we could work efficiently remotelly and OFFLINE.

## To update the local list (cache) of remote branches

``` bash
# To show all local and remote branches that (local) Git knows about:
git branch -a
# And to update it:
git remote update origin --prune
# Now you can see everything up to date with the origin
git branch -a

```
Or you can also do a `git pull --all` to fetch from origin and update all your local branches.
 
## git patch

To generate a patch from the last 10 commits:
``` bash
git format-patch -10 --stdout > mypatch.patch
```
Creat a patch from stashed changes:
```
git stash list
git stash show -p stash@{<number>} > mypatch.patch
```

From uncommited changes:
```git diff > mypatch.patch```

To apply a patch:
```git apply mypatch.patch```

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
If you don't have you can [generate one with GPG](/docs/linux/#generate-a-gpg-key-pair).

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

## Signing your git commits

Congigure the key to be used:
`git config --global user.signingkey EUUU75A0F4CD1D98FC863AAB9AFEA220BB696AA1`

Do `git config --local commit.gpgsign true` in your repository.

## Pushing automatically after a commit
You need to have an executable (chmod +x) file in .git/hooks/post-commit that contains the following:
```
#!/bin/sh
git push origin master
```
You can create it by doing:
``` sh
echo "#!/bin/sh" >> .git/hooks/post-commit
echo "git push origin master" >> .git/hooks/post-commit
```

## Software archeology with git

### See the author names

``` bash
git shortlog -sne | cut -f2 | sort 
```

### Count the number of commits per file

Files with many commits can often, but not always, point to a design problem.

``` bash
git log --no-merges --no-renames --numstat --pretty=format:"" -- **/*.java | cut -d$'\t' -f3 | grep -v '^$' | sort | uniq -c | sort
```

### Search for commit activity per author

``` bash
git shortlog -ns -- **/*.java
```
Or, better:
``` bash 
git shortlog -ns .  --since "5 month ago" 
```

### Search for awkward things

Todos and fixmes:

``` bash
git grep --perl-regexp "\/\/ *(todo|fixme)"
``` 

Commented code:

``` bash
git grep --perl-regexp " \/\/.*(=|;)" -- *.java

```

### Browse through the commit messages

``` bash
git log --pretty=format:"%h %s"
```
### Count last changed line in each Java source code file per author

``` bash
find . -type d -name ".git" -prune -o -type f \( -iname "*.java" \) | xargs -n1  git blame -w -f -C -M  --date=format:"|" | cut -d"|" -f 1 | cut -d"(" -f 2 | sed 's/\s*$//' | sort | uniq -c | sort

```
### Search in the commit content
``` bash
 git rev-list --all | xargs git grep \.stream\(\)
``` 
 Or:

``` bash
 git grep stream\. $(git rev-list --all)
```
In `git grep` you can have the line number by adding `-n`.

Or :
```
git log -Gstream
```
You can add a `-p` flag:
```
git log -p -Gstream
```

See more ways in [this forum](stackoverflow.com/questions/2928584/how-to-grep-search-through-committed-code-in-the-git-history)

### Limiting to a subtree, for instance lib/util. Notice you need to pass it in both commands:

 ``` bash
 git grep <regexp> $(git rev-list --all -- lib/util) -- lib/util
```

### Search working tree for text matching regular expression regexp:

``` bash 
git grep <regexp>
```

### Search for a pattern only in the changed lines

``` bash
git diff --unified=0 | grep <pattern>
```

### Search all revisions for text matching regular expression regexp:

``` bash
git grep \.stream\(\) $(git rev-list --all)
```

### All the branches that contain a commit

``` bash
git branch -a --contains 8beeff00d
```

### Getting more information about a commit
You can then get more information like author, date, and diff using git show:

``` bash
git show 6988bec26b1503d45eb0b2e8a4364afb87dde7af
```
### All commits that cointain a regex
 
 ``` bash
 git log --author=edu --pretty -G \.stream\(\)
 ```

 ``` bash
 git log --author=edu --pretty -G \.stream\(\) -- **/*Test.java
 ```
 
 ``` bash
 git log --author=edu --pretty -G \.stream\(\)
 ```
 