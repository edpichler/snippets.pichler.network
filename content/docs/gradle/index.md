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
``` bash
# Gradle 6 and Java 8
docker run --rm -u gradle -v "$PWD":/home/gradle/project \ 
  -w /home/gradle/project gradle:6-jdk8 gradle test
# Gradle 7 and Java 16
docker run --rm -u gradle -v "$PWD":/home/gradle/project \
  -w /home/gradle/project gradle:7-jdk16 gradle test
```

## How to make the gradle test runner fail fast
``` bash
./gradlew test --fail-fast
```

## Skipping tests (or any other task)
``` bash
./gradlew build -x test
```

## Filtering tests in gradle
``` bash
 ./gradle test --tests "*SubscriptionTest*"

```

# Gradle Enterprise
## Debugging cache operations when using the Gradle Maven Extension
And also debug to find out why a cache was missed:

```
mvn clean verify -Dorg.slf4j.simpleLogger.log.gradle.goal.cache=debug  -Dgradle.scan.captureGoalInputFiles=true
```
Source: https://docs.gradle.com/enterprise/maven-extension/
