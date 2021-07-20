---
title: "Gradle"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
# bookSearchExclude: false
---

## To have nice test logs in Gradle

``` groovy
plugins {
    id 'com.adarshr.test-logger' version '3.0.0'
}
```
Documentation: https://github.com/radarsh/gradle-test-logger-plugin

## To run any version of gradle using a docker container
It facilitates running gradle from a linux environment. See bellow two examples combining different versions of [Java](../java/) and [Gradle](../gradle/):
- `docker run --rm -u gradle -v "$PWD":/home/gradle/project -w /home/gradle/project gradle:6-jdk8 gradle test`
- `docker run --rm -u gradle -v "$PWD":/home/gradle/project -w /home/gradle/project gradle:7-jdk16 gradle test`