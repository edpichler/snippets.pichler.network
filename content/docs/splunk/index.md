---
title: "Splunk"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Splunk snippets collection 
## Searching in Splunk with table and regex
```bash
index=*  sourcetype=*  "*Failed to*" 
| rex ".*Failed to(?<groupName1>.{10})(?<groupName2>.{10}).*" 
| table timestamp, groupName1, groupName2, message
```