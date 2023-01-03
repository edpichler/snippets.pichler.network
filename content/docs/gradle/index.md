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
## Integrate gradle with
When you have a maven project with the `maven-jar-compiler`plugin configured to publish the test folder (type `test-jar`), maven will publish the production jar and also the test-jar. You can import the test-jar  using a Gradle classifier. Example:

``` groovy
testImplementation 'com.company:library-common:1.0.0:tests'
```
So, there will be `library-common-1.0.0.jar` and `library-common-1.0.0-tests.jar` in the repository available to gradle, and it will use the one specified in the classifier.

## Excluding transitive dependencies

From entire classpath:

``` kotlin
configurations {
    all {
        exclude("org.apache.logging.log4j","log4j-1.2-api")
    }
}
```

From the testImplementation classpath:

``` kotlin
configurations {
    testImplementation {
        exclude("org.apache.logging.log4j","log4j-1.2-api")
    }
}
```

From the added dependency itself:

``` kotlin
api(project(":myproject")) {
    exclude("org.apache.logging.log4j","log4j-1.2-api")
}
```

# Gradle Enterprise
## Debugging cache operations when using the Gradle Maven Extension
And also debug to find out why a cache was missed:

```
mvn clean verify -Dorg.slf4j.simpleLogger.log.gradle.goal.cache=debug  -Dgradle.scan.captureGoalInputFiles=true
```
Source: https://docs.gradle.com/enterprise/maven-extension/
