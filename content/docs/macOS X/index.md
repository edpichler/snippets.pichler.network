---
title: "macOS X"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
## Switching between different installed Java versions in macOS X
Once you have several java versions installed, you can configure your `.bash_profile` to have the following lines:

``` bash
alias j19="export JAVA_HOME=`/usr/libexec/java_home -v 19`;  java -version"
alias j17="export JAVA_HOME=`/usr/libexec/java_home -v 17`;  java -version"
alias j16="export JAVA_HOME=`/usr/libexec/java_home -v 16`;  java -version"
alias j12="export JAVA_HOME=`/usr/libexec/java_home -v 12`;  java -version"
alias j11="export JAVA_HOME=`/usr/libexec/java_home -v 11`;  java -version"
alias j10="export JAVA_HOME=`/usr/libexec/java_home -v 10`;  java -version"
alias j9="export JAVA_HOME=`/usr/libexec/java_home  -v 9`;   java -version"
alias j8="export JAVA_HOME=`/usr/libexec/java_home  -v 1.8`; java -version"
alias j7="export JAVA_HOME=`/usr/libexec/java_home  -v 1.7`; java -version"
```

## Generating self-signed SSL certificates to be used in Nginx
The same command works on [Linux](../linux/).

``` bash
openssl req -x509 -nodes -days 36500 -newkey rsa:2048  \ 
  -keyout private-selfsigned.key -out public-selfsigned.crt
```

## Discover the IPs that are being resolved for a domain
``` bash
nslookup google.se
```

## Creating a brew PR to upgrade a version
Let's take Opera as an example. The Opera's [tap is here](https://github.com/Homebrew/homebrew-cask/blob/HEAD/Casks/opera.rb).

To upgrade brew casks:
``` bash
brew bump-cask-pr opera --version 77.0.4054.276
```

And for regular formulas:
``` bash
brew bump-formula-pr  ...
```

## Creating permanent aliases
Create a file like ~/.zsh_aliases
``` bash
cd; touch .zsh_aliases
```
Add alias on it;
``` bash
echo "myalias=\"ls\"" >> .zsh_aliases
```

Reference the alias in your bash profile by adding the follow lines.
``` bash
if [ -f ~/.zsh_aliases ]; then
. ~/.zsh_aliases
else 
  touch .zsh_aliases;
  echo echo "myalias=\"ls\"" >> .zsh_aliases;
fi
```