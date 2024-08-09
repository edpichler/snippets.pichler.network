
## How to watch nginx logs
``` bash
#access logs
sudo tail -f /var/log/nginx/access.log

#error logs
sudo tail -f /var/log/nginx/error.log
```

## Script to create self signed certificates for Nginx
<script src="https://gist.github.com/edpichler/d7daa0c26b18dca6dc49f65f8d51274c.js"></script>