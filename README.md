# Gradle Tips & Tricks

Tips and tricks for Gradle https://gradle.org

## Caching Dependencies for Docker image

```groovy
task copyDeps(type: Copy) {
  from (configurations.runtime + configurations.testRuntime) exclude '*'
  into '/tmp'
}
```

Sources: https://github.com/paltamadura/microtrader/blob/master/build.gradle#L48, https://github.com/paltamadura/microtrader/blob/master/docker/test/Dockerfile#L16

## Configuring Sub-Projects

### Method 1

```groovy
configure(subprojects - project(':microtrader-common')) {
  // ...
```

Source: https://github.com/paltamadura/microtrader/blob/master/build.gradle#L74

### Method 2

```groovy
def javaProjects = subprojects.findAll {
    it.name != "ui"
}

configure(javaProjects) {
 // ...
```

Source: https://github.com/Netflix/conductor/blob/master/build.gradle#L37

## Custom Name Prefix for Sub-Projects

```groovy
rootProject.children.each {it.name="conductor-${it.name}"}
```

Source: https://github.com/Netflix/conductor/blob/master/settings.gradle#L7

## Declaring an Artifact in a Non-Java Module

Bundles a web app into a JAR and a Java service declares it as a runtime dependency. Useful for backend for frontend style full-stack app where the Java service serves a static web app.

https://github.com/xword/java-npm-gradle-integration-example/blob/master/gradle-groovy-dsl/npm-app/build.gradle

Adapted from the above: https://github.com/paltamadura/fullstack-java-react-starter/blob/master/ui/build.gradle
