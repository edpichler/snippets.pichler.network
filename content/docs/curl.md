---
title: "cURL"
weight: 5
# bookFlatSection: true
# bookToc: false
# bookHidden: true
# bookCollapseSection: true
# bookComments: false
# bookSearchExclude: false
---
## Check if a website contains a text using cURL
``` bash
if curl -L -s --compressed https://snippets.pichler.network  | grep 'docker'; \
    then echo "Website is up."; \
    else echo "Website is down."; exit -1; fi
```