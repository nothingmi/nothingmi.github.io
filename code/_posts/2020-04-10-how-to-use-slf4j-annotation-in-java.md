---
layout: post
title: How to use Slf4j Annotation in Java
description: Reduce boring codes and make your work more efficient than before on logging.
image: /assets/2020-04-10-how-to-use-slf4j-annotation-in-java/banner.jpg
categories:
    - code
tags:
    - java
    - slf4j
---

## Introduction

[Slf4j](http://www.slf4j.org/) is widely used in Java world for log control. Generally, developers declare a logger variable mannually in their Java classes. But today, I will show you another way to manage a logger object with less code than before.

## 1. Create a new folder as the root directory of your Java project

Run the following command in your prompt.

```shell
mkdir gradle-slf4j-annotation && cd gradle-slf4j-annotation
```

## 2. Initialize your Java project

*I would like to use [Gradle](https://gradle.org/) here to start an fresh new Java project. And developers can use [Maven](https://maven.apache.org/) instead.*

Run the following commands in your prompt, and several questions will guide you to the final. That's why I like Gradle, it's so clear and interactive.

```shell
$ gradle init

Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4] 2

Select implementation language:
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Swift
Enter selection (default: Java) [1..5] 3

Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Groovy) [1..2] 1

Select test framework:
  1: JUnit 4
  2: TestNG
  3: Spock
  4: JUnit Jupiter
Enter selection (default: JUnit 4) [1..4] 4

Project name (default: gradle-slf4j-annotation):
Source package (default: gradle.slf4j.annotation):

> Task :init
Get more help with your project: https://docs.gradle.org/6.2.2/userguide/tutorial_java_projects.html

BUILD SUCCESSFUL in 15s
2 actionable tasks: 2 executed
```

## 3. Change Repository Center to Maven

Open *build.gradle* file, and update *repositories* block, like this:

```groovy
repositories {
    // Use jcenter for resolving dependencies.
    // You can declare any Maven/Ivy/file repository here.
    mavenCentral()
}
```

## 4. Import Lombok library

Slf4J Annotation is a part of [Lombok](https://projectlombok.org/), which is widely used for generating getter and setter methods to a Java class.

Open *build.gradle* file, and add the Lombok implementation statement in *dependencies* block, like this:

```groovy
dependencies {
    // ...
    compileOnly 'org.projectlombok:lombok:1.18.12'
    annotationProcessor 'org.projectlombok:lombok:1.18.12'

    testCompileOnly 'org.projectlombok:lombok:1.18.12'
    testAnnotationProcessor 'org.projectlombok:lombok:1.18.12'
    // ...
}
```

## 5. Import Slf4j Simple Implementation Library

Slf4j has three official wrapped implementations, they are JDK14, Log4j, and Simple. I use Simple here to explain the basic usage.

Open *build.gradle* file, and add the Slf4j Simple implementation statement in *dependencies* block, like this:

```groovy
dependecies {
    // ...
    implementation 'org.slf4j:slf4j-simple:1.7.30'
    // ...
}
```

## 6. Use Slf4j Annotation

Open *App.java* and add *@Slf4j* annotation in *App* class, like this:

```java
/*
 * This Java source file was generated by the Gradle 'init' task.
 */
package gradle.slf4j.annotation;

@Slf4j
public class App {

    // ...
}
```

A class with Slf4j annotation means a static member variable named *log* being declared. Therefore the annotation will generate:

```java
/*
 * This Java source file was generated by the Gradle 'init' task.
 */
package gradle.slf4j.annotation;

public class App {

    private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(App.class);

    // ...
}
```

## 7. Output log with Slf4j

Update *main* method in *App.java*, like this:

```java
/*
 * This Java source file was generated by the Gradle 'init' task.
 */
package gradle.slf4j.annotation;

import lombok.extern.slf4j.Slf4j;

@Slf4j
public class App {

    public String getGreeting() {
        return "Hello world.";
    }

    public static void main(String[] args) {
        log.info(new App().getGreeting());
    }
}
```

This application will output *Hello, World!* to console when it runs.

## 8. Run the Application

Run the following command in your promptt.

```shell
$ ./gradlew clean build run

> Task :run
[main] INFO gradle.slf4j.annotation.App - Hello world.

BUILD SUCCESSFUL in 2s
9 actionable tasks: 9 executed
```

A log has been printed in Your console. And you can custom your slf4j configurations as before.

**Note:** *Slf4j** annotation provide another parameter named *topic*. It means the category of the constructed Logger. By default, it will use the type where the annotation is placed.

## Conclusion

It's awesome. You never need to write boring codes to declare a logger instance. Annotation is a great feature in Java, more and more third-party libraries have their annotations to reduce boring codes.
