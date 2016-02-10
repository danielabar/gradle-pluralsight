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
brew cask install java
brew install gradle
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
gradle hello
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
