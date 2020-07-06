## SonarScanner for Gradle

The SonarScanner for Gradle provides an easy way to start SonarQube analysis of a Gradle project. 

## Prerequisites
   - Gradle versions 3+
   - At least the minimal version of Java supported by your SonarQube server is in use 


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
        jcenter()
    }

    dependencies {
    
        classpath "org.sonarsource.scanner.gradle:sonarqube-gradle-plugin:3.0"
    }
}

apply plugin: "org.sonarqube"

```

## Configure the Scanner
    For configuration we need to set the sonarqube properties file, as below.

```
 sonarqube {

    properties {

        property "sonar.projectKey", "fullmetrics"
        property "sonar.projectName", "fullmetrics"

        property "sonar.host.url", "http://34.93.51.4:9000/"   
        property "sonar.token", "899a10b89afb58576aee2dfcf21274d7487afcbd"  //Token of your project

    }
}

```


## Configuration shared between subprojects can be configured in a subprojects block.

```
// build.gradle

    subprojects {

    sonarqube {
        
        properties {

            property "sonar.sources", "src"

            property "sonar.java.checkstyle.reportPaths", "build/reports/checkstyle/checkstyle-main.xml"
            property "sonar.java.pmd.reportPaths", "build/reports/pmd/pmd-main.xml"
            property "sonar.java.findbugs.reportPaths", "build/reports/findbugs/findbugs-main.xml"
            property "sonar.junit.reportPaths", "build/reports/tests/test"

            }
        }
    }

```

