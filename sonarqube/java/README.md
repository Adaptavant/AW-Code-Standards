## Sonarqube Configuration 

The SonarScanner for Gradle provides an easy way to start SonarQube analysis of a Gradle project.

The ability to execute the SonarQube analysis via a regular Gradle task makes it available anywhere Gradle is available (developer build, CI server, etc.), without the need to manually download, setup, and maintain a SonarQube Runner installation. The Gradle build already has much of the information needed for SonarQube to successfully analyze a project. By preconfiguring the analysis based on that information, the need for manual configuration is reduced significantly.

 - [Gradle](#Gradle)

 - [Maven](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-maven/)




# Gradle


The SonarScanner for Gradle provides an easy way to start SonarQube analysis of a Gradle project. 



## Prerequisites
   - Gradle versions 3+
   - At least the minimal version of Java supported by your SonarQube server is in use,`SonarQube` scanners require version `8` or `11` of the `JVM` and the SonarQube server requires version `11`


## Analyzing
To analyze first, include the scanner in your build in build.gradle:

### Using the plugin DSL above version 5

```
plugins {
  id "org.sonarqube" version "3.0"
}
```

### Using legacy plugin application below 5 
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


#### Configuration shared between subprojects can be configured in a subprojects block.

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

#### Task dependencies 

All tasks that produce output that should be included in the SonarQube analysis need to be executed before the sonarqube task runs. Typically, these are compile tasks, test tasks, and code coverage tasks.

Starting with v3.0 of the SonarScanner for Gradle, task dependencies are no longer added automatically. Instead, the SonarScanner plugin enforces the correct order of tasks with mustRunAfter. You need to be either **manually** run the tasks that produce output before sonarqube, or you can add a dependency to the build script:


```
// build.gradle

    project.tasks["sonarqube"].dependsOn "test"

```

