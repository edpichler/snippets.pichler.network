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

## Basic kubectl commands
| Command                                                                             |                                                      |
|-------------------------------------------------------------------------------------|------------------------------------------------------|
| `kubectl get pods`                                                                  | List all pods in the current namespace               |
| `kubectl get pods --all-namespaces`                                                 | List all pods in all namespaces                      |
| `kubectl get pods -n trust-manager`                                                 | List all pods in the namespace `trust-manager`       |
| `kubectl get pods -o wide`                                                          | List all pods with more details                      |
| `kubectl get pods -o wide -n trust-manager`                                         | List all pods in the namespace `trust-manager`       |
| `kubectl describe pod trust-manager`                                                | Describe a pod with the name `trust-manager`         |
| `kubectl get events --field-selector involvedObject.name=<pod-name> -n <namespace>` | Get events for a specific pod                        |
| `kubectl get events -n <namespace>`                                                 | Get events for a specific namespace                  |
| `kubectl get events --all-namespaces`                                               | Get events for all namespaces                        |

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