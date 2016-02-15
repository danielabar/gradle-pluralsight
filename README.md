<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Gradle course with Pluralsight](#gradle-course-with-pluralsight)
  - [Introduction](#introduction)
    - [What is Gradle](#what-is-gradle)
    - [Install](#install)
    - [Gradle Builds](#gradle-builds)
      - [Hello Gradle](#hello-gradle)
      - [Task Lifecycle](#task-lifecycle)
      - [Build Java](#build-java)
    - [Gradle Wrapper](#gradle-wrapper)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Gradle course with Pluralsight

## Introduction

### What is Gradle

* Build by convention
* Written in Groovy, it's a Groovy DSL
* Dependency management with Ivy or Maven
* Supports multi-project builds
* Can build things other than Java projects such as JavaScript or C++
* Highly customizable
* Declarative Build Language (expresses intent)
  * We tell Gradle _what_ we would like to happen, not _how_
  * Makes build scripts easier to read than Ant or Maven xml
  * Maintainable

### Install

```shell
$ brew cask install java
$ brew install gradle
```

### Gradle Builds

* Build file is typically named `build.gradle`
* Build file contains _tasks_, and
  * plugins
  * dependencies

#### Hello Gradle

Save as `build.gradle`:

```groovy
task hello {
  doLast {
    println "Hello, Gradle"
  }
}
```

To run this, `cd` to directly where `build.gradle` is located and run

```shell
$ gradle hello
```

#### Task Lifecycle

* Initialization phase
* Configuration phase
* doFirst (method)
* doLast (method)

#### Build Java

```groovy
apply plugin: 'java'
```

The java plugin uses a convention for directory structure

```
├── example
│   ├── build.gradle
│   └── src
│       ├── main
│       │   └── java
│       │       └── learning
│       │           └── App.java
│       └── test
│           └── java
│               └── learning
│                   └── AppTest.java
```

Running `gradle tasks` displays a list of all the tasks that the java plugin has installed.

Run `gradle build` to build the project.

Gradle output will mark a task `UP-TO-DATE` if there's nothing to do. Re-running a build will only rebuild things that have changed.

Gradle output goes to the `build` folder, example compiled classes, libs, jar files, etc.

Simple example can be run `java -cp build/classes/main com.pluralsight.Hello`

### Gradle Wrapper

Recommended way to run gradle to ensure the same version is always used on a project by everyone on the team. In the build file, add a wrapper task to _set_ the gradle version:

```groovy
task wrapper(type: Wrapper) {
  gradleVersion = '2.11'
}
```

Execute the wrapper task:

```shell
$ gradle wrapper
```

This task sets up the gradle wrapper. Generates two new files in project root, `gradlew` (shell script) and `gradlew.bat` (for windows).

Also generates a `gradle` directory in project root, containing a wrapper directory with `gradle-wrapper.jar` and `gradle-wrapper.properties`.

Now instead of running tasks via `gradle`, instead use the wrapper, `gradlew`. For example:

```
./gradlew build
```

This will first download the version of gradle specified in the wrapper, install it in your home `.gradle` directory, then run the task using that gradle version.

The gradle wrapper scripts and jar files can be checked into version control, along with the gradle wrapper task specified in the build file. This ensures that everyone who checks out the project and runs the build will do so with the same gradle version.

## Basic Gradle Tasks

Gradle DSL is written in Groovy, which is a JVM language. See [Groovy Fundamentals Course on Pluralsight](https://app.pluralsight.com/library/courses/groovy-fundamentals/table-of-contents).

### What is a Task

* Code that Gradle will execute
* Has a __lifecycle__ (different parts of the task will run at different times)
  * Initialization phase
  * Configuration phase
  * Execution phase
* Has __properties__
  * Common properties such as description, and group it belongs to
  * Custom to that task such as directory files are being copied to or from
  * Properties are typically configured during the Configuration Phase of the lifecycle
* Has __actions__ (code that executes), divided into two parts:
  * First action - code that needs to run before other code within the task
  * Last action - code that executes as part of the task
* Has __dependencies__: A task may require that another task complete before this one can execute. Gradle will work out the task dependencies and take care that tasks run in the correct order.

### Writing Simple Tasks

Groovy is object oriented. In a Gradle build script, top level object is `project`.
This object is used to define everything within build script.

To create a task:

```groovy
project.task("Task1")
```

To list all tasks

```shell
$ gradle tasks
```

Notice "Task1" is in a group called "Other tasks".

Can also define a task without `project` keyword, because Gradle knows "project" is the top level object and will delegate everything to it:

```groovy
task("Task2")
```

Another way to define a task, don't need brackets:

```groovy
task "Task3"
```

Finally, don't even need the quotes to define a task:

```groovy
task Task4
```

To add a property to a task:

```groovy
Task4.description = "My super duper awesome task"
```

Description will appear in shell when running `gradle tasks`, for example:

```
Other tasks
-----------
Task1
Task2
Task3
Task4 - My super duper awesome task
```
