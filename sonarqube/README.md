## SonarScanner for Gradle

The SonarScanner for Gradle provides an easy way to start SonarQube analysis of a Gradle project. 

## Prerequisites
   - Gradle versions 3+
   - At least the minimal version of Java supported by your SonarQube server is in use 


## Configure the Scanner
Installation is automatic, but certain global properties should still be configured. A good place to configure global properties is `~/.gradle/gradle.properties` .

properties should be prefixed by `systemProp`.

```
    # gradle.properties
    systemProp.sonar.host.url=http://localhost:9000
 
    #----- Token generated from an account with 'publish analysis' permission
    systemProp.sonar.login=<token>

```


## Analyzing
To analyze first, include the scanner in your build in build.gradle:

### Using the plugin DSL

```
plugins {
  id "org.sonarqube" version "3.0"
}
```

### Using legacy plugin application
```
buildscript {
  repositories {
    maven {
      url "https://plugins.gradle.org/m2/"
    }
  }
  dependencies {
    classpath "org.sonarsource.scanner.gradle:sonarqube-gradle-plugin:3.0"
  }
}

apply plugin: "org.sonarqube"

```

