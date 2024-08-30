---
title: "Kubernetes"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Kubernetes [&#128279;](http://kubernetes.io/)


## Namespaces
| Command                                      |                                                      |
|----------------------------------------------|------------------------------------------------------|
| `kubectl create namespace trust-manager`     | Create a new namespace with the name `trust-manager` |
| `kubectl delete namespace trust-manager`     | Delete a namespace with its resources altogether     |
| `kubectl get namespaces`                     | List all namespaces                                  |

## Config maps
| Command                                     |                                                       |
|---------------------------------------------|-------------------------------------------------------|
| `kubectl get configmaps --all-namespaces`   | List all config maps in all namespaces                |
| `kubectl delete configmap custom-ca-bundle` | Delete a config map with the name `custom-ca-bundle`  |