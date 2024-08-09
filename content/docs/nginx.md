
## How to watch nginx logs
``` bash
#access logs
sudo tail -f /var/log/nginx/access.log

#error logs
sudo tail -f /var/log/nginx/error.log
```

## Script to create self signed certificates for Nginx

``` zsh
#!/bin/bash
echo "Generating an SSL private key to sign your certificate..."
openssl genrsa -des3 -out private.key 1024

echo "Generating a Certificate Signing Request..."
openssl req -new -key private.key -out myssl.csr

echo "Removing passphrase from key (for nginx)..."
cp private.key private.key.old
openssl rsa -in private.key.old -out private.key
rm private.key.old

echo "Generating certificate..."
openssl x509 -req -days 36500 -in myssl.csr -signkey private.key -out self-signed-public.crt

echo "Copying certificate (myssl.crt) to /etc/ssl/certs/"
mkdir -p  /etc/ssl/certs
cp self-signed-public.crt /etc/ssl/certs/

echo "Copying key (myssl.key) to /etc/ssl/private/"
mkdir -p  /etc/ssl/private
cp private.key /etc/ssl/private/
```
Source: https://gist.github.com/edpichler/d7daa0c26b18dca6dc49f65f8d51274c#file-gistfile1-sh