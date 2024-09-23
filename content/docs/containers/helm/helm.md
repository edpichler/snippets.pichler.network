---
title: "Helm"
weight: 2
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Helm [&#128279;](https://helm.sh/) 
The package manager for Kubernetes. 

## basic helm commands

| Command                                |                                                             |
|----------------------------------------|-------------------------------------------------------------|
| `helm search repo nginx`               | Search for a chart (nginx in this case) in the repositories |
| `helm install my-nginx nginx/nginx`    | Install a chart.                                            |
| `helm repo update ngix`                | Update the repository cache                                 |
| `helm search repo nginx --versions`    | List the versions of a chart                                |

## helm show


| Command                                          |       |
|--------------------------------------------------|-------|
| `helm show values jetstack/cert-manager \| vim`  |       |