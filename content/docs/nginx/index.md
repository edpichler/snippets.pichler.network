---
title: "Nginx"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
## How to watch nginx logs
``` bash
#access logs
sudo tail -f /var/log/nginx/access.log

#error logs
sudo tail -f /var/log/nginx/error.log
```