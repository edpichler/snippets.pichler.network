---
title: "Git"
weight: 2
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Git

Git is one of the most important software ever created. It enabled people to work geographically distributed even with intermittent Internet connection. Each contributor has a copy of the repository including the historical changes. Software evolved faster than ever.


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

 ## Changing the author of the last commit
 ``` bash
 git commit --amend --author="Eduardo Ivan Pichler <eduardo.pichler@myemail.com>" --no-edit
 ```

 ## Changing the Author for global and local repositories
 ``` bash
git config --global user.name "João"
git config --global user.email "joao@myemail.com"
```
It can also be `local` instead of `global`.

## git commit

### Signing your commits

Congigure the key to be used:
`git config --global user.signingkey EUUU75A0F4CD1D98FC863AAB9AFEA220BB696AA1`

Do `git config --local commit.gpgsign true` in your repository.

## git push

### Auto setup the remote branches
```
git config --global 'push.autoSetupRemote' true    
```

### Pushing automatically after a commit

You need to have an executable (chmod +x) file in .git/hooks/post-commit that contains the following:

```
#!/bin/sh
git push origin master
```
You can create it by doing:
```
FILE=".git/hooks/post-commit"
if [ ! -f $FILE ]; then
  touch $FILE
  echo '#!/bin/sh' | tee $FILE 
fi
echo 'git push origin master' | tee -a $FILE
```

## git log

Software archeology with git.

| Command                                                                                                                                    | Description                                                                                                         |
|--------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| `git shortlog -sne \| cut -f2 \| sort`                                                                                                     | Shows the authors of the commits                                                                                    |
| `git log --oneline`                                                                                                                        | Shows the commit hash and the commit message                                                                        |
| `git log --oneline --author=carlos `                                                                                                       | Shows the commits of a specific author                                                                              | 
| `git log --stat`                                                                                                                           | Shows the commit hash and the files changed                                                                         |
| `git log --oneline --graph --all --decorate`                                                                                               | Shows the commit hash and the branches                                                                              |
| `git log --oneline --graph --all --decorate --simplify-by-decoration`                                                                      | Shows the commit hash and the branches simplified                                                                   |
| `git log --no-merges --no-renames --numstat --pretty=format:"" -- **/*.java \| cut -d$'\t' -f3 \| grep -v '^$' \| sort \| uniq -c \| sort` | Count the number of commits per file. Files with many commits can often, but not always, point to a design problem. | 
| `git shortlog -ns -- **/*.java`                                                                                                            | Shows the number of commits per author in a specific file                                                           |  
| `git shortlog -ns .  --since "5 month ago"`                                                                                                | Shows the number of commits per author in the last 5 months                                                         |
| `git grep --perl-regexp "\/\/ *(todo\| fixme)"`                                                                                            | Shows the todos and fixmes in the code                                                                              |
| `git log --pretty=format:"%h %s"`                                                                                                          | Browse through the commit messages                                                                                  | 


Commented code:

``` bash
git grep --perl-regexp " \/\/.*(=|;)" -- *.java

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
 