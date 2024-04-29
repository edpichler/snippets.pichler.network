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

## Creating dirs recursively in Linux

`mkdir -p foo/bar/zoo/my\ dir\ with\ spaces/edu`

## Environment variables

Load from a local `.env` file:

`alias loadenv='export $(xargs <.env)'`

### Using tools (dotenv)

https://github.com/gyf304/dotenv
https://github.com/dotenvx/dotenvx
https://github.com/ko1nksm/shdotenv


## **Pottentially** automatically confirming a command in Linux

``` bash
yes | ./script.sh
```

In crontab you can do something like:

``` bash
@monthly yes | ./script.sh
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

### Generate a subkey
```bash
gpg --pinentry-mode=loopback --passphrase="" --quick-addkey 690E75A0F4CD1D98FC86234AAB9AFEA220BB696234  rsa4096 sign 0
```
Explanation:

The --quick-add-key option takes between 1 and 4 space-delimited arguments:
 - fpr: the "fingerprint" of existing private key
 - algo: the desired algorithm for the new subkey
 - usage: the desired usage kind for the new subkey
 - expire: the desired expiration date or duration for the new subkey

The --passphrase="" disables passphrase protection on the new subkey.

The --pinentry-mode=loopback must (since version 2.1) be used in conjunction with --passphrase to avoid the interactive prompt.

Docs: https://manpages.debian.org/bookworm/gpg/gpg.1.en.html#quick-add-key

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
### Change the GPG key password
```bash
gpg --edit-key Your-Key-ID-Here
gpg> passwd
gpg> save
```
### Delete private and public keys
```bash
gpg --delete-secret-key "User Name"
gpg --delete-key "User Name"
```

### GPG keys managers

Two softwares to manage GPG keys: https://www.gpg4win.org/ and https://gpgtools.org/ and [kleopatra](https://www.openpgp.org/software/kleopatra/).

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

## Execute a command in loop in shell/bash on Linux
```bash 
for i in {1..10}; do
	echo "Hello Friend"
done
```

## SSH

### Authorizing a public key to connect into your server using ssh
Just add the public key in .ssh/authorized_keys and refresh the ssh settings:
 
 ``` 
 sudo echo "my long key content.." >> ~/.ssh/authorized_keys
 
 service sshd reload
 ```
 Or use the command `ssh-copy-id` from the client.

 ## Rsync

### Copy files and folders, recursively, to another folder
 
``` bash
rsync -av /home/tin/sample /home/tin/test`
``` 

## Samba

### Sharing a folder with the Windows network protocol:

Edit /etc/samba/smb.conf and add:

```
[global]
  allow insecure wide links = yes  #global configuration to allow sharing the content of any folder where a symlink is pointing to
  #client min protocol = SMB2
  #client max protocol = SMB3
  protocol = SMB3 # to force a protocol version
  #source https://www.cyberciti.biz/faq/how-to-configure-samba-to-use-smbv2-and-disable-smbv1-on-linux-or-unix/
  workgroup = SAMBA
  security = user
  passdb backend = tdbsam


[some_name]
  path = /media/my_shared_folder
  writeable=Yes
  create mask=0770
  directory mask=0770
  public=no
  follow symlinks=yes #in case you want to share the content of the symlinks
  wide links=yes      #in case you want to share the content of the symlinks that the destination is outside of the shared folder too
  security=user
  valid users=pi
``` 

Then restart the service:

```
sudo systemctl restart smbd.service
```
### Testing if samba is working

```
smbclient -L localhost
```

Check logs:
```
sudo systemctl status smbd.service
```

To setup the firewall:
```
sudo ufw allow from 192.168.0.0/16 to any app Samba
```
or
```
sudo ufw allow Samba
```
Then you can edit your ufw with `sudo ufw status numbered` and `sudo ufw delete #`.

## Find duplicate files in Windows and Linux

*Tip*: Sometimes is better to create hardlinks instead of deleting the duplicated files. Hardlinks and symlinks almost does not use space.

Generate a list of duplicated files:

`rdfind -makeresultsfile true .` or  `fdupes -R .` does the job.

`rfind` can replace the duplicated files with hard and symbolic links. The flag is `rdfind -makehardlinks true .` I tested and it worked in both, Windows and Linux.

And, the flag `-makeresultsfile true` is self explaining. 

## Changing permissions in files and directories

Per example, to change directory permissions:

``` bash
find [YOURDRIVEPATH] -type d -exec chmod 2755 {} \;
```

To change permissions in files:

``` bash
find [YOURDRIVEPATH] -type f -exec chmod 0644 {} \;
```

The `2755` permission scheme allows the owner to read, write, and execute, the group to read and execute, and others to read and execute. We set this on directories because "executing" a directory means searching it. The `2` sets the setgid permission, so new files and directories created get the same group ownership as the directory they're created in.

The `0644` permission scheme allows the owner to read and write, and everyone else just read. The `0` unsets the setgid permission if it was set.

Reference: [Reddit](https://www.reddit.com/r/jellyfin/comments/nzcv6f/grant_the_service_user_at_least_read_access/)

## Tar and Gzip

### backing up a folder
The command in an alias:
```bash
alias backup-myapp='tar -C /opt/myapp/bin/ -cvf /opt/myapp/bin_$(date "+%Y-%m-%d__%H_%M_%S").tar ./; ls /opt/myapp'
```

### Printing the contents of a tar file

``` bash
tar -tf file.tar 
```

Or you can see it with vim:

``` bash
vim file.tar
```

## Grep

### Find a string in files recursively with grep:
``` sh
grep -ir --include "*.cpp" "xyz" .
```
### Grep with regex and shorter lines

Example:
```
grep -oE '.{70}\.java.{0,100}'
```

## Find who logged in since the last reboot
`last`