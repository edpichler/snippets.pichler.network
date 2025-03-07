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

## git branch

| Command          | Description                |
|------------------|----------------------------|
| `git branch -r`  | Lists the remote branches. |

### Batch renaming branches

Adding a prefix:
```
REGEX='^.*noble.*'
NEW_PREFIX='old/'
DRY_RUN=true

for branch in $(git branch | grep -E "$REGEX"); do
  new_branch_name="${NEW_PREFIX}${branch}"
  if [ "$DRY_RUN" = false ]; then
    echo "Renaming to $new_branch_name..."
    git branch -m $branch $new_branch_name
  else
    echo "Dry run: git branch -m $branch $new_branch_name"
  fi
done
```

## git checkout

| Command                                                                                        | Description                                    |
|------------------------------------------------------------------------------------------------|------------------------------------------------|
| `git checkout -b origin/<branch-name>` or `git checkout -b <branch-name> origin/<branch-name>` | Checks out a branch from the origin.           |
| `git checkout -b <branch-name> my-tag`                                                         | Creates a new branch from a tag.               |

## git patch

| Command                                                              | Description                                    |
|----------------------------------------------------------------------|------------------------------------------------|
| `git format-patch -10 --stdout > mypatch.patch`                      | Creates a patch file with the last 10 commits. |
| `git format-patch -1 --stdout <commit-hash> mypatch.patch`           | Creates a patch file with the commit.          |
| `git apply mypatch.patch`                                            | Applies the patch file.                        |
| `git apply --check mypatch.patch`                                    | Checks if the patch can be applied.            |
| `git stash list; git stash show -p stash@{<number>} > mypatch.patch` | Creates a patch file from a stash.             |
| `git diff > mypatch.patch`                                           | Creates a patch file from uncommited changes.  |

## git stash

Everything is stashed in a stack. To reference it you can do `stash@{n}` where n is the offset in the stack.

To stash it you can do:

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
| `git log --author=edu --pretty -G \.stream\(\)`                                                                                            | Shows the commits of a specific author that contains a regex.                                                       |
| `git log --author=edu --pretty -G \.stream\(\) -- **/*Test.java`                                                                           | Shows the commits of a specific author that contains a regex filtering by specific files.                           |

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
## git rebasing

| Command                                                      | Description                                                                |
|--------------------------------------------------------------|----------------------------------------------------------------------------|
| `git fetch && git fetch origin tag my-tag --no-tags --force` | Fetch the my-tag from the origin                                           |
| `git rebase my-tag; git checkout -b feature/branch-name `    | Rebase the current branch from the my-tag and create a new branch from it. |
| `git fetch; git rebase my-tag`                               | Rebase the current branch from the my-tag                                  |


## git branch
| Command                                                                                                   | Description                                                            |
|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| `git branch -a`                                                                                           | Shows all the local and remote branches  that the local git know about |
| `git branch -a --contains 8beeff00d`                                                                      | Shows the branches that contain a commit                               |
| `git branch -d branch_name`                                                                               | Deletes a branch                                                       |
| `git branch -m old_branch new_branch`                                                                     | Renames a branch                                                       |
| `git branch -vv`                                                                                          | Shows the tracking branches                                            |

### Renaming branches

``` bash
preffix='old'
new_prefix='new_preffix'
for branch in $(git branch | grep "^  $preffix"); do
  new_branch_name=$(echo $branch | sed "s/^$preffix/$new_prefix/")
  echo "Renaming to $new_branch_name..."
  # git branch -m $branch $new_branch_name
done
```

## git show
| Command                                                                                                    | Description                     |
|------------------------------------------------------------------------------------------------------------|---------------------------------|
| `git show 6988bec26b1503d45eb0b2e8a4364afb87dde7af`                                                        | Shows the details of a commit   |

## git filter-branch

Rewriting branches history.

| Command                                                                                                    | Description                                                 |
|------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| `git filter-branch --tree-filter 'rm -rf terraform/*.tfstate'`                                             | Remove all the `.tfstate` files from the repository history |

## git rm

| Command                                                                                                    | Description                                                 |
|------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
|`git rm -r –cached folder`                                                                                  | Removing a file whitout deleting it locally.                |
